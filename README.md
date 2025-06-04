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

## How to Install & Run
