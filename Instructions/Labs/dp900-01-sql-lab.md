---
lab:
  title: Azure SQL Database 살펴보기
  module: Explore relational data in Azure
---

# <a name="explore-azure-sql-database"></a>Azure SQL Database 살펴보기

이 연습에서는 Azure 구독에서 Azure SQL Database 리소스를 프로비저닝한 다음, SQL을 사용하여 관계형 데이터베이스의 테이블을 쿼리합니다.

이 랩을 완료하는 데 약 **15**분이 걸립니다.

## <a name="before-you-start"></a>시작하기 전에

관리 수준 액세스 권한이 있는 [Azure 구독](https://azure.microsoft.com/free)이 필요합니다.

## <a name="provision-an-azure-sql-database-resource"></a>Azure SQL Database 리소스 프로비저닝하기

1. In the <bpt id="p1">[</bpt>Azure portal<ept id="p1">](https://portal.azure.com?azure-portal=true)</ept>, select <bpt id="p2">**</bpt>&amp;#65291; Create a resource<ept id="p2">**</ept> from the upper left-hand corner and search for <bpt id="p3">*</bpt>Azure SQL<ept id="p3">*</ept>. Then in the resulting <bpt id="p1">**</bpt>Azure SQL<ept id="p1">**</ept> page, select <bpt id="p2">**</bpt>Create<ept id="p2">**</ept>.

1. 사용 가능한 Azure SQL 옵션을 검토한 다음 **SQL 데이터베이스** 타일에서 **단일 데이터베이스**가 선택되어 있는지 확인하고 **만들기**를 선택합니다.

    ![Azure SQL 페이지를 보여 주는 Azure Portal의 스크린샷](images//azure-sql-portal.png)

1. **SQL Database 만들기** 페이지에서 다음과 같은 값을 입력합니다.
    - **구독**: Azure 구독을 선택합니다.
    - **리소스 그룹**: 선택한 이름으로 새 리소스 그룹을 만듭니다.
    - **데이터베이스 이름**: *AdventureWorks*
    - <bpt id="p1">**</bpt>Server<ept id="p1">**</ept>:  Select <bpt id="p2">**</bpt>Create new<ept id="p2">**</ept> and create a new server with a unique name in any available location. Use <bpt id="p1">**</bpt>SQL authentication<ept id="p1">**</ept> and specify your name as the server admin login and a suitably complex password (remember the password - you'll need it later!)
    - **SQL 탄력적 풀을 사용하고 싶나요?**: 아니요
    - **컴퓨팅 + 스토리지**: 그대로 두기
    - **백업 스토리지 중복**: 로컬 중복 백업 스토리지

1. On the <bpt id="p1">**</bpt>Create SQL Database<ept id="p1">**</ept> page, select <bpt id="p2">**</bpt>Next :Networking &gt;<ept id="p2">**</ept>, and on the <bpt id="p3">**</bpt>Networking<ept id="p3">**</ept> page, in the <bpt id="p4">**</bpt>Network connectivity<ept id="p4">**</ept> section, select <bpt id="p5">**</bpt>Public endpoint<ept id="p5">**</ept>. Then select <bpt id="p1">**</bpt>Yes<ept id="p1">**</ept> for both options in the <bpt id="p2">**</bpt>Firewall rules<ept id="p2">**</ept> section to allow access to your database server from Azure services and your current client IP address.

1. **다음: 보안 >** 을 선택하고 **Microsoft Defender for SQL 사용** 옵션을 **나중에**로 설정합니다.

1. **다음: 추가 설정 >** 을 선택하고 **추가 설정** 탭에서 **기존 데이터 사용** 옵션을 **샘플**로 설정합니다. (이렇게 하면 나중에 살펴볼 수 있는 샘플 데이터베이스가 만들어집니다.)

1. **검토 + 만들기**를 선택하고 **만들기**를 선택하여 Azure SQL Database를 만듭니다.

1. Wait for deployment to complete. Then go to the resource that was deployed, which should look like this:

    ![SQL Database 페이지를 보여 주는 Azure Portal의 스크린샷](images//sql-database-portal.png)

1. 페이지 왼쪽 창에서 **쿼리 편집기(미리 보기)** 를 선택한 다음 서버에 대해 지정한 관리자 로그인과 암호를 사용하여 로그인합니다.
    
                  클라이언트 IP 주소가 허용되지 않는다는 오류 메시지가 표시되는 경우 메시지 끝에 있는 **허용 목록 IP...** 링크를 선택하여 액세스를 허용한 후 다시 로그인해 봅니다. (앞에서 컴퓨터의 클라이언트 IP 주소를 방화벽 규칙에 추가했으나, 네트워크 구성에 따라 쿼리 편집기가 다른 주소에서 연결할 수 있습니다.)
    
    쿼리 편집기의 모습은 다음과 같습니다.
    
    ![Azure Portal의 쿼리 편집기 스크린샷](images//query-editor.png)

1. **Tables** 폴더를 확장하여 데이터베이스에서 테이블을 봅니다.

1. **쿼리 1** 창에 다음 SQL 코드를 입력합니다.

    ```sql
    SELECT * FROM SalesLT.Product;
    ```

1. 쿼리 위의 **&#9655; 실행**을 선택하여 쿼리를 실행하고 결과를 봅니다. 결과에는 다음과 같이 **SalesLT.Product** 테이블의 모든 행에 대한 모든 열이 포함됩니다.

    ![쿼리 결과가 있는 쿼리 편집기를 보여 주는 Azure Portal의 스크린샷](images//sql-query-results.png)

1. SELECT 문을 다음 코드로 바꾼 다음 **&#9655; 실행**을 선택하여 새 쿼리를 실행하고 결과를 봅니다. (결과에는 **ProductID**, **Name**, **ListPrice**, **ProductCategoryID** 열만 포함됩니다.)

    ```sql
    SELECT ProductID, Name, ListPrice, ProductCategoryID
    FROM SalesLT.Product;
    ```

1. 이번에는 JOIN을 사용하여 **SalesLT.ProductCategory** 테이블에서 범주 이름을 가져오는 다음 쿼리를 실행해 봅니다.

    ```sql
    SELECT p.ProductID, p.Name AS ProductName,
            c.Name AS Category, p.ListPrice
    FROM SalesLT.Product AS p
    JOIN [SalesLT].[ProductCategory] AS c
        ON p.ProductCategoryID = c.ProductCategoryID;
    ```

1. 쿼리 편집기 창을 닫고 편집 내용을 삭제합니다.

> **팁**: Azure SQL Database 탐색을 완료하면 이 연습에서 만든 리소스 그룹을 삭제할 수 있습니다.
