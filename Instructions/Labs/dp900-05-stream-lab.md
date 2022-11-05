---
lab:
  title: Azure Stream Analytics 살펴보기
  module: Explore fundamentals of real-time analytics
---

# <a name="explore-azure-stream-analytics"></a>Azure Stream Analytics 살펴보기

이 연습에서는 Azure 구독에서 Azure Stream Analytics 작업을 프로비저닝하고 이를 사용하여 실시간 데이터 스트림을 처리합니다.

이 랩을 완료하는 데 약 **15**분이 걸립니다.

## <a name="before-you-start"></a>시작하기 전에

관리 수준 액세스 권한이 있는 [Azure 구독](https://azure.microsoft.com/free)이 필요합니다.

## <a name="create-azure-resources"></a>Azure 리소스 만들기

1. Azure 구독 자격 증명을 사용하여 [Azure Portal](https://portal.azure.com)에서 Azure 구독에 로그인합니다.

1. 페이지 위쪽의 검색 창 오른쪽에 있는 **[\>_]** 단추를 사용하여 Azure Portal에서 새 Cloud Shell을 만들고 ***Bash*** 환경을 선택하고 메시지가 표시되면 스토리지를 만듭니다. Cloud Shell은 다음과 같이 Azure Portal 아래쪽 창에 명령줄 인터페이스를 제공합니다.

    ![Cloud Shell 창이 있는 Azure Portal](./images/cloud-shell.png)

1. Azure Cloud Shell에서 다음 명령을 입력하여 이 연습에 필요한 파일을 다운로드합니다.

    ```bash
    git clone https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals dp-900
    ```

1. 명령이 완료될 때까지 기다렸다가 다음 명령을 입력하여 현재 디렉터리를 이 연습용 파일이 있는 폴더로 변경합니다.

    ```bash
    cd dp-900/streaming
    ```

1. 다음 명령을 입력하여 이 연습에 필요한 Azure 리소스를 만드는 스크립트를 실행합니다.

    ```bash
    bash setup.sh
    ```

    스크립트가 실행될 때까지 기다렸다가 다음 작업을 수행합니다.

    1. 리소스를 만드는 데 필요한 Azure CLI 확장을 설치합니다(실험적 확장에 대한 경고는 무시해도 됨).
    1. 이 연습에 제공된 Azure 리소스 그룹을 식별합니다.
    1. *Azure IoT Hub* 리소스를 만듭니다. 이것은 시뮬레이션된 디바이스에서 데이터 스트림을 받는 데 사용됩니다.
    1. *Azure Storage Account*를 만듭니다. 이것은 처리된 데이터를 저장하는 데 사용됩니다.
    1. *Azure Stream Analytics* 작업을 만듭니다. 이것은 들어오는 디바이스 데이터를 실시간으로 처리하고 그 결과를 스토리지 계정에 쓰게 됩니다.

## <a name="explore-the-azure-resources"></a>Azure 리소스 살펴보기

1. [Azure Portal](https://portal.azure.com?azure-portal=true) 홈페이지에서 **리소스 그룹**을 선택하여 구독의 리소스 그룹을 확인합니다. 여기에는 설치 스크립트로 확인된 **learn*xxxxxxxxxxxxxxxxx...** * 리소스 그룹이 포함되어야 합니다.
2. **learn*xxxxxxxxxxxxxxxxx...** * 리소스 그룹을 선택하고, 그 안에 있는 리소스를 검토합니다. 다음이 포함되어야 합니다.
    - 들어오는 디바이스 데이터를 받는 데 사용되는 **iothub*xxxxxxxxxxxxx***라는 *IoT Hub*.
    - 데이터 처리 결과가 기록되는 **store*xxxxxxxxxxxx***라는 스토리지 계정.
    - 스트리밍 데이터 처리에 사용되는 **stream*xxxxxxxxxxxxx***라는 Stream Analytics 작업.

    이 세 리소스가 모두 표시되지 않으면 표시될 때까지 **&#8635; Refresh** 단추를 클릭합니다.

    > **참고**: 학습 샌드박스를 사용하는 경우 리소스 그룹에 **cloudshell*xxxxxxxx***이라는 또 다른 스토리지 계정도 포함될 수 있습니다. 이것은 설치 스크립트 실행에 사용한 Azure Cloud Shell용 데이터 저장에 사용됩니다.

3. **stream*xxxxxxxxxxxxx*** Stream Analytics 작업을 선택하고 해당 **개요** 페이지에서 정보를 확인합니다. 다음 상세 정보에 유의합니다.
    - 이 작업은 **iotinput**이라는 입력과 **bloboutput**이라는 출력이 하나씩 있습니다. 이는 설치 스크립트에서 만든 IoT Hub 및 Storage 계정을 참조합니다.
    - 이 작업에는 **iotinput** 입력에서 데이터를 읽고 10초마다 처리되는 메시지 수를 계수하여 집계하고 그 결과를 **bloboutput** 출력에 기록하는 쿼리가 있습니다.

## <a name="use-the-resources-to-analyze-streaming-data"></a>리소스를 사용하여 스트리밍 데이터 분석

1. Stream Analytics 작업용 **개요** 페이지 상단에서 **&#9655; 시작** 단추를 선택한 다음 **작업 시작** 창에서 **시작**을 선택하여 작업을 시작합니다.
2. 스트리밍 작업이 시작되었다는 알림을 기다립니다.
3. Azure Cloud Shell로 다시 전환하고, 다음 명령을 입력하여 IoT Hub로 데이터를 전송하는 디바이스를 시뮬레이션합니다.

    ```
    bash iotdevice.sh
    ```

4. 시뮬레이션이 시작될 때까지 기다립니다. 시작은 다음과 같은 출력으로 표시됩니다.

    ```
    Device simulation in progress: 6%|#    | 7/120 [00:08<02:21, 1.26s/it]
    ```

5. 시뮬레이션이 실행될 때 Azure Portal에서 **learn*xxxxxxxxxxxxxxxxx...** * 리소스 그룹용 페이지로 돌아가 **store*xxxxxxxxxxxx*** 스토리지 계정을 선택합니다.
6. 스토리지 계정 블레이드 왼쪽의 창에서 **컨테이너** 탭을 선택합니다.
7. **데이터** 컨테이너를 엽니다.
8. **데이터** 컨테이너에서 현재 연도의 폴더를 포함하는 폴더 계층 구조를 탐색합니다. 여기에는 월, 일 및 시간의 하위 폴더가 있습니다.
9. 시간 폴더에서 생성된 파일을 확인합니다. 이름이 **0_xxxxxxxxxxxxxxxx.json**과 비슷해야 합니다.
10. 파일의 **...** 메뉴(파일 세부 정보의 오른쪽)에서 **보기/편집**을 선택하고, 파일 내용을 검토합니다. 파일 내용은 각 10초간의 JSON 기록으로 구성되어야 하며, 다음과 같이 IoT 디바이스에서 받은 메시지 수가 표시되어야 합니다.

    ```
    {"starttime":"2021-10-23T01:02:13.2221657Z","endtime":"2021-10-23T01:02:23.2221657Z","device":"iotdevice","messages":2}
    {"starttime":"2021-10-23T01:02:14.5366678Z","endtime":"2021-10-23T01:02:24.5366678Z","device":"iotdevice","messages":3}
    {"starttime":"2021-10-23T01:02:15.7413754Z","endtime":"2021-10-23T01:02:25.7413754Z","device":"iotdevice","messages":4}
    ...
    ```

11. **&#8635; 새로 고침** 단추를 사용하여 파일을 새로 고칩니다. Stream Analytics 작업은 디바이스에서 IoT Hub로 스트리밍될 때 디바이스 데이터를 실시간으로 처리하므로 추가 결과가 파일에 기록됩니다.
12. Azure Cloud Shell로 돌아가 디바이스 시뮬레이션이 완료될 때까지 기다립니다(약 3분 동안 실행되어야 함).
13. 다시 Azure Portal에서 파일을 한 번 더 새로 고쳐 시뮬레이션 중에 생성된 전체 결과 집합을 확인합니다.
14. **learn*xxxxxxxxxxxxxxxxx...** * 리소스 그룹으로 돌아가 **stream*xxxxxxxxxxxxx*** Stream Analytics 작업을 다시 엽니다.
15. Stream Analytics 작업 페이지 상단에서 **&#11036; 중지** 단추를 사용해 작업을 중지하고, 메시지가 표시되면 확인합니다.

> **참고**: 스트리밍 솔루션 탐색을 완료하면 이 연습에서 만든 리소스 그룹을 삭제합니다.
