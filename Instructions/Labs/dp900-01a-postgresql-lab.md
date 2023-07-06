---
lab:
  title: Azure Database for PostgreSQL 살펴보기
  module: Explore relational data in Azure
---

# Azure Database for PostgreSQL 살펴보기

이 연습에서는 Azure 구독에서 Azure Database for PostgreSQL 리소스를 프로비저닝합니다.

이 랩을 완료하는 데 약 **5**분이 걸립니다.

## 시작하기 전에

관리 수준 액세스 권한이 있는 [Azure 구독](https://azure.microsoft.com/free)이 필요합니다.

## Azure Database for PostgreSQL 리소스 프로비저닝하기

이 연습에서는 Azure Database for PostgreSQL 리소스를 프로비저닝해 봅니다.

1. Azure Portal의 왼쪽 상단에서 **&#65291; 리소스 만들기**를 선택하고 *Azure Database for PostgreSQL*을 검색합니다. 그런 다음 표시되는 **Azure Database for PostgreSQL** 페이지에서 **만들기**를 선택합니다.

1. 사용 가능한 Azure Database for PostgreSQL 옵션을 검토한 다음, **Azure Database for PostgreSQL** 타일에서 **유연한 서버(권장)** 를 선택한 후 **만들기**를 선택합니다.

    ![Azure Database for PostgreSQL 배포 옵션 스크린샷](images/postgresql-options.png)

1. **SQL Database 만들기** 페이지에서 다음과 같은 값을 입력합니다.
    - **구독**: Azure 구독을 선택합니다.
    - **리소스 그룹**: 선택한 이름으로 새 리소스 그룹을 만듭니다.
    - **서버 이름**: 고유한 이름을 입력합니다.
    - **지역**: 사용자에게 가까운 지역을 선택합니다.
    - **PostgreSQL 버전**: 변경하지 않은 상태로 유지합니다.
    - **워크로드 형식**: **개발**을 선택합니다.
    - **컴퓨팅 + 스토리지**: 변경하지 않은 상태로 유지합니다.
    - **가용성 영역**: 변경하지 않은 상태로 유지합니다.
    - **고가용성 사용**: 변경하지 않은 상태로 유지합니다.
    - **관리자 사용자 이름**: 본인의 이름을 입력합니다.
    - **암호** 및 **암호 확인**: 적당히 복잡한 암호여야 합니다.

1. 완료되면 **다음: 네트워킹**을 선택합니다.

1. **방화벽 규칙** 아래에서 **&#65291; 현재 클라이언트 IP 주소 추가**를 선택합니다.

1. **검토 + 만들기**를 선택하고 **만들기**를 선택하여 Azure PostgreSQL 데이터베이스를 만듭니다.

1. 배포가 완료될 때까지 기다립니다. 그런 다음 배포된 리소스로 이동합니다. 리소스의 모습은 다음과 같습니다.

    ![Azure Database for PostgreSQL 페이지를 보여 주는 Azure Portal의 스크린샷](images/postgresql-portal.png)

1. Azure Database for PostgreSQL 리소스를 관리하는 데 사용할 수 있는 옵션을 검토합니다.

> **팁**: Azure Database for PostgreSQL 탐색을 완료하면 이 연습에서 만든 리소스 그룹을 삭제할 수 있습니다.
