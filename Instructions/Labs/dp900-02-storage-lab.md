---
lab:
  title: Azure Storage 살펴보기
  module: Explore Azure Storage for non-relational data
---

# <a name="explore-azure-storage"></a>Azure Storage 살펴보기

이 연습에서는 Azure 구독에서 Azure Storage 계정을 프로비저닝하고 데이터를 저장하는 데 사용할 수 있는 다양한 방법을 살펴봅니다.

이 랩을 완료하는 데 약 **15**분이 걸립니다.

## <a name="before-you-start"></a>시작하기 전에

관리 수준 액세스 권한이 있는 [Azure 구독](https://azure.microsoft.com/free)이 필요합니다.

## <a name="provision-an-azure-storage-account"></a>Azure Storage 계정 프로비전

Azure Storage를 사용하는 첫 번째 단계는 Azure 구독에서 Azure Storage 계정을 프로비전하는 것입니다.

1. 아직 로그인하지 않은 경우 [Azure Portal](https://portal.azure.com?azure-portal=true)에 로그인합니다.
1. On the Azure portal home page, select <bpt id="p1">**</bpt>&amp;#65291; Create a resource<ept id="p1">**</ept> from the upper left-hand corner and search for <bpt id="p2">*</bpt>Storage account<ept id="p2">*</ept>. Then in the resulting <bpt id="p1">**</bpt>Storage account<ept id="p1">**</ept> page, select <bpt id="p2">**</bpt>Create<ept id="p2">**</ept>.
1. **스토리지 계정 만들기** 페이지에서 다음 값을 입력합니다.
    - <bpt id="p1">**</bpt>Subscription<ept id="p1">**</ept>: If you're using a sandbox, select <bpt id="p2">*</bpt>Concierge Subscription<ept id="p2">*</ept>. Otherwise, select your Azure subscription.
    - **리소스 그룹**: 샌드박스를 사용하고 있다면 기존 리소스 그룹을 선택합니다(이름의 예: *learn-xxxx...* ). 그렇지 않다면 원하는 이름으로 새 리소스 그룹을 만듭니다.
    - **스토리지 계정 이름**: 소문자 및 숫자를 사용하여 스토리지 계정의 고유한 이름을 입력합니다.
    - **지역**: 사용 가능한 지역을 선택합니다.
    - **성능**: 표준
    - **중복도**: LRS(로컬 중복 스토리지)

1. Select <bpt id="p1">**</bpt>Next: Advanced &gt;<ept id="p1">**</ept> and view the advanced configuration options. In particular, note that this is where you can enable hierarchical namespace to support Azure Data Lake Storage Gen2. Leave this option <bpt id="p1">**</bpt><bpt id="p2">&lt;u&gt;</bpt>unselected<ept id="p2">&lt;/u&gt;</ept><ept id="p1">**</ept> (you'll enable it later), and then select <bpt id="p3">**</bpt>Next: Networking &gt;<ept id="p3">**</ept> to view the networking options for your storage account.
1. Select <bpt id="p1">**</bpt>Next: Data protection &gt;<ept id="p1">**</ept> and then in the <bpt id="p2">**</bpt>Recovery<ept id="p2">**</ept> section, <bpt id="p3">&lt;u&gt;</bpt>de<ept id="p3">&lt;/u&gt;</ept>select all of the <bpt id="p4">**</bpt>Enable soft delete...<ept id="p4">**</ept> options. These options retain deleted files for subsequent recovery, but can cause issues later when you enable hierarchical namespace.
1. 기본 설정을 변경하지 않고 나머지 **다음 >** 페이지를 계속 진행한 다음 **검토 + 만들기** 페이지에서 선택 항목의 유효성이 검사될 때까지 기다렸다가 **만들기**를 선택하여 Azure Storage 계정을 만듭니다.
1. Wait for deployment to complete. Then go to the resource that was deployed.

## <a name="explore-blob-storage"></a>Blob 스토리지 살펴보기

이제 Azure Storage 계정이 있으므로 Blob 데이터용 컨테이너를 만들 수 있습니다.

1. `https://aka.ms/product1.json`에서 [product1.json](https://aka.ms/product1.json?azure-portal=true) JSON 파일을 다운로드하여 컴퓨터에 저장합니다(나중에 Blob 스토리지에 업로드할 것이므로 임의의 폴더에 저장할 수 있음).

                  JSON 파일이 브라우저에 표시되는 경우 페이지를 ***product1.json*** 으로 저장합니다.

1. 스토리지 컨테이너에 대한 Azure Portal 페이지의 왼쪽에 있는 **데이터 스토리지** 섹션에서 **컨테이너**를 선택합니다.
1. **컨테이너** 페이지에서 **&#65291; 컨테이너**를 선택하고 **프라이빗(익명 액세스 없음)** 의 퍼블릭 액세스 수준으로 **data**라는 새 컨테이너를 추가합니다.
1. **data** 컨테이너가 만들어지면 **컨테이너** 페이지에 나열되는지 확인합니다.
1. In the pane on the left side, in the top section, select **Storage browser **. This page provides a browser-based interface that you can use to work with the data in your storage account.
1. 스토리지 브라우저 페이지에서 **Blob 컨테이너**를 선택하고 **data** 컨테이너가 나열되는지 확인합니다.
1. **data** 컨테이너를 선택합니다. 이 컨테이너는 비어 있습니다.
1. **&#65291; 디렉터리 추가**를 선택하고 폴더에 대한 정보를 읽은 후 **products**라는 새 디렉터리를 만듭니다.
1. 스토리지 탐색기에서 현재 보기에 방금 만든 **products** 폴더의 내용이 표시되는지 확인합니다. 페이지 맨 위에 있는 “이동 경로”에 **Blob containers > data > products** 경로가 반영되는지 확인합니다.
1. 이동 경로에서 **data**를 선택하여 **data** 컨테이너로 전환합니다. 여기에 **products**라는 폴더는 포함되어 있지 <u>않습니다</u>.

    Folders in blob storage are virtual, and only exist as part of the path of a blob. Since the <bpt id="p1">**</bpt>products<ept id="p1">**</ept> folder contained no blobs, it isn't really there!

1. **&#10514; 업로드** 단추를 사용하여 **Blob 업로드** 패널을 엽니다.
1. In the <bpt id="p1">**</bpt>Upload blob<ept id="p1">**</ept> panel, select the <bpt id="p2">**</bpt>product1.json<ept id="p2">**</ept> file you saved on your local computer previously. Then in the <bpt id="p1">**</bpt>Advanced<ept id="p1">**</ept> section, in the <bpt id="p2">**</bpt>Upload to folder<ept id="p2">**</ept> box, enter <bpt id="p3">**</bpt>product_data<ept id="p3">**</ept> and select the <bpt id="p4">**</bpt>Upload<ept id="p4">**</ept> button.
1. 아직 열려 있으면 **Blob 업로드** 패널을 닫고 **data** 컨테이너에 **product_data** 가상 폴더가 만들어졌는지 확인합니다.
1. **product_data** 폴더를 선택하고 업로드한 **product1.json** Blob이 포함되어 있는지 확인합니다.
1. 왼쪽의 **데이터 스토리지** 섹션에서 **컨테이너**를 선택합니다.
1. **data** 컨테이너를 열고 방금 만든 **product_data** 폴더가 나열되는지 확인합니다.
1. Select the <bpt id="p1">**</bpt>&amp;#x2027;&amp;#x2027;&amp;#x2027;<ept id="p1">**</ept> icon at the right-end of the folder, and note that it doesn't display any options. Folders in a flat namespace blob container are virtual, and can't be managed.
1. **data** 페이지의 오른쪽 위에 있는 **X** 아이콘을 사용하여 페이지를 닫고 **컨테이너** 페이지로 돌아갑니다.

## <a name="explore-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2 살펴보기

Azure Data Lake Store Gen2 support enables you to use hierarchical folders to organize and manage access to blobs. It also enables you to use Azure blob storage to host distributed file systems for common big data analytics platforms.

1. `https://aka.ms/product2.json`에서 [product2.json](https://aka.ms/product2.json?azure-portal=true) JSON 파일을 로컬 컴퓨터로 다운로드하여 이전에 **product1.json**을 다운로드한 동일한 폴더에 저장합니다(나중에 스토리지에 업로드함).
1. 스토리지 계정에 대한 Azure Portal 페이지의 왼쪽에서 **설정** 섹션까지 아래로 스크롤하고 **Data Lake Gen2 업그레이드**를 선택합니다.
1. Azure Portal 홈페이지의 왼쪽 상단에서 **&#65291; 리소스 만들기**를 선택하고 스토리지 계정을 검색합니다.
1. 업그레이드가 완료되면 왼쪽 창의 맨 위 섹션에서 **스토리지 브라우저**를 선택하고 여전히 **product_data** 폴더가 포함된 **data** Blob 컨테이너의 루트로 다시 이동합니다.
1. **product_data** 폴더를 선택하고 이전에 업로드한 **product1.json** 파일이 여전히 포함되어 있는지 확인합니다.
1. **&#10514; 업로드** 단추를 사용하여 **Blob 업로드** 패널을 엽니다.
1. 그런 다음 결과 **스토리지 계정** 페이지에서 **만들기**를 선택합니다.
1. 아직 열려 있으면 **Blob 업로드** 패널을 닫고 **product_data** 폴더에 **product2.json** 파일이 포함되어 있는지 확인합니다.
1. 왼쪽의 **데이터 스토리지** 섹션에서 **컨테이너**를 선택합니다.
1. **data** 컨테이너를 열고 방금 만든 **product_data** 폴더가 나열되는지 확인합니다.
1. 폴더의 오른쪽 끝에 있는 **&#x2027;&#x2027;&#x2027;** 아이콘을 선택합니다. 계층 구조 네임스페이스를 사용하도록 설정하면 폴더 수준에서 폴더 이름 바꾸기, 권한 설정 등의 구성 작업을 수행할 수 있습니다.
1. **data** 페이지의 오른쪽 위에 있는 **X** 아이콘을 사용하여 페이지를 닫고 **컨테이너** 페이지로 돌아갑니다.

## <a name="explore-azure-files"></a>Azure Files 살펴보기

Azure Files는 클라우드 기반 파일 공유를 만드는 방법을 제공합니다.

1. 스토리지 컨테이너에 대한 Azure Portal 페이지의 왼쪽에 있는 **데이터 스토리지** 섹션에서 **파일 공유**를 선택합니다.
1. 파일 공유 페이지에서 **&#65291; 파일 공유**를 선택하고 **트랜잭션 최적화** 계층을 사용하여 **files**라는 새 파일 공유를 추가합니다.
1. **파일 공유**에서 새 **files** 공유를 엽니다.
1. At the top of the page, select <bpt id="p1">**</bpt>Connect<ept id="p1">**</ept>. Then in the <bpt id="p1">**</bpt>Connect<ept id="p1">**</ept> pane, note that there are tabs for common operating systems (Windows, Linux, and macOS) that contain scripts you can run to connect to the shared folder from a client computer.
1. **연결** 창을 닫은 다음 **files** 페이지를 닫아 Azure Storage 계정의 **파일 공유** 페이지로 돌아갑니다.

## <a name="explore-azure-tables"></a>Azure 테이블 살펴보기

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

    |속성 이름 | Type | 값 |
    | ------------ | ---- | ----- |
    | Name | String | 위젯 |

1. 다음 값을 사용하여 두 번째 속성을 추가합니다.

    |속성 이름 | Type | 값 |
    | ------------ | ---- | ----- |
    | 가격 | Double | 2.99 |

1. **삽입**을 선택하여 새 엔터티에 대한 행을 테이블에 삽입합니다.
1. 스토리지 브라우저에서 행이 **products** 테이블에 추가되었는지, 행이 마지막으로 수정된 시기를 나타내는 **타임스탬프** 열이 만들어졌는지 확인합니다.
1. 다음 속성을 사용하여 **products** 테이블에 다른 엔터티를 추가 합니다.

    |속성 이름 | Type | 값 |
    | ------------ | ---- | ----- |
    | PartitionKey | String | 1 |
    | RowKey | String | 2 |
    | Name | String | Kniknak |
    | 가격 | Double | 1.99 |
    | 단종 | 부울 | true |

1. 새 엔터티를 삽입한 후 중단된 제품이 포함된 행이 테이블에 표시되는지 확인합니다.

    You have manually entered data into the table using the storage browser interface. In a real scenario, application developers can use the Azure Storage Table API to build applications that read and write values to tables, making it a cost effective and scalable solution for NoSQL storage.

> **팁**: Azure Storage 탐색을 완료하면 이 연습에서 만든 리소스 그룹을 삭제할 수 있습니다.
