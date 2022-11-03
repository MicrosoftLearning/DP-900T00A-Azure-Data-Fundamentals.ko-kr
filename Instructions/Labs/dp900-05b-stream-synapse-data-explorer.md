---
lab:
  title: Azure Synapse Data Explorer 살펴보기
  module: Explore fundamentals of real-time analytics
---

# <a name="explore-azure-synapse-data-explorer"></a>Azure Synapse Data Explorer 살펴보기

이 연습에서는 Azure Synapse Data Explorer를 사용하여 시계열 데이터를 분석합니다.

이 랩을 완료하는 데 약 **25**분이 걸립니다.

## <a name="before-you-start"></a>시작하기 전에

관리 수준 액세스 권한이 있는 [Azure 구독](https://azure.microsoft.com/free)이 필요합니다.

## <a name="provision-a-synapse-analytics-workspace"></a>Synapse Analytics 작업 영역 프로비전

> **팁**: 이전 연습에서 만든 Azure Synapse 작업 영역이 여전히 있는 경우에는 이 섹션을 건너뛰고 **[데이터 탐색기 풀 만들기](#create-a-data-explorer-pool)** 로 바로 이동합니다.

1. [https://portal.azure/com](https://portal.azure.com?azure-portal=true)에서 Azure Portal을 열고 Azure 구독과 연관된 자격 증명을 사용하여 로그인합니다.

    >                 **참고**: 현재 구독을 포함하는 디렉터리에서 작업하고 있는지 확인합니다. 디렉터리는 오른쪽 상단의 사용자 ID 아래에서 확인할 수 있습니다. 그렇지 않은 경우 사용자 아이콘을 선택하고 디렉터리를 전환합니다.

1. Azure Portal의 **홈** 페이지에서 **&#65291; 리소스 만들기** 아이콘을 사용하여 새 리소스를 만듭니다.
1. *Azure Synapse Analytics*를 검색하고, 다음 설정을 사용하여 새 **Azure Synapse Analytics** 리소스를 만듭니다.
    - **구독**: ‘Azure 구독’
        - **리소스 그룹**: 적절한 이름(예: "synapse-rg")의 새 리소스 그룹을 만듭니다.
        - **관리되는 리소스 그룹**: 적절한 이름(예: "synapse-managed-rg")을 입력합니다.
    - **작업 영역 이름**: 고유한 작업 영역 이름(예: "synapse-ws-<your_name>”)을 입력합니다.
    - **지역**: 사용 가능한 지역을 선택합니다.
    - **Data Lake Storage Gen 2 선택**: 구독에서
        - **계정 이름**: 고유한 이름(예: "datalake<your_name>")의 새 계정을 만듭니다.
        - **파일 시스템 이름**: 고유한 이름(예: "fs<your_name>")의 새 파일 시스템을 만듭니다.

    >                 **참고**: Synapse Analytics 작업 영역에서는 Azure 구독에 리소스 그룹 두 개가 있어야 합니다. 한 그룹은 명시적으로 만든 리소스용이고, 다른 하나는 서비스에서 사용하는 관리되는 리소스용입니다. 데이터, 스크립트 및 기타 아티팩트를 저장하는 데이터 레이크 스토리지 계정도 필요합니다.

1. 이러한 세부 정보를 입력했다면 **검토 + 만들기**를 선택하고 **만들기**를 선택하여 작업 영역을 만듭니다.
1. 작업 영역이 생성될 때까지 기다립니다. 5분 정도 걸릴 수 있습니다.
1. 배포가 완료되면, 생성된 리소스 그룹으로 이동하여 Synapse Analytics 작업 영역 및 Data Lake 스토리지 계정이 포함되어 있는지 확인합니다.
1. Synapse 작업 영역을 선택하고, **개요** 페이지의 **Synapse Studio** 카드에서 **열기**를 선택하여 새 브라우저 탭에서 Synapse Studio를 엽니다. Synapse Studio는 Synapse Analytics 작업 영역에서 사용할 수 있는 웹 기반 인터페이스입니다.
1. Synapse Studio 왼쪽에 있는 **&rsaquo;&rsaquo;** 아이콘을 사용하여 메뉴를 확장합니다. Synapse Studio에서 리소스를 관리하고 데이터 분석 작업을 수행하는 데 사용하는 다양한 페이지가 표시됩니다.

## <a name="create-a-data-explorer-pool"></a>Data Explorer 풀 만들기

1. Synapse Studio에서 **관리** 페이지를 선택합니다.
1. **데이터 탐색기 풀** 탭을 선택한 후 **&#65291; 새로 만들기** 아이콘을 사용하여 다음과 같이 설정한 새 풀을 만듭니다.
    - **데이터 탐색기 풀 이름**: dxpool
    - **워크로드**: 컴퓨팅 최적화
    - **크기**: 매우 작음(코어 2개)
1. **다음: 추가 설정 >** 을 선택하고 **스트리밍 수집** 설정을 사용하도록 설정합니다. 이렇게 하면 데이터 탐색기가 Azure Event Hubs와 같은 스트리밍 원본에서 새 데이터를 수집할 수 있습니다.
1. **검토 후 만들기**를 선택하여 데이터 탐색기 풀을 만든 다음 배포될 때까지 기다립니다(15분 이상 걸릴 수 있으며 상태가 만들기에서 온라인으로 변경됨).

## <a name="create-a-database-and-ingest-data"></a>데이터베이스 만들기 및 데이터 수집

1. Synapse Studio에서 **데이터** 페이지를 선택합니다.
1. **작업 영역** 탭이 선택되어 있는지 확인하고 필요한 경우 페이지 왼쪽 위에 있는 **&#8635;** 아이콘을 선택하여 **데이터 탐색기 데이터베이스**가 나열되도록 보기를 새로 고칩니다.
1. **데이터 탐색기 데이터베이스**를 확장하고 **dxpool**이 나열되는지 확인합니다.
1. **데이터** 창에서 **&#65291;** 아이콘을 사용하여 **dxpool** 풀에 **iot-data**라는 새 **데이터 탐색기 데이터베이스**를 만듭니다.
1. 데이터베이스가 만들어질 때까지 기다리는 동안 [https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/data/devices.csv](https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/data/devices.csv?azure-portal=true)에서 **devices.csv**를 로컬 컴퓨터로 다운로드하여 임의의 폴더에 저장합니다.
1. Synapse Studio에서 필요한 경우 데이터베이스가 만들어질 때까지 기다린 다음, 새 **iot-data** 데이터베이스에 대한 **...** 메뉴에서 **Azure Data Explorer에서 열기**를 선택합니다.
1. Azure Data Explorer가 포함된 새 브라우저 탭의 **데이터** 탭에서 **새 데이터 수집**을 선택합니다.
1. **대상** 페이지에서 다음 설정을 선택합니다.
    - **클러스터**: Azure Synapse 작업 영역의 ***dxpool*** 데이터 탐색기 풀
    - **데이터베이스**: iot-data
    - **테이블**: **devices**라는 새 테이블을 만듭니다.
1. **다음: 원본**을 선택하고 **원본** 페이지에서 다음 옵션을 선택합니다.
    - **소스 유형**: 파일
    - **파일**: 로컬 컴퓨터에서 **devices.csv** 파일을 업로드합니다.
1. **다음: 스키마**를 선택하고 **스키마** 페이지에서 다음 설정이 올바른지 확인합니다.
    - **압축 유형**: 압축되지 않음
    - **데이터 형식**: CSV
    - **첫 번째 레코드 무시**: 선택됨
    - **매핑**: devices_mapping
1. 열 데이터 형식이 시간(datetime), 디바이스(string) 및 값(long)으로 올바르게 식별되었는지 확인합니다. 그런 다음, **다음: 수집 시작**을 선택합니다.
1. 수집이 완료되면 **닫기**를 선택합니다.
1. Azure Data Explorer의 **쿼리** 탭에서 **iot-data** 데이터베이스가 선택되어 있는지 확인하고 쿼리 창에서 다음 쿼리를 입력합니다.

    ```kusto
    devices
    ```

1. 도구 모음에서 **&#9655; 실행**을 선택하여 쿼리를 실행하고 결과를 검토합니다. 결과는 다음과 같은 형식이어야 합니다.

    | 시간 | 디바이스 | 값 |
    | --- | --- | --- |
    | 2022-01-01T00:00:00Z | Dev1 | 7 |
    | 2022-01-01T00:00:01Z | Dev2 | 4 |
    | ... | ... | ... |

    결과가 이와 일치하면 파일의 데이터에서 **devices** 테이블을 성공적으로 만든 것입니다.

    >                 **팁**: 이 예제에서는 파일에서 매우 적은 양의 일괄 처리 데이터를 가져왔는데 이 연습의 용도로는 괜찮습니다. 실제로 데이터 탐색기를 사용하여 훨씬 많은 양의 데이터를 분석할 수 있습니다. 스트림 수집을 사용하도록 설정했으므로 Azure Event Hubs와 같은 스트리밍 원본에서 테이블로 데이터를 수집하도록 데이터 탐색기를 구성할 수도 있습니다.

## <a name="use-kusto-query-language-to-query-the-table-in-synapse-studio"></a>Synapse Studio에서 Kusto 쿼리 언어를 사용하여 테이블 쿼리

1. Azure Data Explorer 브라우저 탭을 닫고 Synapse Studio가 포함된 탭으로 돌아갑니다.
1. **데이터** 페이지에서 **iot-data** 데이터베이스 및 해당 **Tables** 폴더를 확장합니다. 그런 다음 **devices** 테이블의 **...** 메뉴에서 **새 KQL 스크립트** > **1,000개 행 사용**을 선택합니다.
1. 생성된 쿼리 및 해당 결과를 검토합니다. 쿼리에는 다음 코드가 포함되어야 합니다.

    ```kusto
    devices
    | take 1000
    ```

    쿼리 결과에는 처음 1,000개의 데이터 행이 포함되어야 합니다.

1. 쿼리를 다음과 같이 수정합니다.

    ```kusto
    devices
    | where Device == 'Dev1'
    ```

1. **&#9655; 실행**을 선택하여 쿼리를 실행합니다. 그런 다음 결과를 검토합니다. *Dev1* 디바이스에 대한 행만 포함되어야 합니다.

1. 쿼리를 다음과 같이 수정합니다.

    ```kusto
    devices
    | where Device == 'Dev1'
    | where Time > datetime(2022-01-07)
    ```

1. 쿼리를 실행하고 결과를 검토합니다. 이 결과에는 2022년 1월 7일 이후의 *Dev1* 디바이스에 대한 행만 포함되어야 합니다.

1. 쿼리를 다음과 같이 수정합니다.

    ```kusto
    devices
    | where Time between (datetime(2022-01-01 00:00:00) .. datetime(2022-07-01 23:59:59))
    | summarize AvgVal = avg(Value) by Device
    | sort by Device asc
    ```

1. 쿼리를 실행하고 결과를 검토합니다. 이 결과에는 디바이스 이름 오름차순으로 2022년 1월 1일부터 1월 7일 사이에 기록된 평균 디바이스 값이 포함되어야 합니다.

1. KQL 쿼리 탭을 닫아 변경 내용을 삭제합니다.

## <a name="delete-azure-resources"></a>Azure 리소스 삭제

Azure Synapse Analytics 탐색을 완료했으므로, 지금까지 만든 리소스를 삭제하여 불필요한 Azure 비용을 방지해야 합니다.

1. 변경 내용을 저장하지 않고 Synapse Studio 브라우저 탭을 닫은 후에 Azure Portal로 돌아갑니다.
1. Azure Portal의 **홈** 페이지에서 **리소스 그룹**을 선택합니다.
1. Synapse Analytics 작업 영역의 리소스 그룹(관리되는 리소스 그룹이 아님)을 선택하고 작업 영역에 대한 Synapse 작업 영역, 스토리지 계정 및 데이터 탐색기 풀이 포함되어 있는지 확인합니다(이전 연습을 완료한 경우 Spark 풀도 포함됨).
1. 리소스 그룹의 **개요** 페이지에서 **리소스 그룹 삭제**를 선택합니다.
1. 리소스 그룹 이름을 입력하여 삭제 의사를 확인한 다음 **삭제**를 선택합니다.

    몇 분이 지나면 Azure Synapse 작업 영역과 여기에 연결된 관리되는 작업 영역이 삭제됩니다.