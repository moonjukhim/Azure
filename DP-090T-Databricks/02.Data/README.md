### Azure Data Lake

1. Storage Account 생성

   - Azure Storage Account 이동
   - Create 클릭
     - Basics
       - Subscription : [YOUR_SUBSCRIPTION]
       - Resource group : databricks-rg
       - Storage account name : formula1dl1103 **중복되지 않는 이름**
       - Region : Korea Central
       - Redundancy : LRS
     - Advanced
       - Enable Herarchical Namespace 체크(Data Lake Storage Gen2)

2. Container 생성

   - 각각 다음의 컨테이너(raw/processed/presentation/demo)를 생성
     - raw
     - processed
     - presentation
     - demo
   - raw 에 데이터 파일 업로드

3. Azure Data Lake 연결

   - Access Key
     - abfs driver
       - abfs[s]://container@storage_account_name.dfs.core.window.net/folder_name/file_name
     - spark conf
   - 2번 노트북 사용
   - Storage Account
     - Security + networking
       - Access Keys 복사

4. Azure Key Vault를 이용한 Azure Data Lake 연결

   - Azure Key Vault 생성

     - Key Vault로 이동
     - Create 클릭
       - Basics
         - Subscription : [YOUR_SUBSCRIPTION]
         - Resource group : databricks-rg
         - Key vault name : formula1-key-vault-[중복되지 않는 임의의 수]
         - Region : Korea Central
         - Pricing tier : Standard
       - Access configuration
         - Permission model : Vault access policy 선택

   - Key Vault에 Secret 생성

     - Generate/import 클릭
     - Update options : Manual
     - Name : formula1dl-account-key
     - Scret value : Storage Account의 Access Key
     - Content type : Storage Account Key
     - Create 클릭

   - Creating Scret Scope
     - Azure Portal에서 Microsoft Azure 옆에 databricks를 클릭
     - 숨겨진 databricks의 url 뒤에 다음의 내용을 입력 : **#secrets/createScope**
     - Create Secret Scope 페이지가 나옴
       - Scope Name : formula1-scope
       - Manage Principal : All users
       - DNS Name : [Vault URI]
       - Resource ID : [Resource ID]
         - Key Vault의 Properties에서 가져올 수 있음
   - Databricks Secrets Utility (2.2.access_to_data_lake 노트북)

5. Mounting Data Lake Container to Databricks

   - Service Principal을 이용하여 Mount
   - Microsoft Entra ID로 이동
   - App registrations로 이동

     - - New registration 클릭
         - Name : formula1-app
         - Support account types : 디폴트 선택
     - 생성된 formula1-app regstration 클릭
       - Application (client) ID --> 노트북에 복사
       - Directory (tenant) ID --> 노트북에 복사 (2.3.Mount.Databricks.FS)
     - Manage의 Certificates & secrets로 이동
       - - New client secret 클릭
           - Description : Formula1 App
           - Expires : 디폴트 선택
           - Add 클릭
           - 생성된 Client secret의 Value 값을 노트북에 복사 (client_secret으로 사용)

   - Key Vault에 다음의 secret 생성

     - formula1-app-client-id / value에 client_id 값 입력
     - formula1-app-tenant-id / value에 tenant_id 값 입력
     - formala1-app-client-secret / value에 client_secret 값 입력

   - 2.3.Mount.Databricks.FS 노트북 실행
