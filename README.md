# Dev Environment Setup (Windows)

Руководство по настройке окружения для работы с Claude Code + MCP + Superpowers на Windows 10/11.

> **Версия для macOS:** [dev-environment-setup](https://github.com/Afanaseva/dev-environment-setup)

---

## Содержание

1. [Предварительные требования](#1-предварительные-требования)
2. [Cursor](#2-cursor)
3. [Python](#3-python)
4. [Node.js](#4-nodejs)
5. [Git и GitHub CLI](#5-git-и-github-cli)
6. [Claude Code](#6-claude-code)
7. [Авторизация в Claude](#7-авторизация-в-claude)
8. [MCP-плагины](#8-mcp-плагины)
9. [Superpowers Skills](#9-superpowers-skills)
10. [Проверка окружения](#10-проверка-окружения)
11. [Автоматическая установка (промпты)](#11-автоматическая-установка-промпты)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. Предварительные требования

### Операционная система
- Windows 10 (версия 1903 или новее) или Windows 11
- Права администратора для установки программ

### VPN
Для авторизации в Claude необходим VPN.

**Проверка VPN:**
Откройте браузер и перейдите на https://ifconfig.me — IP должен быть не российским.

### Терминал
Все команды выполняются в **PowerShell** (рекомендуется) или **Windows Terminal**.

Для запуска PowerShell от имени администратора:
- Нажмите `Win + X` → выберите "Windows PowerShell (Администратор)" или "Терминал (Администратор)"

---

## 2. Cursor

### Шаг 1. Скачать

Перейти на официальный сайт: https://cursor.sh

Нажать **Download for Windows**.

### Шаг 2. Установить

- Запустить скачанный `.exe` файл
- Следовать инструкциям установщика (Next → Next → Install)
- Запустить Cursor после установки

### Шаг 3. Открыть терминал в Cursor

В Cursor: `Terminal → New Terminal` или `Ctrl + `` `

По умолчанию откроется PowerShell.

---

## 3. Python

### Вариант А: Установка через Microsoft Store (рекомендуется)

1. Откройте Microsoft Store
2. Найдите "Python 3.12" (или последнюю версию)
3. Нажмите "Получить" / "Install"

### Вариант Б: Установка с официального сайта

1. Перейдите на https://www.python.org/downloads/windows/
2. Скачайте последнюю версию Python 3
3. **ВАЖНО:** При установке поставьте галочку ✅ **"Add Python to PATH"**
4. Нажмите "Install Now"

### Проверка

Откройте новое окно PowerShell и выполните:

```powershell
python --version
```

Должна отобразиться версия, например: `Python 3.12.x`

---

## 4. Node.js

Node.js необходим для Claude Code.

### Установка

1. Перейдите на https://nodejs.org
2. Скачайте **LTS версию** для Windows
3. Запустите установщик и следуйте инструкциям
4. **ВАЖНО:** На шаге "Tools for Native Modules" можно пропустить (не обязательно)

### Проверка

Откройте новое окно PowerShell:

```powershell
node --version
npm --version
```

---

## 5. Git и GitHub CLI

### Установка Git

**Вариант А: Через winget (Windows 10/11)**

```powershell
winget install Git.Git
```

**Вариант Б: Официальный установщик**

1. Перейдите на https://git-scm.com/download/win
2. Скачайте и установите (все настройки по умолчанию подойдут)

### Установка GitHub CLI

**Через winget:**

```powershell
winget install GitHub.cli
```

**Или скачайте с:** https://cli.github.com/

### Проверка

```powershell
git --version
gh --version
```

> **Примечание:** Авторизация в GitHub (`gh auth login`) не требуется на этом этапе, но может понадобиться позже для работы с репозиториями.

---

## 6. Claude Code

### Установка

В PowerShell выполните:

```powershell
npm install -g @anthropic-ai/claude-code
```

### Проверка

```powershell
claude --version
```

Если версия отображается — установка успешна.

---

## 7. Авторизация в Claude

### Порядок действий

0. **Включить VPN** — это обязательно!
1. Открыть Cursor
2. **ЗАКРЫТЬ окно диалога с Cursor (панель справа) — насовсем**
3. Открыть терминал в Cursor (`Ctrl + `` `)
4. Ввести команду:

```powershell
claude
```

5. Claude спросит, есть ли у вас оплаченный аккаунт — выберите вариант **1** (Yes)
6. Откроется браузер для авторизации — войдите в свой аккаунт
7. Вернитесь в терминал — если видите приветствие с оранжевым рисунком, всё OK!

### Если не вышло

**А)** Убедитесь, что VPN включен и работает

**Б)** Если требует залогиниться заново:

```
/login
```

### Альтернатива: корпоративный прокси без VPN

Если у вас есть аккаунт на GitHub и он входит в одну из двух организаций — [sputnik-systems](https://github.com/sputnik-systems) или [sputnik-asgardos](https://github.com/sputnik-asgardos) — можно подключить Claude Code к корпоративному прокси `cc.sputnik.systems` вместо использования VPN и оплаченного Claude-аккаунта.

> **Как понять, зарегистрированы ли вы на GitHub?** Откройте <https://github.com/login>. Если сможете войти под своим логином и паролем — у вас есть аккаунт. Если логина нет — зарегистрируйтесь на <https://github.com/join>, а затем попросите в команде добавить вас в `sputnik-systems` или `sputnik-asgardos`. Проверить членство в организации заранее не обязательно — установщик ниже всё подскажет сам.

### Порядок действий

1. Открыть PowerShell
2. Разрешить запуск скриптов (один раз):

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

3. Скачать установщик: перейти в папку, куда хотите положить скрипт, и открыть в браузере <https://asgardos.ai/platform/llm-proxy/setup.ps1>. Браузер попросит войти через GitHub (нужен аккаунт в одной из организаций — `sputnik-systems` или `sputnik-asgardos`) — после входа файл скачается автоматически. Переложите `setup.ps1` из папки Загрузки в нужную директорию.

4. Разблокировать скачанный файл (Windows помечает файлы из интернета) и запустить установщик:

   ```powershell
   Unblock-File .\setup.ps1
   .\setup.ps1
   ```

5. В терминале появится короткий код (например `WDJB-MJHT`) и ссылка. Откройте ссылку, введите код, подтвердите.
6. Дождитесь строки `Готово. Попробуй: agclaude -p 'hi'`. Установщик положил `agclaude.cmd` / `agcodex.cmd` / `agopencode.cmd` в `%LOCALAPPDATA%\asgardos\bin` и записал токен + конфиг в `%APPDATA%\orchestra\` (default-модель `zai/glm-5.1`).
7. **Перезапустите PowerShell** — чтобы PATH обновился.
8. Проверьте:

   ```powershell
   agclaude -p "какую модель используешь"
   ```

Ответ должен содержать `glm-5.1` или `mimo` — прокси подключён.

> **`claude` тоже работает напрямую** — установщик настраивает `~/.claude/settings.json` с `apiKeyHelper` и `ANTHROPIC_BASE_URL`. Можно писать `claude -p "..."` вместо `agclaude -p "..."`. Токен обновляется автоматически при протухании.

### Если не вышло

**А)** `agclaude: command not found` после setup — не перезапустили PowerShell. Закройте и откройте заново. Если и после этого не находит — добавьте `%LOCALAPPDATA%\asgardos\bin` в PATH вручную.

**Б)** В браузере при входе или на device-коде пишет `access denied` — ваш GitHub-аккаунт не входит ни в `sputnik-systems`, ни в `sputnik-asgardos`. Напишите в команду, чтобы добавили в одну из них.

**В)** После `agclaude` всё равно отвечает как обычный Claude — запустите `.\setup.ps1` ещё раз (токен мог протухнуть, либо вы запускаете голый `claude` вместо `agclaude`).

**Г)** `401 unauthorized` — токен протух, повторно запустите `.\setup.ps1`. Если apiKeyHelper настроен (по умолчанию после setup), токен обновится автоматически.

**Д)** `apiKeyHelper` не срабатывает — если у вас выставлена переменная окружения `ANTHROPIC_AUTH_TOKEN`, она имеет приоритет над apiKeyHelper. Уберите её из системного окружения.

### Сменить модель

После setup default-модель автоматически подставляется во все `ag*`-команды. Сменить командой:

```powershell
agclaude --set-model mimo/mimo-v2-pro
```

Или через классы — они автоматически выбирают лучшую доступную модель:

```powershell
agclaude --set-model strong      # GLM-5.1 / MiMo-v2-pro (рекомендуется для кода)
agclaude --set-model strongest   # MiMo-v2.5-pro / GLM-5.1
agclaude --set-model fast        # GLM-5-turbo / MiMo-v2-omni
```

Или разово без сохранения:

```powershell
agclaude --model mimo/mimo-v2-pro -p "ваш промпт"
```

Доступные модели: `zai/glm-5.1`, `zai/glm-5-turbo`, `zai/glm-4.7`, `mimo/mimo-v2.5-pro`, `mimo/mimo-v2-pro`, `mimo/mimo-v2-omni`.

Полный гайд (ручной, без установщика): [sputnik-asgardos/llm-proxy](https://github.com/sputnik-asgardos/llm-proxy/blob/main/CLAUDE_CODE_SETUP.md).

### Удалить прокси-клиент

Запустите установщик с флагом `-Uninstall`:

```powershell
Unblock-File .\setup.ps1
.\setup.ps1 -Uninstall
```

Скрипт удалит: обёртки `agclaude`/`agcodex`/`agopencode`, токен и конфиг, запись из PATH, остатки старых npm-пакетов.

---

## 8. MCP-плагины

MCP-плагины расширяют возможности Claude Code.

### A) Playwright MCP

```powershell
claude mcp add --scope user playwright -- npx -y @playwright/mcp@latest
```

Проверка:

```powershell
claude mcp list | Select-String "playwright"
```

### B) Context7 MCP

```powershell
claude mcp add context7 -- npx -y @upstash/context7-mcp@latest
```

Альтернативный вариант (SSE transport):

```powershell
claude mcp add --transport sse context7 https://mcp.context7.com/sse
```

Проверка:

```powershell
claude mcp list | Select-String "context7"
```

### C) Serena MCP (универсальный LSP)

Сначала установите `uv`:

```powershell
pip install uv
```

Или через pipx:

```powershell
pip install pipx
pipx install uv
```

Установка Serena:

```powershell
claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context claude-code --project .
```

Проверка:

```powershell
claude mcp list | Select-String "serena"
```

### D) Cartographer (карта кодовой базы) — необязательный

Cartographer создаёт полную карту проекта в файле `docs/CODEBASE_MAP.md`. При старте новой сессии Claude Code читает эту карту вместо того, чтобы заново обходить весь проект — это **значительно экономит токены** на больших кодовых базах.

Репозиторий: https://github.com/kingbootoshi/cartographer

Сначала установите `tiktoken`:

```powershell
pip install tiktoken
```

Установка — в интерактивном режиме Claude Code (`claude`):

```
/plugin marketplace add kingbootoshi/cartographer
/plugin install cartographer
```

Использование:

```
/cartographer
```

> **Когда нужен:** для проектов с 20+ файлами. В маленьких проектах Claude и так быстро разбирается в структуре.

Проверка:

```powershell
# После запуска /cartographer должен появиться файл:
Test-Path docs\CODEBASE_MAP.md
```

---

## 9. Superpowers Skills

Superpowers — набор скилов для улучшения работы Claude Code.

Репозиторий: https://github.com/obra/superpowers

### Установка

Запустите интерактивный `claude` и выполните:

```
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

### Обновление

```
/plugin update superpowers
```

---

## 10. Проверка окружения

### Чек-лист

| # | Компонент | Команда проверки (PowerShell) |
|---|-----------|-------------------------------|
| 1 | VPN | Проверить IP на https://ifconfig.me |
| 2 | Cursor | Должен быть открыт |
| 3 | Python | `python --version` |
| 4 | Node.js | `node --version` и `npm --version` |
| 5 | Git | `git --version` |
| 6 | GitHub CLI | `gh --version` |
| 7 | Claude CLI | `claude --version` |
| 8 | Авторизация Claude | `claude auth status` |
| 9 | MCP-плагины | `claude mcp list` |
| 10 | Superpowers | Проверить в интерактивном режиме |

### Быстрая проверка всего

Можно попросить Claude проверить окружение, отправив ему этот запрос:

```
Проверь моё окружение для работы на Windows. Нужно проверить и вывести статус (установлено / не установлено) для следующих пунктов:

1. VPN подключение
2. Cursor
3. Python
4. Node.js
5. Git
6. GitHub CLI (gh)
7. Claude CLI
8. Наличие аккаунта Claude (авторизация)
9. Установлены ли плагины:
   – Context7
   – Playwright
   – Serena (универсальный LSP)
10. Настроенные скилы из https://github.com/obra/superpowers
```

---

## 11. Автоматическая установка (промпты)

Установка разделена на 2 этапа:
- **Промпт 1** — для Cursor (установка всего до MCP-плагинов)
- **Промпт 2** — для Claude (установка Superpowers + финальная проверка)

---

### Промпт 1: Для Cursor

Скопируйте и отправьте этот промпт в чат Cursor:

```
Ты — установщик dev-окружения для Windows. Работаешь в PowerShell на текущей машине.

Цель: подготовить базовое окружение для Claude Code + MCP:
- VPN (проверка, при необходимости попроси включить вручную)
- Python
- Node.js
- Git
- GitHub CLI (gh) — ТОЛЬКО установить, НЕ логинить в GitHub
- Claude Code/CLI (claude)
- Авторизация в Claude
- MCP-плагины: Context7, Playwright, Serena

Правила:
1) Сначала сделай диагностику и выведи краткий план (что уже есть / чего нет).
2) Затем выполняй установку/настройку по шагам.
3) Где нужны ручные действия (VPN/логин в браузере) — остановись и попроси меня сделать действие.
4) Используй winget для установки, где возможно.

ШАГ 0 — Диагностика
- Определи версию Windows:
  - [System.Environment]::OSVersion
  - Get-ComputerInfo | Select-Object WindowsVersion, OsName
- Проверь PowerShell версию:
  - $PSVersionTable.PSVersion
- Проверь VPN:
  - (Invoke-WebRequest -Uri "https://ifconfig.me/ip" -UseBasicParsing).Content
Если IP российский — попроси включить VPN вручную.

ШАГ 1 — Python
- Проверь: python --version
- Если нет: winget install Python.Python.3.12
- После установки перезапусти терминал и проверь снова

ШАГ 2 — Node.js
- Проверь: node --version
- Если нет: winget install OpenJS.NodeJS.LTS
- Проверь npm: npm --version

ШАГ 3 — Git и GitHub CLI
- Проверь: git --version
- Если нет: winget install Git.Git
- Проверь: gh --version
- Если нет: winget install GitHub.cli
НЕ авторизуй в GitHub!

ШАГ 4 — Claude Code
- npm install -g @anthropic-ai/claude-code
- Проверь: claude --version
- Авторизация: claude auth login
  (откроется браузер; дождись завершения)

ШАГ 5 — MCP-плагины

A) Playwright MCP:
- claude mcp add --scope user playwright -- npx -y @playwright/mcp@latest

B) Context7 MCP:
- claude mcp add context7 -- npx -y @upstash/context7-mcp@latest

C) Serena MCP:
- pip install uv
- claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context claude-code --project .

Проверь все плагины: claude mcp list

ШАГ 6 — Промежуточный отчёт
Выведи статус:
- VPN: подключен / не подключен (показать IP)
- Установлено: python, node/npm, git, gh, claude
- Авторизация Claude: результат claude auth status
- MCP-плагины: вывод claude mcp list

После этого напиши:
"Базовая установка завершена. Теперь запусти команду `claude` в терминале и отправь ему Промпт 2 для установки Superpowers."
```

---

### Промпт 2: Для Claude

После выполнения Промпта 1 запустите `claude` в терминале и отправьте этот промпт:

```
Установи Superpowers и проверь окружение.

ШАГ 1 — Superpowers Skills
Выполни команды:
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace

ШАГ 2 — Финальная проверка окружения
Проверь и выведи статус (установлено / не установлено) для всех пунктов:

1. VPN подключение (проверь IP)
2. Python (python --version)
3. Node.js (node --version и npm --version)
4. Git (git --version)
5. GitHub CLI (gh --version)
6. Claude CLI (claude --version)
7. Авторизация Claude (claude auth status)
8. MCP-плагины (claude mcp list):
   - Context7
   - Playwright
   - Serena
9. Superpowers Skills — установлены?

Выведи итоговую таблицу со статусами.
```

---

## 12. Troubleshooting

### VPN не работает
- Проверьте, что VPN-клиент запущен и подключен
- Проверьте IP: откройте https://ifconfig.me в браузере

### Python/Node не найден после установки
- Закройте и откройте терминал заново
- Или перезапустите Cursor
- Проверьте, что Python добавлен в PATH:
  ```powershell
  $env:PATH -split ';' | Select-String "Python"
  ```

### winget не найден
- winget доступен в Windows 10 (1903+) и Windows 11
- Обновите "App Installer" в Microsoft Store
- Или установите программы вручную с официальных сайтов

### Claude Code не запускается / команда `claude` не найдена

**2. Указание пути к Git Bash (если Git установлен в нестандартном месте)**

Claude Code использует Git Bash внутри. Если Git установлен не в стандартную папку (например, на диск `F:`), терминал не сможет его найти. Укажите путь вручную:

```powershell
$env:CLAUDE_CODE_GIT_BASH_PATH = "F:\VPN\Git\bin\bash.exe"
```

> Замените путь на тот, где у вас реально лежит `bash.exe`. Чтобы найти его:
> ```powershell
> Get-ChildItem -Path "C:\", "D:\", "F:\" -Recurse -Filter "bash.exe" -ErrorAction SilentlyContinue | Select-Object FullName
> ```

**3. Добавление Claude Code в PATH**

Если команда `claude` не найдена после установки, добавьте путь вручную:

```powershell
$env:PATH += ";$env:USERPROFILE\.local\bin"
```

> Для постоянного применения добавьте эту строку в профиль PowerShell:
> ```powershell
> Add-Content $PROFILE '$env:PATH += ";$env:USERPROFILE\.local\bin"'
> ```

### Claude не авторизуется
- Убедитесь, что VPN включен
- Попробуйте `/login` в интерактивном режиме claude
- Проверьте, что браузер по умолчанию работает корректно

### MCP-плагин не работает
- Перезапустите Claude Code
- Проверьте список плагинов:
  ```powershell
  claude mcp list
  ```

### Ошибка "execution policy" в PowerShell
Если PowerShell блокирует выполнение скриптов:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### npm выдаёт ошибки прав доступа
Попробуйте запустить PowerShell от имени администратора или используйте:

```powershell
npm config set prefix "$env:APPDATA\npm"
```

---

## Ссылки

- [Cursor](https://cursor.sh)
- [Python](https://www.python.org/downloads/windows/)
- [Node.js](https://nodejs.org)
- [Git for Windows](https://git-scm.com/download/win)
- [GitHub CLI](https://cli.github.com/)
- [Claude Code](https://www.npmjs.com/package/@anthropic-ai/claude-code)
- [Superpowers](https://github.com/obra/superpowers)

---

## Версия для macOS

Если вам нужна инструкция для macOS, перейдите в репозиторий:
https://github.com/Afanaseva/dev-environment-setup
