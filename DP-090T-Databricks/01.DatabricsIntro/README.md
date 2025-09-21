## Azure Databricks

1. Creating Azure Databricks Service

   - Azure Databricks 서비스로 이동
     - Basic
       - Subscription : **[YOUR_SUBSCRIPTION]**
       - Resource group : databricks-rg
       - Workspace name : databricks-ws
       - Region : Korea Central
       - Pricing Tier : Premium
     - Networking
       - No Public IP : No (둘다 No 선택)
     - Review + create
   - Launch Databricks workspace

2. Default Catalog Change

   - Settings
     - Advanced
       - Default Catalog에서
         - **hive_metastore** 로 변경

3. Databricks Clusters

   - Databricks의 Compute로 이동
     - Create compute 클릭
     - Simple form : OFF
     - 연필모양 아이콘을 눌러클러스터 이름을 변경 : databricks-cluster
     - Access mode : Dedicated
     - Use Photon Acceleration : Check 해제
     - Node type : Standard_DS3_V2
     - Terminate : 20

4. Databricks Notebooks

   - Workspace로 이동
     - Workspace/Worspace/User/[사용자]/
     - Create Folder : databricks
     - Create Notebook
       - 반드시 생성되어 있는 cluster에 연결하여 사용
