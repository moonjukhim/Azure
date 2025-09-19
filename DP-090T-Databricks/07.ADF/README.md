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
                                - 다음의 내용이 나오도록 **@formatDateTime(pipeline().parameters.p_window_end_date, 'yyy-MM-dd')**
                        

2. Pipeline Debug

    - Debug
        - "2021-03-21" 입력 후, Debug

3. Publish all
    - Pipelines
        - pl_ingest_formula1_data
    - Linked services
        - link_databricks_ws

4. Missing File Handling

    - 파이프라인을 더 견고하게 수정
    - Data Factory의 Pipelines의 Activivies로 이동
        - Get Metadata 검색 후, 드래그&드롭
            - General
                - Name : Get Folder Details
            - Settings
                - Dataset에서 + New 클릭
                    - Azure의 Azure Data Lake Storage Gen2 선택
                    - Select format에서 JSON 선택
                    - Continue 클릭
                    - Set properties 에서
                        - Name : ds_formula1_raw
                        - Linked service 에서 + New 클릭
                            - Name : link_formula1dl1103_storage
                            - Authentication type : Account key
                            - Storage account name : formular1df1103 선택
                            - 우측 하단의 Test connection 클릭
                            - Create 클릭
                        - File path
                            - raw / {Directory}
                                - Parameters 탭에서 + New 클릭
                                    - Name : p_window_end_date
                                        - connection 탭으로 가서 Add dynamic content에서 "p_window_end_date" 를 선택
                                            - content가 다음의 내용이 되도록 수정 **@formatDateTime(dataset().p_window_start_date, 'yyy-MM-dd')**
                                        - Ok 클릭
                            - raw/@formatDateTime(dataset().p_window_start_date, 'yyy-MM-dd') 형식까지 완성
                - Dataset에 이전의 설정이 보이며, Name : p_window_end_date, 그리고 value 값은 비어 있음
                    - 동적으로 설정이 되도록 Add dynamic content를 클릭
                        - Parameter 탭에서 p_windows_end_date 선택
                - Dataset의 아래 Field list에서 + New 클릭
                    - Argument 아래에서 Exists 선택
        - If condition 검색 후, 드래그&드롭
            - 이전 Get Metadata를 선택하고 If condition과 연결
                - General
                    - Name : If Folder Exists
                - Activities
                    - Expression 에서 Add dynamic content 클릭
                        - Get Folder Details 선택
                            - Content : **@activity('Get Folder Details').output.exists** 과 같이 되어야 함
                            - Ok 클릭
                        - True 일때 
                            - Ingestion Circuit을 오른쪽 마우스 클릭하여 Cut, 하고 True일 때 Activities로 붙여넣기
                        - False 일때 메일 등의 알림을 보내도록 고민
            - Debug를 클릭하고 존재하지 않는 "2021-03-20" 입력 후, 디버그
                - Output 내용에 존재하지 않음으로 메시지 출력
                









