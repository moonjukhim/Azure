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

    - raw/processed/presentation/demo 컨테이너 생성
    - raw 파일 업로드

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

