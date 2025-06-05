# Aerich CLI 개선 프로젝트

## 📌 소개

본 프로젝트는 [Aerich](https://github.com/tortoise/aerich) 마이그레이션 툴의 CLI(Command Line Interface)에 실용적인 기능을 추가하고, 일부 기존 코드의 오타를 수정하여 사용성과 개발 편의성을 향상시키기 위한 목적에서 시작되었습니다.

Aerich는 Tortoise-ORM 기반의 마이그레이션 도구로서, 본 프로젝트는 아래와 같은 기능적 확장을 중심으로 개선 작업을 수행했습니다.

---

## 🎯 Goal

- 기존 명령어에 대한 **사용법 및 예시 제공**
- **데이터베이스와 마이그레이션 간 차이 확인**
- **마이그레이션 흐름을 시각적으로 표현**
- **모든 마이그레이션 초기화**
- **출력 메시지의 정확도 개선 (오타 수정 포함)**

---

## 🚀 Added Commands

### `describe`

- 특정 Aerich 명령어에 대한 설명, 예시, 옵션 목록을 표시합니다.
- 사용 예:
  ```bash
  aerich describe migrate
  ```

### `status`

- 현재 DB 상태와 마이그레이션 파일을 비교하여 반영 여부를 출력합니다.
- 사용 예:
  ```bash
  aerich status
  ```

### `graph`

- 마이그레이션 파일 간의 의존 관계를 트리 형태로 시각화하여 출력합니다.
- 사용 예:
  ```bash
  aerich graph
  ```

### `reset`

- 데이터베이스를 초기화하여 모든 마이그레이션을 롤백하고, 데이터베이스에서 모든 테이블을 삭제합니다.
- 주의: 이 작업은 데이터베이스를 완전히 초기화하며 모든 데이터가 삭제됩니다.
- 사용 예:
  ```bash
  aerich reset
  ```

## ✅ Requirements

```bash
aiomysql==0.2.0
aiosqlite==0.21.0
annotated-types==0.7.0
anyio==4.9.0
asyncclick==8.1.8.0
colorama==0.4.6
dictdiffer==0.9.0
idna==3.10
iniconfig==2.1.0
iso8601==2.1.0
packaging==25.0
pluggy==1.6.0
pydantic==2.11.5
pydantic_core==2.33.2
Pygments==2.19.1
PyMySQL==1.1.1
pypika-tortoise==0.6.0
pytest==8.4.0
pytz==2025.2
setuptools==78.1.1
sniffio==1.3.1
tomli_w==1.2.0
tortoise-orm==0.25.0
typing-inspection==0.4.1
typing_extensions==4.14.0
wheel==0.45.1
```

## 🖥️ How to Install & Run

Load image from tar file

```bash
docker load < final_2021040039:v1.tar
```

Find `final_2021040039:v1` image

```bash
docker images
```

Create a container with an image

```bash
docker run -dit final_2021040039:v1
```

Find container

```bash
docker ps
```

Access to container

```bash
docker exec -it <CONTAINER_ID> /bin/bash
```

### ⌨️ How to use CLI command

**First initialize aerich**

```bash
aerich init-db
```

To use CLI command what I make

```bash
pip install -e .
```

**Use CLI command what I make**

Output descriptions and use examples for specific aerich commands

- To search `aerich_command` type `aerich -h`

```bash
aerich describe <COMMAND_IN_AERICH>
```

Compare the current database status with the latest migration file to output differences

```bash
aerich status
```

Visualize and output dependencies between migration files in tree form

```bash
aerich graph
```

Initialize the database, roll back all migrations, and delete the table

```bash
aerich reset
```

## 📌Directory Structure

```
AERICH/
├── .github/              # GitHub 관련 파일
├── aerich/               # 주요 소스 코드
│ ├── pycache/            # 파이썬 컴파일된 바이트 코드
│ ├── ddl/                # 데이터 정의 언어 관련 파일
│ ├── inspectdb/          # 데이터베이스 검사 관련 모듈
│ ├── __init__.py         # 초기화 파일
│ ├── __main__.py
│ ├── _compat.py          # 호환성 관련 코드
│ ├── cli.py              # CLI 명령어 정의
│ ├── coder.py
│ ├── enums.py            # 열거형 상수 정의
│ ├── exceptions.py       # 예외 처리 코드
│ ├── migrate.py          # 마이그레이션 관련 코드
│ ├── models.py           # 데이터 모델 정의
│ ├── utils.py
│ └── version.py
├── migrations/           # 마이그레이션 파일
│ └── models/             # 모델 관련 마이그레이션 파일
├── tests/                # 테스트 코드 관련 파일일
├── .gitignore
├── CHANGELOG.md
├── conftest.py           # 테스트 설정 관련 코드
├── LICENSE
├── Makefile
├── README.md
├── poetry.lock
└── pyproject.toml
```

**주요 파일들**

- `migrations` - 처음 DB 설정 및 migrate시 변경 내용 자동 저장
- `cli.py` - aerich에서 사용할 명령어들을 정의하는 파일일

## 🛑 How to finish and Exit

To exit

```bash
exit
```

Stop container

```bash
docker stop <CONTAINER_ID>
```

Remove container

```bash
docker rm <CONTAINER_ID>
```

Remove image

```bash
docker rmi final_2021040039:v1
```

## ©️ License

This project is licensed under the
[Apache-2.0](https://github.com/long2ice/aerich/blob/master/LICENSE) License.
