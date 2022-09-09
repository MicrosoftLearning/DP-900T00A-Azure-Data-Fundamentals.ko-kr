---
lab:
  title: Azure Cosmos DB 살펴보기
  module: Explore fundamentals of Azure Cosmos DB
---
# <a name="explore-azure-cosmos-db"></a>Azure Cosmos DB 살펴보기

이 연습에서는 Azure 구독에서 Azure Cosmos DB 데이터베이스를 프로비저닝하고 비관계형 데이터를 저장하는 데 사용할 수 있는 다양한 방법을 살펴봅니다.

이 랩을 완료하는 데 약 **15**분이 걸립니다.

## <a name="before-you-start"></a>시작하기 전에

관리 수준 액세스 권한이 있는 [Azure 구독](https://azure.microsoft.com/free)이 필요합니다.

## <a name="create-a-cosmos-db-account"></a>Cosmos DB 계정 만들기

To use Cosmos DB, you must provision a Cosmos DB account in your Azure subscription. In this exercise, you'll provision a Cosmos DB account that uses the core (SQL) API.

1. In the Azure portal, select <bpt id="p1">**</bpt>+ Create a resource<ept id="p1">**</ept> at the top left, and search for <bpt id="p2">*</bpt>Azure Cosmos DB<ept id="p2">*</ept>.  In the results, select <bpt id="p1">**</bpt>Azure Cosmos DB<ept id="p1">**</ept> and select  <bpt id="p2">**</bpt>Create<ept id="p2">**</ept>.
1. **코어(SQL) - 권장** 타일에서 **만들기**를 선택합니다.
1. 다음 세부 정보를 입력한 다음 **검토 + 만들기**를 선택합니다. 
    - <bpt id="p1">**</bpt>Subscription<ept id="p1">**</ept>: If you're using a sandbox, select <bpt id="p2">*</bpt>Concierge Subscription<ept id="p2">*</ept>. Otherwise, select your Azure subscription.
    - **리소스 그룹**: 샌드박스를 사용하고 있다면 기존 리소스 그룹을 선택합니다(이름의 예: *learn-xxxx...* ). 그렇지 않다면 원하는 이름으로 새 리소스 그룹을 만듭니다.
    - **계정 이름**: 고유한 이름을 입력합니다.
    - **위치**: 권장 위치 선택
    - **용량 모드**: 프로비저닝된 처리량
    - ** 할인 적용**: 사용 가능한 경우 적용 선택
    - **총 계정 처리량 제한**: 선택하지 않음
1. 구성의 유효성이 검사되면 **만들기**를 선택합니다.
1. Wait for deployment to complete. Then go to the deployed resource.

## <a name="create-a-sample-database"></a>예제 데이터베이스 만들기

*이 프로시저에서 포털에 표시되는 팁을 닫습니다*.

1. 새 Cosmos DB 계정 페이지 왼쪽 창에서 **데이터 탐색기**를 선택합니다.
1. **데이터 탐색기** 페이지에서 **빠른 시작 실행**을 선택합니다.
1. **새 컨테이너** 탭에서 샘플 데이터베이스에 미리 채워진 설정을 검토한 다음 **확인**을 선택합니다.
1. **SampleDB** 데이터베이스와 **SampleContainer** 컨테이너가 생성될 때까지(1분 정도 걸릴 수 있음) 화면 하단에 있는 창에서 상태를 관찰합니다.

## <a name="view-and-create-items"></a>항목 보기 및 만들기

1. In the Data Explorer page, expand the <bpt id="p1">**</bpt>SampleDB<ept id="p1">**</ept> database and the <bpt id="p2">**</bpt>SampleContainer<ept id="p2">**</ept> container, and select <bpt id="p3">**</bpt>Items<ept id="p3">**</ept> to see a list of items in the container. The items represent addresses, each with a unique id and other properties.
1. 목록에서 항목을 선택하여 항목 데이터의 JSON 표현을 확인합니다.
1. 페이지 맨 위에서 **새 항목**을 선택하여 새 빈 항목을 만듭니다.
1. 다음과 같이 새 항목에 대한 JSON을 수정한 다음 **저장**을 선택합니다.

    ```json
    {
        "address": "1 Any St.",
        "id": "123456789"
    }
    ```

1. 새 항목을 저장하면 추가 메타데이터 속성이 자동으로 추가됩니다.

## <a name="query-the-database"></a>데이터베이스 쿼리

1. **데이터 탐색기** 페이지에서 **새 SQL 쿼리** 아이콘을 선택합니다.
1. SQL 쿼리 편집기에서 기본 쿼리(`SELECT * FROM c`)를 검토하고 `SELECT * FROM c` 단추를 사용하여 실행합니다.
1. 모든 항목의 전체 JSON 표현을 포함하는 결과를 검토합니다.
1. 쿼리를 다음과 같이 수정합니다.

    ```sql
    SELECT c.id, c.address
    FROM c
    WHERE CONTAINS(c.address, "Any St.")
    ```

1. **쿼리 실행** 단추를 사용하여 수정된 쿼리를 실행하고 결과를 검토합니다. 여기에는 **주소** 필드에 "Any St."라는 텍스트가 포함된 항목의 JSON 엔터티가 포함됩니다.
1. SQL 쿼리 편집기를 닫고 변경 내용을 삭제합니다.

    You've seen how to create and query JSON entities in a Cosmos DB database by using the data explorer interface in the Azure portal. In a real scenario, an application developer would use one of the many programming language specific software development kits (SDKs) to call the core (SQL) API and work with data in the database.

> **팁**: Azure Cosmos DB 탐색을 완료하면 이 연습에서 만든 리소스 그룹을 삭제할 수 있습니다.
