# Aerich

# Aerich CLI 개선 프로젝트

## 📌 소개

본 프로젝트는 [Aerich](https://github.com/tortoise/aerich) 마이그레이션 툴의 CLI(Command Line Interface)에 실용적인 기능을 추가하고, 일부 기존 코드의 오타를 수정하여 사용성과 개발 편의성을 향상시키기 위한 목적에서 시작되었습니다.

Aerich는 Tortoise-ORM 기반의 마이그레이션 도구로서, 본 프로젝트는 아래와 같은 기능적 확장을 중심으로 개선 작업을 수행했습니다.

---

## 🎯 목표 (Goal)

- 기존 명령어에 대한 **사용법 및 예시 제공**
- **데이터베이스와 마이그레이션 간 차이 확인**
- **마이그레이션 흐름을 시각적으로 표현**
- **출력 메시지의 정확도 개선 (오타 수정 포함)**

---

## 🚀 추가된 명령어

### `describe`

- 특정 Aerich 명령어에 대한 설명, 예시, 옵션 목록을 표시합니다.
- 사용 예:
  ```bash
  aerich describe migrate
  ```

## Introduction

Aerich is a database migrations tool for TortoiseORM, which is like alembic for SQLAlchemy, or like Django ORM with
it\'s own migration solution.

## Install

Just install from pypi:

```shell
pip install "aerich[toml]"
```

## Quick Start

```shell
> aerich -h

Usage: aerich [OPTIONS] COMMAND [ARGS]...

Options:
  -V, --version      Show the version and exit.
  -c, --config TEXT  Config file.  [default: pyproject.toml]
  --app TEXT         Tortoise-ORM app name.
  -h, --help         Show this message and exit.

Commands:
  downgrade  Downgrade to specified version.
  heads      Show current available heads in migrate location.
  history    List all migrate items.
  init       Init config file and generate root migrate location.
  init-db    Generate schema and generate app migrate location.
  inspectdb  Introspects the database tables to standard output as...
  migrate    Generate migrate changes file.
  upgrade    Upgrade to specified version.
```

## Usage

You need to add `aerich.models` to your `Tortoise-ORM` config first. Example:

```python
TORTOISE_ORM = {
    "connections": {"default": "mysql://root:123456@127.0.0.1:3306/test"},
    "apps": {
        "models": {
            "models": ["tests.models", "aerich.models"],
            "default_connection": "default",
        },
    },
}
```

### Initialization

```shell
> aerich init -h

Usage: aerich init [OPTIONS]

  Init config file and generate root migrate location.

Options:
  -t, --tortoise-orm TEXT  Tortoise-ORM config module dict variable, like
                           settings.TORTOISE_ORM.  [required]
  --location TEXT          Migrate store location.  [default: ./migrations]
  -s, --src_folder TEXT    Folder of the source, relative to the project root.
  -h, --help               Show this message and exit.
```

Initialize the config file and migrations location:

```shell
> aerich init -t tests.backends.mysql.TORTOISE_ORM

Success create migrate location ./migrations
Success write config to pyproject.toml
```

_Note_: aerich will import the config file when running init-db/migrate/upgrade/heads/history commands, so it is better to keep this file simple and clean.

### Init db

```shell
> aerich init-db

Success create app migrate location ./migrations/models
Success generate schema for app "models"
```

If your Tortoise-ORM app is not the default `models`, you must specify the correct app via `--app`,
e.g. `aerich --app other_models init-db`.

### Update models and make migrate

```shell
> aerich migrate --name drop_column

Success migrate 1_202029051520102929_drop_column.py
```

Format of migrate filename is
`{version_num}_{datetime}_{name|update}.py`.

If `aerich` guesses you are renaming a column, it will ask `Rename {old_column} to {new_column} [True]`. You can choose
`True` to rename column without column drop, or choose `False` to drop the column then create. Note that the latter may
lose data.

If you need to manually write migration, you could generate empty file:

```shell
> aerich migrate --name add_index --empty

Success migrate 1_202326122220101229_add_index.py
```

### Upgrade to latest version

```shell
> aerich upgrade

Success upgrade 1_202029051520102929_drop_column.py
```

Now your db is migrated to latest.

### Downgrade to specified version

```shell
> aerich downgrade -h

Usage: aerich downgrade [OPTIONS]

  Downgrade to specified version.

Options:
  -v, --version INTEGER  Specified version, default to last.  [default: -1]
  -d, --delete           Delete version files at the same time.  [default:
                         False]

  --yes                  Confirm the action without prompting.
  -h, --help             Show this message and exit.
```

```shell
> aerich downgrade

Success downgrade 1_202029051520102929_drop_column.py
```

Now your db is rolled back to the specified version.

### Show history

```shell
> aerich history

1_202029051520102929_drop_column.py
```

### Show heads to be migrated

```shell
> aerich heads

1_202029051520102929_drop_column.py
```

### Inspect db tables to TortoiseORM model

Currently `inspectdb` support MySQL & Postgres & SQLite.

```shell
Usage: aerich inspectdb [OPTIONS]

  Introspects the database tables to standard output as TortoiseORM model.

Options:
  -t, --table TEXT  Which tables to inspect.
  -h, --help        Show this message and exit.
```

Inspect all tables and print to console:

```shell
aerich --app models inspectdb
```

Inspect a specified table in the default app and redirect to `models.py`:

```shell
aerich inspectdb -t user > models.py
```

For example, you table is:

```sql
CREATE TABLE `test`
(
    `id`       int            NOT NULL AUTO_INCREMENT,
    `decimal`  decimal(10, 2) NOT NULL,
    `date`     date                                    DEFAULT NULL,
    `datetime` datetime       NOT NULL                 DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    `time`     time                                    DEFAULT NULL,
    `float`    float                                   DEFAULT NULL,
    `string`   varchar(200) COLLATE utf8mb4_general_ci DEFAULT NULL,
    `tinyint`  tinyint                                 DEFAULT NULL,
    PRIMARY KEY (`id`),
    KEY `asyncmy_string_index` (`string`)
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4
  COLLATE = utf8mb4_general_ci
```

Now run `aerich inspectdb -t test` to see the generated model:

```python
from tortoise import Model, fields


class Test(Model):
    date = fields.DateField(null=True)
    datetime = fields.DatetimeField(auto_now=True)
    decimal = fields.DecimalField(max_digits=10, decimal_places=2)
    float = fields.FloatField(null=True)
    id = fields.IntField(primary_key=True)
    string = fields.CharField(max_length=200, null=True)
    time = fields.TimeField(null=True)
    tinyint = fields.BooleanField(null=True)
```

Note that this command is limited and can't infer some fields, such as `IntEnumField`, `ForeignKeyField`, and others.

### Multiple databases

```python
tortoise_orm = {
    "connections": {
        "default": "postgres://postgres_user:postgres_pass@127.0.0.1:5432/db1",
        "second": "postgres://postgres_user:postgres_pass@127.0.0.1:5432/db2",
    },
    "apps": {
        "models": {"models": ["tests.models", "aerich.models"], "default_connection": "default"},
        "models_second": {"models": ["tests.models_second"], "default_connection": "second", },
    },
}
```

You only need to specify `aerich.models` in one app, and must specify `--app` when running `aerich migrate` and so on, e.g. `aerich --app models_second migrate`.

## Restore `aerich` workflow

In some cases, such as broken changes from upgrade of `aerich`, you can't run `aerich migrate` or `aerich upgrade`, you
can make the following steps:

1. drop `aerich` table.
2. delete `migrations/{app}` directory.
3. rerun `aerich init-db`.

Note that these actions is safe, also you can do that to reset your migrations if your migration files is too many.

## Use `aerich` in application

You can use `aerich` out of cli by use `Command` class.

```python
from aerich import Command

async with Command(tortoise_config=config, app='models') as command:
    await command.migrate('test')
    await command.upgrade()
```

## Upgrade/Downgrade with `--fake` option

Marks the migrations up to the latest one(or back to the target one) as applied, but without actually running the SQL to change your database schema.

- Upgrade

```bash
aerich upgrade --fake
aerich --app models upgrade --fake
```

- Downgrade

```bash
aerich downgrade --fake -v 2
aerich --app models downgrade --fake -v 2
```

### Ignore tables

You can tell aerich to ignore table by setting `managed=False` in the `Meta` class, e.g.:

```py
class MyModel(Model):
    class Meta:
        managed = False
```

**Note** `managed=False` does not recognized by `tortoise-orm` and `aerich init-db`, it is only for `aerich migrate`.

## License

This project is licensed under the
[Apache-2.0](https://github.com/long2ice/aerich/blob/master/LICENSE) License.
