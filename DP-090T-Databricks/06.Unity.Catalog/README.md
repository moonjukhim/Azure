### Unity Catalog

1. 새로운 Databrics workspace 생성

    - Azure Databricks로 이동
        - + Create 클릭
            - Subscription : [YOUR_SUBSCRIPTION]
            - Resource group : databricks-uc-rg 새로 생성
            - Workspace name : databricks-uc-ws
            - Region : Korea Central
            - Pricing Tier : Premium
            - Rewview & Create 클릭

2. 새로운 Storage Account 생성

    - Storage Account로 이동
        - + Create 클릭
            - Basics 
                - Subscription : [YOUR_SUBSCRIPTION]
                - Resource group : databricks-uc-rg
                - Storage account name : databricksuc1103
                - Region : Korea Central
                - Performance : Standard
                - Redundancy : LRS
            - Advanced
                - Hierarchical Namespace : **Enable hirerarchical namespace 체크**
            - Create 클릭

3. Access connector 생성

    - Access Connector for Azure Databricks
        - + Create 클릭
            - Subscription : [YOUR_SUBSCRIPTION]
            - Resource group : databricks-uc-rg
            - Name : databricks-uc-access
            - Region : Korea Central

4. Storage Account에서 role 부여

    - Storage account로 이동
        - IAM
            - add/Add role assignment
            - Role에서 Storage Blob Data Contributor 선택
            - Members에서
                - Managed identity 선택
                - + Select members
                    - Access Connector for Azure Databrics 선택
                    - 이전에 생성한 databricks-uc-access 선택
            - Review + assign 클릭

5. Databrics Unity Catalog 생성
    
    - databricks-uc-ws 로 이동
    - launch workspace
        - 왼쪽 메뉴의 **Catalog**로 이동
        - + 버튼을 클릭하고 Create a catalog 클릭
            - Catalog name : databricks_uc_meta
            - Storage location 에서 Create a new external location 클릭
            - Create a new external location 새로운 페이지로 이동하게 됨
                - External location name : databricks_uc_meta_location
                - Storage type : Azure Data Lake Storage
                - URL을 입력하기 위해 Azure Storage Account에서 container 추가
                    - Name : metastore
                - URL : abfss://metastore@databricksuc1103.dfs.core.windows.net (본인의 Storage Account Name을 입력)
                - Storage credential 에서 입력창 클릭
                    - + Create new storage credential
                    - Access connector ID : 이전에 생성한 Connector ID의 **Resource ID 값을 찾아서 입력**
                - Create 클릭
            - Storage location 에서 앞에서 생성한 "databricks_uc_meta" 선택
            - Create 클릭
    - Databrics Cluster 생성
        - 왼쪽 메뉴의 **Compute** 로 이동
        - Create Compute 클릭
            - 윗부분에서 Simple form을 OFF로 클릭
            - Single node 선택
            - Access mode : Standard (Unity Catalog 선택 되는 부분 확인)
            - Use Photon Acceleration : 해제
            - Node type : Standard_DS3_v2
            - Terminate after **20** minutes of inactivity
            - Create compute

6. Create Unity Catalog Object

    - 왼쪽 메뉴의 Catalog로 이동
    - 앞에서 생성한 **databricks_uc_meta** 카탈로그가 존재하는지 확인
        - Catalog 메뉴에서 databricks_uc_meta를 선택하고 오른쪽 상단의 **Create schema** 클릭
        - Create a new schema 창이 뜸
            - Schema name : demo_schema
            - Storage location : databricks_uc_meta_location
        - 생성된 demo_schema 이름 옆에 **Create** 버튼을 클릭하고 Table을 선택
            - 로컬에 가지고 있는 circuits.csv를 업로드
            - 반드시 이전에 생성한 **databricks_uc_cluster** compute 선택
            - Create table 클릭
        - 테이블이 정상적으로 생성되면 카탈로그에서 demo_schema 밑에 circuits 테이블 목록을 확인할 수 있음
        - 그리고 Storage account에 데이터파일이 생성된 것을 확인할 수 있음

7. Unity Catalog's Benefits

    - Data Discovery
    - Data Audit
        - 만약 정보를 가져올 수 없다고 에러가 발생할 경우
            - 나의 Subscription으로 이동
            - Resource providers → Microsoft.Insights 검색
            - Register 클릭 (필요 시 Microsoft.OperationalInsights도 함께 등록)
            - 상태가 Registered로 바뀌면 진단 설정 화면을 다시 열기
    - Data Lineage
    - Data Access Control Overview
