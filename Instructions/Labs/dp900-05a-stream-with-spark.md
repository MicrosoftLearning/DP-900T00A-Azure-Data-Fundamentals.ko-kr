---
lab:
  title: Azure Synapse Analytics에서 Spark 스트리밍 살펴보기
  module: Explore fundamentals of real-time analytics
---

# Azure Synapse Analytics에서 Spark 스트리밍 살펴보기

이 연습에서는 Azure Synapse Analytics에서 Spark 구조적 스트리밍과 델타 테이블을 사용하여 스트리밍 데이터를 처리합니다.

이 랩을 완료하는 데 약 **15**분이 걸립니다.

## 시작하기 전에

관리 수준 액세스 권한이 있는 [Azure 구독](https://azure.microsoft.com/free)이 필요합니다.

## Synapse Analytics 작업 영역 프로비전

Synapse Analytics를 사용하려면 Azure 구독에서 Synapse Analytics 작업 영역 리소스를 프로비전해야 합니다.

1. [Azure Portal](https://portal.azure.com?azure-portal=true)에서 Azure Portal을 열고 Azure 구독과 연관된 자격 증명을 사용하여 로그인합니다.

    > **참고**: 현재 자체 구독을 포함하는 디렉터리에서 작업하고 있는지 확인합니다. 디렉터리는 오른쪽 상단의 사용자 ID 아래에서 확인할 수 있습니다. 그렇지 않은 경우 사용자 아이콘을 선택하고 디렉터리를 전환합니다.

2. Azure Portal의 **홈** 페이지에서 **&#65291; 리소스 만들기** 아이콘을 사용하여 새 리소스를 만듭니다.
3. *Azure Synapse Analytics*를 검색하고, 다음 설정을 사용하여 새 **Azure Synapse Analytics** 리소스를 만듭니다.
    - **구독**: ‘Azure 구독’
        - **리소스 그룹**: 적절한 이름(예: "synapse-rg")의 새 리소스 그룹을 만듭니다.
        - **관리되는 리소스 그룹**: 적절한 이름(예: "synapse-managed-rg")을 입력합니다.
    - **작업 영역 이름**: *고유한 작업 영역 이름(예: “synapse-ws-<your_name>”)을 입력합니다.*
    - **지역**: *사용 가능한 지역을 선택합니다*.
    - **Data Lake Storage Gen 2 선택**: 구독에서
        - **계정 이름**: *고유한 이름(예: "datalake<your_name>")의 새 계정을 만듭니다.*
        - **파일 시스템 이름**: *고유한 이름(예: "fs<your_name>")의 새 파일 시스템을 만듭니다.*

    > **참고**: Synapse Analytics 작업 영역에서는 Azure 구독에 리소스 그룹 두 개가 있어야 합니다. 한 그룹은 명시적으로 만든 리소스용이고, 다른 하나는 서비스에서 사용하는 관리되는 리소스용입니다. 데이터, 스크립트 및 기타 아티팩트를 저장하는 데이터 레이크 스토리지 계정도 필요합니다.

4. 이러한 세부 정보를 입력했다면 **검토 + 만들기**를 선택하고 **만들기**를 선택하여 작업 영역을 만듭니다.
5. 작업 영역이 생성될 때까지 기다립니다. 5분 정도 걸릴 수 있습니다.
6. 배포가 완료되면, 생성된 리소스 그룹으로 이동하여 Synapse Analytics 작업 영역 및 Data Lake 스토리지 계정이 포함되어 있는지 확인합니다.
7. Synapse 작업 영역을 선택하고, **개요** 페이지의 **Synapse Studio** 카드에서 **열기**를 선택하여 새 브라우저 탭에서 Synapse Studio를 엽니다. Synapse Studio는 Synapse Analytics 작업 영역에서 사용할 수 있는 웹 기반 인터페이스입니다.
8. Synapse Studio 왼쪽에 있는 **&rsaquo;&rsaquo;** 아이콘을 사용하여 메뉴를 확장합니다. Synapse Studio에서 리소스를 관리하고 데이터 분석 작업을 수행하는 데 사용할, 다음과 같은 다양한 페이지가 표시됩니다.

    ![Synapse Studio](images/synapse-studio.png)

## Spark 풀 만들기

Spark를 사용하여 스트리밍 데이터를 처리하려면 Azure Synapse 작업 영역에 Spark 풀을 추가해야 합니다.

1. Synapse Studio에서 **관리** 페이지를 선택합니다.
2. **Apache Spark 풀** 탭을 선택한 후 **&#65291; 새로 만들기** 아이콘을 사용하여 다음과 같이 설정한 새 Spark 풀을 만듭니다.
    - **Apache Spark 풀 이름**: sparkpool
    - **노드 크기 패밀리**: 메모리 최적화
    - **노드 크기**: 작음(가상 코어 4개/32GB)
    - **자동 크기 조정**: 사용
    - **노드 수** 3----3
3. Spark 풀을 검토하고 만든 다음, 배포될 때까지 기다립니다(몇 분 정도 걸릴 수 있습니다).

## 스트림 처리 살펴보기

Spark를 사용하여 스트림 처리를 탐색하려면 Spark 구조적 스트리밍 및 델타 테이블로 몇 가지 기본 스트림을 쉽게 처리할 수 있는 Python 코드와 메모가 포함된 Notebook을 사용합니다.

1. [Structured Streaming and Delta Tables.ipynb](https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/Spark%20Structured%20Streaming%20and%20Delta%20Tables.ipynb) Notebook을 로컬 컴퓨터로 다운로드합니다(Notebook이 텍스트 파일로 브라우저에 열린 경우에는 로컬 폴더로 저장합니다. 이때 .txt 파일이 아닌 **Structured Streaming and Delta Tables.ipynb**로 저장해야 합니다).
2. Synapse Studio에서 **개발** 페이지를 선택합니다.
3. **&#65291;** 메뉴에서 **&#8612; 가져오기**를 선택하고, 로컬 컴퓨터에서 **Structured Streaming and Delta Tables.ipynb** 파일을 선택합니다.
4. Notebook의 지침에 따라 Spark 풀에 연결하고, 포함된 코드 셀을 실행하여 스트림 처리에 Spark를 사용하는 다양한 방법을 살펴봅니다.

## Azure 리소스 삭제

> **참고**: : Azure Synapse Analytics를 사용하는 다른 연습을 완료하려는 경우에는 이 섹션을 건너뛸 수 있습니다. 그렇지 않은 경우에는 다음 단계를 따라 불필요한 Azure 비용을 방지하세요.

1. 변경 내용을 저장하지 않고 Synapse Studio 브라우저 탭을 닫은 후에 Azure Portal로 돌아갑니다.
1. Azure Portal의 **홈** 페이지에서 **리소스 그룹**을 선택합니다.
1. Synapse Analytics 작업 영역의 리소스 그룹(관리되는 리소스 그룹이 아님)을 선택하고 작업 영역에 대한 Synapse 작업 영역, 스토리지 계정 및 데이터 탐색기 풀이 포함되어 있는지 확인합니다(이전 연습을 완료한 경우 Spark 풀도 포함됨).
1. 리소스 그룹의 **개요** 페이지에서 **리소스 그룹 삭제**를 선택합니다.
1. 리소스 그룹 이름을 입력하여 삭제 의사를 확인한 다음 **삭제**를 선택합니다.

    몇 분이 지나면 Azure Synapse 작업 영역과 여기에 연결된 관리되는 작업 영역이 삭제됩니다.
