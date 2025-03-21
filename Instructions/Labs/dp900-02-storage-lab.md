---
lab:
  title: Azure Storage 살펴보기
  module: Explore Azure Storage for non-relational data
---

# Azure Storage 살펴보기

이 연습에서는 Azure 구독에서 Azure Storage 계정을 프로비저닝하고 데이터를 저장하는 데 사용할 수 있는 다양한 방법을 살펴봅니다.

이 랩을 완료하는 데 약 **15**분이 걸립니다.

## 시작하기 전에

관리 수준 액세스 권한이 있는 [Azure 구독](https://azure.microsoft.com/free)이 필요합니다.

## Azure Storage 계정 프로비전

Azure Storage를 사용하는 첫 번째 단계는 Azure 구독에서 Azure Storage 계정을 프로비전하는 것입니다.

1. 아직 로그인하지 않은 경우 [Azure Portal](https://portal.azure.com?azure-portal=true)에 로그인합니다.
1. Azure Portal 홈페이지의 왼쪽 상단에서 **＋ 리소스 만들기**를 선택하고 *스토리지 계정*을 검색합니다. 그런 다음 결과 **스토리지 계정** 페이지에서 **만들기**를 선택합니다.
1. **스토리지 계정 만들기** 페이지에서 다음 값을 입력합니다.
    - **구독**: Azure 구독을 선택합니다.
    - **리소스 그룹**: 선택한 이름으로 새 리소스 그룹을 만듭니다.
    - **스토리지 계정 이름**: 소문자 및 숫자를 사용하여 스토리지 계정의 고유한 이름을 입력합니다.
    - **지역**: 사용 가능한 지역을 선택합니다.
    - **성능**: *표준*
    - **중복도**: *LRS(로컬 중복 스토리지)*

1. **다음: 고급 >** 을 선택하고 구성 옵션을 검토합니다. 특히, Azure Data Lake Storage Gen2를 지원하려면 여기서 계층 구조 네임스페이스를 사용하도록 설정할 수 있습니다. 이 옵션을 **<u>선택 취소</u>** 된 상태로 두고(나중에 사용하도록 설정) **다음: 네트워킹 >** 을 선택하여 스토리지 계정의 네트워킹 옵션을 확인합니다.
1. **다음: 데이터 보호 >** 를 선택한 다음 **복구** 섹션에서 **일시 삭제 사용...** 옵션을 모두 선택 <u>취소</u>합니다. 이러한 옵션은 이후의 복구를 위해 삭제된 파일을 유지하지만 나중에 계층 구조 네임스페이스를 사용하도록 설정하면 문제가 발생할 수 있습니다.
1. 기본 설정을 변경하지 않고 나머지 **다음 >** 페이지를 계속 진행한 다음 **검토** 페이지에서 선택 항목의 유효성이 검사될 때까지 기다렸다가 **만들기**를 선택하여 Azure Storage 계정을 만듭니다.
1. 배포가 완료될 때까지 기다립니다. 그런 다음 배포된 리소스로 이동합니다.

## Blob 스토리지 살펴보기

이제 Azure Storage 계정이 있으므로 Blob 데이터용 컨테이너를 만들 수 있습니다.

1. `https://aka.ms/product1.json`에서 [product1.json](https://aka.ms/product1.json?azure-portal=true) JSON 파일을 다운로드하여 컴퓨터에 저장합니다(나중에 Blob 스토리지에 업로드할 것이므로 임의의 폴더에 저장할 수 있음).

    *JSON 파일이 브라우저에 표시되는 경우 페이지를 **product1.json**으로 저장합니다.*

1. 스토리지 컨테이너에 대한 Azure Portal 페이지의 왼쪽에 있는 **데이터 스토리지** 섹션에서 **컨테이너**를 선택합니다.
1. **컨테이너** 페이지에서 **&#65291; 컨테이너**를 선택하고 익명 액세스 수준이 **비공개(익명 액세스 불가)** 인 **데이터**라는 이름의 새 컨테이너를 추가합니다.
1. **data** 컨테이너가 만들어지면 **컨테이너** 페이지에 나열되는지 확인합니다.
1. 왼쪽 창의 맨 위 섹션에서 **스토리지 브라우저**를 선택합니다. 이 페이지에서는 스토리지 계정의 데이터에 대한 작업에 사용할 수 있는 브라우저 기반 인터페이스를 제공합니다.
1. 스토리지 브라우저 페이지에서 **Blob 컨테이너**를 선택하고 **data** 컨테이너가 나열되는지 확인합니다.
1. **data** 컨테이너를 선택합니다. 이 컨테이너는 비어 있습니다.
1. **&#65291; 디렉터리 추가**를 선택하고 폴더에 대한 정보를 읽은 후 **products**라는 새 디렉터리를 만듭니다.
1. 스토리지 브라우저에서 현재 보기에 방금 만든 **products** 폴더의 내용이 표시되는지 확인합니다. 페이지 맨 위에 있는 “이동 경로”에 **Blob containers > data > products** 경로가 반영되는지 확인합니다.
1. 이동 경로에서 **data**를 선택하여 **data** 컨테이너로 전환합니다. 여기에 **products**라는 폴더는 포함되어 있지 <u>않습니다</u>.

    Blob 스토리지의 폴더는 가상이며 Blob 경로의 일부로만 존재합니다. **products** 폴더에 Blob이 없으므로 실제로 존재하지 않는 것입니다.

1. **&#10514; 업로드** 단추를 사용하여 **Blob 업로드** 패널을 엽니다.
1. **Blob 업로드** 패널에서 이전에 로컬 컴퓨터에 저장한 **product1.json** 파일을 선택합니다. 그런 다음 **고급** 섹션의 **폴더에 업로드** 상자에 **product_data**를 입력하고 **업로드** 단추를 선택합니다.
1. 아직 열려 있으면 **Blob 업로드** 패널을 닫고 **data** 컨테이너에 **product_data** 가상 폴더가 만들어졌는지 확인합니다.
1. **product_data** 폴더를 선택하고 업로드한 **product1.json** Blob이 포함되어 있는지 확인합니다.
1. 왼쪽의 **데이터 스토리지** 섹션에서 **컨테이너**를 선택합니다.
1. **data** 컨테이너를 열고 방금 만든 **product_data** 폴더가 나열되는지 확인합니다.
1. 폴더의 오른쪽 끝에 있는 **&#x2027;&#x2027;&#x2027;** 아이콘을 선택합니다(아무 옵션도 표시되지 않음). 단일 구조 네임스페이스 Blob 컨테이너의 폴더는 가상이며 관리할 수 없습니다.
1. **data** 페이지의 오른쪽 위에 있는 **X** 아이콘을 사용하여 페이지를 닫고 **컨테이너** 페이지로 돌아갑니다.

## Azure Data Lake Storage Gen2 살펴보기

Azure Data Lake Store Gen2 지원을 사용하면 계층적 폴더를 사용하여 Blob에 대한 액세스를 구성하고 관리할 수 있습니다. 또한 Azure Blob Storage를 사용하여 일반적인 빅 데이터 분석 플랫폼의 분산 파일 시스템을 호스트할 수 있습니다.

1. `https://aka.ms/product2.json`에서 [product2.json](https://aka.ms/product2.json?azure-portal=true) JSON 파일을 다운로드하여 이전에 **product1.json**을 다운로드한 컴퓨터의 폴더에 저장합니다(나중에 Blob Storage에 업로드함).
1. 스토리지 계정에 대한 Azure Portal 페이지의 왼쪽에서 **설정** 섹션까지 아래로 스크롤하고 **Data Lake Gen2 업그레이드**를 선택합니다.
1. **Data Lake Gen2 업그레이드** 페이지에서 각 단계를 확장 및 완료하여 계층 구조 네임스페이스를 사용하고 Azure Data Lake Storage Gen 2를 지원하도록 스토리지 계정을 업그레이드합니다. 이 작업에는 약간의 시간이 걸릴 수 있습니다.
1. 업그레이드가 완료되면 왼쪽 창의 맨 위 섹션에서 **스토리지 브라우저**를 선택하고 여전히 **product_data** 폴더가 포함된 **data** Blob 컨테이너의 루트로 다시 이동합니다.
1. **product_data** 폴더를 선택하고 이전에 업로드한 **product1.json** 파일이 여전히 포함되어 있는지 확인합니다.
1. **&#10514; 업로드** 단추를 사용하여 **Blob 업로드** 패널을 엽니다.
1. **Blob 업로드** 패널에서 로컬 컴퓨터에 저장한 **product2.json** 파일을 선택합니다. 그런 다음 **업로드** 단추를 선택합니다.
1. 아직 열려 있으면 **Blob 업로드** 패널을 닫고 **product_data** 폴더에 **product2.json** 파일이 포함되어 있는지 확인합니다.
1. 왼쪽의 **데이터 스토리지** 섹션에서 **컨테이너**를 선택합니다.
1. **data** 컨테이너를 열고 방금 만든 **product_data** 폴더가 나열되는지 확인합니다.
1. 폴더의 오른쪽 끝에 있는 **&#x2027;&#x2027;&#x2027;** 아이콘을 선택합니다. 계층 구조 네임스페이스를 사용하도록 설정하면 폴더 수준에서 폴더 이름 바꾸기, 권한 설정 등의 구성 작업을 수행할 수 있습니다.
1. **data** 페이지의 오른쪽 위에 있는 **X** 아이콘을 사용하여 페이지를 닫고 **컨테이너** 페이지로 돌아갑니다.

## Azure Files 살펴보기

Azure Files는 클라우드 기반 파일 공유를 만드는 방법을 제공합니다.

1. 스토리지 컨테이너에 대한 Azure Portal 페이지의 왼쪽에 있는 **데이터 스토리지** 섹션에서 **파일 공유**를 선택합니다.
1. 파일 공유 페이지에서 **&#65291; 파일 공유**를 선택하고 **트랜잭션 최적화** 계층을 사용하여 **files**라는 새 파일 공유를 추가합니다.
2. **다음: 백업 >** 을 선택하고 백업을 사용 중지합니다. 그런 다음, **검토 + 생성**를 선택합니다.
1. **파일 공유**에서 새 **files** 공유를 엽니다.
1. 페이지 위쪽에서 **연결**을 선택합니다. 그러면 **연결** 창에는 클라이언트 컴퓨터에서 공유 폴더에 연결하기 위해 실행할 수 있는 스크립트가 포함된 일반 운영 체제(Windows, Linux 및 macOS)용 탭이 있습니다.
1. **연결** 창을 닫은 다음 **files** 페이지를 닫아 Azure Storage 계정의 **파일 공유** 페이지로 돌아갑니다.

## Azure Tables 살펴보기

Azure 테이블은 데이터 값을 저장해야 하지만 관계형 데이터베이스의 전체 기능 및 구조가 필요하지 않은 애플리케이션에 키/값 저장소를 제공합니다.

1. 스토리지 컨테이너에 대한 Azure Portal 페이지의 왼쪽에 있는 **데이터 스토리지** 섹션에서 **테이블**을 선택합니다.
1. **테이블** 페이지에서 **&#65291; 테이블**을 선택하고 **products**라는 새 테이블을 만듭니다.
1. **products** 테이블이 만들어졌으면 왼쪽 창의 맨 위 섹션에서 **스토리지 브라우저**를 선택합니다.
1. 스토리지 탐색기에서 **테이블**을 선택하고 **products** 테이블이 나열되는지 확인합니다.
1. **products** 테이블을 선택합니다.
1. **products** 페이지에서 **&#65291; 엔터티 추가**를 선택합니다.
1. **엔터티 추가** 패널에서 다음 키 값을 입력합니다.
    - **PartitionKey**: 1
    - **RowKey**: 1
1. **속성 추가**를 선택하고 다음 값을 사용하여 새 속성을 만듭니다.

    |Property name | Type | 값 |
    | ------------ | ---- | ----- |
    | 속성 | 문자열 | 위젯 |

1. 다음 값을 사용하여 두 번째 속성을 추가합니다.

    |Property name | Type | 값 |
    | ------------ | ---- | ----- |
    | 가격 | 두 배 | 2.99 |

1. **삽입**을 선택하여 새 엔터티에 대한 행을 테이블에 삽입합니다.
1. 스토리지 브라우저에서 행이 **products** 테이블에 추가되었는지, 행이 마지막으로 수정된 시기를 나타내는 **타임스탬프** 열이 만들어졌는지 확인합니다.
1. 다음 속성을 사용하여 **products** 테이블에 다른 엔터티를 추가 합니다.

    |Property name | Type | 값 |
    | ------------ | ---- | ----- |
    | PartitionKey | 문자열 | 1 |
    | RowKey | 문자열 | 2 |
    | 이름 | 문자열 | Kniknak |
    | 가격 | 두 배 | 1.99 |
    | 단종 | Boolean | true |

1. 새 엔터티를 삽입한 후 중단된 제품이 포함된 행이 테이블에 표시되는지 확인합니다.

    스토리지 브라우저 인터페이스를 사용하여 수동으로 테이블에 데이터를 입력했습니다. 실제 시나리오에서는 애플리케이션 개발자가 Azure Storage Table API를 사용하여 테이블에서 값을 읽고 쓰는 애플리케이션을 빌드할 수 있으므로 NoSQL 스토리지에 대한 비용 효율적이고 확장 가능한 솔루션이 될 수 있습니다.

> **팁**: Azure Storage 탐색을 완료하면 이 연습에서 만든 리소스 그룹을 삭제할 수 있습니다.
