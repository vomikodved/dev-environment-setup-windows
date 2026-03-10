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

**Рекомендуемые VPN-клиенты:**
- ProtonVPN (бесплатный тариф)
- Outline VPN
- OpenVPN

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
