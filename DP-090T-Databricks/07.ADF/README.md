### Data Factory

1. Azure Data Factory 생성하기
    - Basics
        - Subscription : [YOUR_SUBSCRIPTION]
        - Resource group : databricks-rg1
        - Name : databricks-adf-[h+임의의 숫자]
        - Region : Korea Central
    - Git configuration
        - Configure Git later : 체크
    - Review + create
        - Create 클릭
    - Launch Data Factory

2. Create Pipeline

    - Pipeline -> pipeline
    - Name : pl_ingest_formula1_data
    - Concurrency : 1
    - Activities 선택
        - Databrics/Notebook 드롭&다운
            - General
                - Name : Ingest Circuits File
            - Azure Databricks
                - Databricks linked service : + New 클릭
                - Name : link_databricks_ws
                - Connect via integration runtime : 디폴트값
                - Databricks workspace : databricks-ws
                - Select cluster : Existing interactive cluster
                - Authentication type : Managed service identity
                    - **Managed identity name 내용 기억**
                - Databricks의 IAM에서 권한 부여
                    - IAM
                        - Add -> Add role assignment
                        - Role : Priviledged administrator roles의 Contributor
                        - Members
                            - Subscription : [YOUR_SUBSCRIPTION]
                            - Managed identity : Data factory
                            - Selected members : databricks-adf-[YOURS]
                            - Select 클릭
                        - Review + assign
                        - 다시 Data Factory로 돌아가서 작업
                - Choose from existing cluster : databricks-cluster
                - 오른쪽 하단의 Test connection 클릭하여 테스트
            - Settings
                - Browse 클릭
                - Root folder/Users/[YOUR_ACCOUNT]/databricks/5.2.Ingestion.to.Delta.add.partition
                - Base parameters
                    - p_data_source : 
                    - p_file_date : 
                    - 잠깐 Activities 바깥으로 이동, 바깥 부분 클릭
                        - Variables 탭으로 이동
                            - v_data_source 의 Default Value에 : Databrics 입력
                    - Variables로 이동, 이전의 v_data_source를 선택
                    - 다시 Activities 바깥으로 이동, 바깥 부분 클릭
                        - Parameters 탭으로 이동
                            - Name : p_window_end_date
                            - Type : String
                    - Activities로 이동
                        - Base parameters 탭으로 이동
                            - p_file_date에서 Add dynamic content
                                - 다음의 내용이 나오도록 **@formatDateTime(pipeline().parameters.p_window_end_date, 'yyyy-MM-dd')**
                        
                    


                    
