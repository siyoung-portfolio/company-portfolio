# 금융 AI Agent 개발

다큐브 [홈페이지](https://daquv.com/ko)

## 사용된 기술
- Language : Python, Java 1.8, 11
- Framwork : FastAPI, Spring 2.X
- DB : PostgreSQL
- CI/CD : K8S


## 주요 개발 내용

### Python 에이전트를 Java 기반으로 마이그레이션
- 클라우드 브랜치 큐라는 사업을 하면서, 많은 고객에 대응하기 위해 자바로 마이그레이션
- Python에서 쓰던 LangChain과 Graph에서 필요한 부분들만 자바로 옮김

### SuperVisor 아키텍쳐 개발
- 기존에 하나의 워크플로우로 처리되던 아키텍쳐에서 Supervisor가 분리하여 판단하는 아키텍쳐로 변경
- 회사에서 제일 중요하게 여기는 SQL을 만들어주는 워크플로우와 분리하여 관리하기 위함

### HIL(Human-in-the-loop) 개발
- 결과물을 더 정확하게 만들어내기 위해, 사용자가 AI 에이전트 프로세스에 개입하는 HIL 개발
- 이전에 부족했던 정보를 더 정확하게 사용자에게 받아, 데이터 정확성이 올라감

### 멀티 DBMS 대응
- postgreSQL을 비롯하여 MySQL, MSSQL, Oracle 등 다양한 DBMS의 쿼리로도 포맷할 수 있게 개발
- 주로 사용된 언어는 Python이며, 정규식으로 각 DBMS에 맞게 쿼리가 수정이 되도록 개발함
