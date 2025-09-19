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
                - Hierarchical Namespace : Enable hirerarchical namespace 체크
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
        