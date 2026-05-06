# 금융 AI Agent 서비스 QUVI 개발                                                                   
                                                                                                     
  다큐브 [홈페이지](https://daquv.com/ko)
                                                                                                     
  ### 주요 성과                                                                                      
  
  > **Spider 2.0 Text-to-SQL 벤치마크 3개 부문 1위**                                                 
  >                                                                                                
  > - Spider 2.0-snow: 2026년 2월 17일 기준 1위
  > - Spider 2.0-DBT: 2025년 6월 25일 기준 1위                                                       
  > - Spider 2.0-lite: 2026년 3월 9일 기준 1위
                                                                                                     
  ## 기술 스택                                                                                     
                                                                                                     
  - Language: Python 3.12, Java 17                                                                 
  - Framework: FastAPI, Spring Boot 3.4
  - DB: PostgreSQL, MySQL, Oracle, MSSQL, BigQuery                                                   
  - SQL 변환: SQLGlot
  - AI/LLM: LangGraph, LangChain, LangQv (사내 SDK)                                                  
  - Semantic Layer: dbt 1.9, MetricFlow                                                              
  - CI/CD: GitHub Actions, GHCR, Docker                                                              
                                                                                                     
  ## 주요 개발 내용                                                                                  
                                                                                                     
  ### 멀티 DBMS SQL 변환 엔진 개발                                                                 

  - SQLGlot을 활용하여 NL2SQL 엔진이 생성한 쿼리를 각 DBMS 문법으로 변환                             
  - PostgreSQL / MySQL / Oracle / MSSQL / BigQuery 5개 DBMS 대응
  - DBMS별 날짜 함수, 문자열 리터럴, ORDER BY 등 dialect별 분기 처리                                 
  - 변환 정확성 검증을 위한 테스트 코드 작성                                                         
                                                                                                     
  ### Python AI Agent를 Java Spring Boot로 마이그레이션                                              
                                                                                                     
  - 기존 Python(LangGraph) 기반 AI Agent 워크플로우를 Java Spring Boot로 전환                        
  - **이유**: Nuitka(Python 바이너리 컴파일) 사용 시 워커 설정이 올바르게 동작하지 않았고, Java의  
  멀티스레드 처리 능력이 대규모 고객 대응에 더 적합하다고 판단                                       
  - LangGraph의 State와 Graph 구조에서 필요한 부분을 Spring Boot에 맞게 재설계                     
  - SmqStateEntry 분리, DAG 기반 워크플로우 실행기 등 핵심 컴포넌트 구현                             
                                                                                                     
  ### Supervisor 아키텍처 개발                                                                       
                                                                                                     
  - 단일 워크플로우에서 Supervisor 노드가 하위 워크플로우를 분기/판단하는 아키텍처로 전환            
  - SQL 생성 워크플로우를 독립적으로 관리하여 안정성과 확장성 확보                                 
                                                                                                     
  ### HIL(Human-in-the-Loop) 개발                                                                  
                                                                                                     
  - AI Agent 프로세스 중간에 사용자가 개입하여 부족한 정보를 보완하는 HIL 노드 개발                  
  - 사용자 피드백을 Agent State에 반영하여 데이터 정확성 향상
                                                                                                     
  ### dbt + MetricFlow 기반 Semantic Layer R&D                                                       
                                                                                                     
  - dbt Semantic Model + MetricFlow로 제조 재고 도메인 모델링 (100+ 비즈니스 차원)                   
  - BigQuery 커스텀 MetricFlow 클라이언트 구현                                                     
  - Jinja2 템플릿 기반 Ephemeral/View/Semantic YAML 자동 생성기 개발                                 
  - LangGraph 상태 머신 기반 NL → DSL → SQL → 실행 7단계 파이프라인 설계 (QUVI NL2SQL의 원형)        
                                                                                                     
  ### 사내 LLM SDK(LangQv) 개발                                                                      
                                                                                                     
  - Claude / GPT / Gemini / vLLM 4개 프로바이더를 통합하는 Java LLM SDK                              
  - 고객사 서버 설치 시 의존성 충돌을 방지하기 위해 Spring Boot 의존성 제거, 순수 Java로 개발
  - 네이티브 Tool Calling, 스트리밍, KV-Cache 최적화, 컨텍스트 오버플로우 핸들링 지원                
                                                                                                     
  ### Dolt를 활용한 Few-shot DB 버전관리                                                             
                                                                                                     
  - Dolt 셀프호스팅으로 각 프로젝트의 Few-shot 버전관리를 중앙에서 관리                              
  - API를 제공하지 않는 push 등은 서버 내 SSH 커맨드로 구현                                        
                                                                                                     
  ### Retrieval 어드민 기능 개발                                                                   
                                                                                                     
  - Milvus attu를 고객사마다 설치하는 번거로움을 해소하기 위해 자체 어드민 개발                      
  - 사내 라이브러리를 활용하여 자연어 → 임베딩 → 벡터 검색 → 랭킹 반환 파이프라인 구현
                                                                                                     
  ## 트러블 슈팅                                                                                   
                                                                                                     
  ### AICFO 푸시 메시지 중복 발송                                                                    
  
  - 푸시 메시지가 사용자에게 2번씩 중복 발송되는 문제 발견                                           
  - 서버 로그에서 동일 계정에 대한 중복 발송 확인 → 푸시 서버 2대가 동시 실행되고 있었음           
  - 2개 서버에 스케줄러 락을 적용하여 중복 발송 해결                               
