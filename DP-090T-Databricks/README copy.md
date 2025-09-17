1. Databricks 기본 환경
    - Databricks workspace 생성
        - catalog 저장소를 hive_metastore 로 변경
    - Databricks Architecture
        - Control Plane
        - Data Plane
    - Databricks Cluster
        - Single/Multi Node
    - Databricks Notebooks

2. 데이터 연결 및 보안
    - Securing Access to Azure Data Lake
        - key vault
            - Secrets
        - Databricks와 key vault를 연결 ( URL뒤에#/secrets/createScope)
    - Secrets in Cluster
        - Advanced Option에서
            - Spark config 변경
                - fs.azure.account.key.formular11103.dfs.core.windows.net {{secrets/formula1-scope/formula11103-account-key}}
    - Mount 
3. Spark Core
4. Delta Lake
    - 
5. 증분처리 및 DLT
6. Unity Catalog 및 거버넌스
7. ADF 기반 연동
    - formula1/ingestion/1.ingest_circuit_file
    - 파라미터화
        - Notebook path
            - p_data_source : 
            - p_file_date : 
8. 시각화 및 대시 보드
9. 마무리

---

Module 1: Introduction to Azure Databricks
Module 2: Training and Evaluating Machine Learning Models
Module 3: Managing Experiments and Models
Module 4: Integrating Azure Databricks and Azure Machine Learning
