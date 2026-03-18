
## Установка 1C BSL Agent Development Framework в проект

Ссылка: https://github.com/SteelMorgan/1c-agent-based-dev-framework

1. Склонировать проект 1C BSL Agent Development Framework
   
```bash
git clone https://github.com/SteelMorgan/1c-agent-based-dev-framework.git
```
2. Перейти в каталог фреймворка (НЕ проекта!)
3. Запустить терминал, выполнить:

```bash
# Запустить инсталлятор
python tools/1c-ai-agent-cli.py
```
4. Выбрать IDE/CLI, которые вы будете использовать в проекте (можно несколько сразу)
5. Выбрать каталог проекта (куда будут установлены симлинки)
6. Выбрать файлы фреймворка (навыки, правила, сабагенты и т.д.)
7. Подтвердить установку

Навыки, правила, агенты и воркфлоу подключаются отдельно, через инсталлер. Подключение — не копирование файлов фреймворка, а создание на них симлинков. Это позволяет «подсунуть» навыки в проект, физически сохраняя их в одном месте. Поменяли один навык → он изменился для всех проектов.

## Установка Language 1C (BSL)

Ссылка: https://marketplace.visualstudio.com/items?itemName=1c-syntax.language-1c-bsl

Установить расширение Language 1C (BSL) 

## Установка Markdown Preview Mermaid Support

Ссылка: https://marketplace.visualstudio.com/items?itemName=bierner.markdown-mermaid

Установить расширение Markdown Preview Mermaid Support 

## Установка 1c-batch

Ссылка: https://github.com/AlekseyUAM/1c-batch-py

1. Перейти в каталог .claude/skills/1c-batch , выполнить:

```bash
pip install .
```
2. Создать в **корне проекта** файл `.1c-devbase.json` по образцу `.1c-devbase.json.example`:

```json
{
  "platform_path": "/opt/1cv8/x86_64/8.3.27.1786/1cv8",
  "connection": {
    "type": "file",
    "path": "/home/user/Documents/MyBase"
  },
  "user": "Администратор",
  "password": "",
  "cf_dir": "src/cf",
  "cfe_dir": "src/cfe",
  "log_dir": "logs"
}
```

- Если `platform_path` не указан, платформа определяется автоматически (Linux, macOS, Windows).
- Для серверной базы: `"type": "server"`, `"server": "server-1c"`, `"base": "erp_dev"`.

## Установка 1C MCP Tools

Ссылка: https://github.com/RooLee10/1c-mcp-tools
Ссылка на версию без произвольных запросов к БД: https://github.com/vladimir-kharin/1c_mcp

> [!CAUTION]
> **Безопасность данных:** При использовании AI-моделей для запросов к базе данные передаются на внешние серверы. Убедитесь, что:
> - Не запрашиваете персональные данные (ФИО, паспорта, адреса, телефоны)
> - Используете локальные LLM для работы с конфиденциальными данными
> - Ограничили права пользователя, от имени которого выполняются запросы
> - Рассмотрите возможность доработки обфускации данных перед отправкой в LLM

1. Загрузить расширение src/cfe/mcp_tools, выключить у него безопасный режим и защиту от опасных действий
2. Опубликовать базу на веб-сервере с галочкой "Публиковать HTTP сервисы расширений по умолчанию"
3. Отредактировать default.vrd, вписать логин и пароль: 
   
```xml
ib="Srvr=&quot;localhost:1541&quot;;Ref=&quot;demo2&quot;;Usr=&quot;Администратор&quot;;Pwd=&quot;&quot;;">
```
4. Добавить в .mcp.json:

```json   
"1c-md": {
      "type": "http",
      "url": "http://localhost/my_database/hs/mcp"
    }
```    

## Установка yaxunit-runner

Ссылка: https://bia-technologies.github.io/yaxunit/

1. Загрузить расширение src/cfe/YAXUNIT, выключить у него безопасный режим и защиту от опасных действий
2. Создать файл конфигурации запуска в каталоге configs

- [Вручную](https://bia-technologies.github.io/yaxunit/docs/getting-started/run/configuration)
- [С помощью формы настройки](https://bia-technologies.github.io/yaxunit/docs/yaxunit-ui#интерфейс-настройки-конфигурации)

Пример файла - configs/YAXUNITconfig.json.example

## Установка METR - MCP 1C:Enterprise Test Runner

Ссылка: https://github.com/alkoleft/mcp-onec-test-runner

1. Создать каталог для mcp-yaxunit-runner
2. Скачать jar файл последнего релиза из https://github.com/alkoleft/mcp-onec-test-runner/releases
3. Скопировать в каталог mcp-yaxunit-runner файл configs/application.yml.example, переименовать в application.yml.
4. Скопировать в каталог mcp-yaxunit-runner файл configs/application-mcp.yml.example, **заполнить своими данными** переименовать в application-mcp.yml.
5. Добавить в .mcp.json:

```json   
"yaxunit-runner": {
      "type": "stdio",
      "command": "java",
      "args": [
        "-jar",
        "/path/to/mcp-yaxunit-runner.jar"
      ],
      "env": {
        "LOGGING_LEVEL_ROOT": "DEBUG",
        "SPRING_CONFIG_IMPORT": "/path/to/application.yml"
      }
    }
```    

## Установка mcp-bsl-lsp-bridge

Ссылка: https://github.com/SteelMorgan/mcp-bsl-lsp-bridge

1. Указать путь к платформе и выполнить:

```bash
docker run --rm -i \
  -v /path/to/1c/platform:/app/1c-platform:ro \
  ghcr.io/alkoleft/mcp-bsl-context:v0.2.2-stdio \
  --platform-path /app/1c-platform
```
2. Добавить в .mcp.json:

```json   
"lsp-bsl-bridge": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "exec",
        "-i",
        "mcp-lsp-sec",
        "mcp-lsp-bridge"
      ],
      "env": {}
    }
```    

## Установка mcp-bsl-platform-context

Ссылка: https://github.com/alkoleft/mcp-bsl-platform-context

1. Создать каталог для mcp-bsl-context
2. Скачать jar файл последнего релиза из https://github.com/alkoleft/mcp-bsl-platform-context/releases
3. Добавить в .mcp.json:

```json   
"1c-platform": {
      "command": "java",
      "args": [
        "-jar", 
        "/path/to/mcp-bsl-context.jar", 
        "--platform-path", 
        "/opt/1cv8/x86_64/8.3.27.1786"
      ]
    }
```    

## Установка 1c-copilot-proxy

Ссылка: https://github.com/SteelMorgan/spring-mcp-1c-copilot

1. Склонировать проект Spring Boot MCP Server для 1С:Напарник  
```bash
git clone https://github.com/SteelMorgan/spring-mcp-1c-copilot.git
```
2. Получить токен в https://code.1c.ai/
3. В каталоге spring-mcp-1c-copilot скопировать env и вставить токен в .env в строку ONEC_AI_TOKEN=...
```bash
cp env.example .env
```
4. Перенести токен в локальный secret-файл
```bash
./scripts/prepare-token-secret.sh
```
5. Сборка образа
```bash
docker build -f Dockerfile.build -t spring-mcp-1c-copilot .
```
6. Запуск контейнера
```bash
docker compose up -d
```