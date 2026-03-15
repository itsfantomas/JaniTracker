<div align="center">

# 🧹 JaniTracker

### *Your Character Manager for JanitorAI*

**Privacy-first, zero-config character tracker packed into a single HTML file.**

[![Version](https://img.shields.io/badge/version-1.1.2-violet?style=flat-square)](https://github.com/itsfantomas/JaniTracker)
[![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](#-license--лицензия)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react)](https://react.dev)
[![Tailwind](https://img.shields.io/badge/Tailwind_CSS-CDN-06B6D4?style=flat-square&logo=tailwindcss)](https://tailwindcss.com)
[![LocalStorage](https://img.shields.io/badge/storage-LocalStorage-orange?style=flat-square)](#)

---

> **🔗 Companion Tool: [Janitor Exporter](https://github.com/itsfantomas/janitor-exporter)**  
> A Tampermonkey userscript that lets you **bulk-export character cards with metadata** directly from JanitorAI.  
> Use it to extract `.json` card files, then **import them straight into JaniTracker** — no manual data entry needed.

---

[English](#-english) • [Русский](#-русский)

</div>

---

# 🇬🇧 English

## 📖 Overview

**JaniTracker** is a free, open-source character management tool designed for **JanitorAI** bot creators. Instead of keeping notes in random apps or spreadsheets, you get a purpose-built interface to track your characters' statuses, plan updates with checklists, organize bots with tags, and share your portfolio — all without leaving your browser.

Everything is stored **locally in your browser's LocalStorage**. No servers, no accounts, no tracking. Just open the HTML file and start working.

### How it works with Janitor Exporter

[**Janitor Exporter**](https://github.com/itsfantomas/janitor-exporter) is a companion Tampermonkey userscript that runs directly on the JanitorAI website. It adds a **"For Tracker (.json)"** button to character pages, allowing you to download lightweight JSON card files containing character metadata (name, avatar, tags, description, etc.). These exported `.json` files can then be **imported directly into JaniTracker** via the Import button, automatically populating character cards without manual data entry. This workflow enables efficient mass-migration of your entire character library into the tracker.

---

## ✨ Features

### 🕵️ Full Privacy
All data is stored **exclusively** in your browser's `LocalStorage`. Zero servers, zero registration, zero data collection. Your character data never leaves your machine.

### ⏰ Smart Update Timers
Each card displays a freshness indicator based on days since the last update:
- 🟢 **Green** — updated recently (< 30 days)
- 🟡 **Amber** — getting stale (30–60 days)
- 🔴 **Red** — critically outdated (60+ days)

### 🏷️ Flexible Tag System
Organize characters by fandom, genre, series, or any custom category. Tags are displayed as badges on each card and can be used as **interactive filters** — click a tag to show only matching characters.

### ✅ Per-Card Checklists
Each character card has a built-in task list. Track granular to-dos like *"Write First Message"*, *"Find new avatar"*, or *"Add example dialogue"*. Tasks can be toggled directly from the card without opening the editor.

### 📌 Pinning
Pin important characters to the top of the list. Pinned cards are always sorted first, regardless of the active sort order.

### 👯 Card Duplication (Clone)
Duplicate any character card with one click. The clone gets a `(Copy)` suffix and a cleared link — perfect for creating alternate universe (AU) versions.

### 🎛️ Grid & List Views
Toggle between **Grid mode** (visual cards with full details) and **List mode** (compact rows for quick scanning). The view toggle is accessible from the toolbar.

### 🌍 Bilingual Interface
Full support for **Russian** and **English**. Switch languages instantly with a single click — the entire UI, including labels, statuses, and tooltips, re-renders in the selected language.

### 🎨 Dark & Light Themes
Toggle between a dark theme (deep slate) and a light theme. All components, including cards, forms, filters, and the header, adapt seamlessly.

### 💾 Backup & Restore
Export your entire character database as a `.json` file. Import it on another device or browser to restore the full library. Single-card `.json` files (e.g., from [Janitor Exporter](https://github.com/itsfantomas/janitor-exporter)) can also be imported individually or in bulk.

### 🛡️ Backup Reminder
The app tracks the date of your last backup. If **14+ days** pass without one, a non-intrusive amber banner appears prompting you to save your data.

### 📤 Social Sharing
Generate a formatted character list (with names, links, and tags) and copy it to clipboard — ready to paste into **Discord**, **Telegram**, or any markdown-compatible platform.

---

## 🏗️ Architecture

JaniTracker is a **Single File Application (SFA)** — the entire application is contained within one `index.html` file (~42 KB). This is a deliberate design choice: no build step, no dependencies to install, no server to run. Just download and open.

### Component Hierarchy

```
index.html
├── <head>
│   ├── Tailwind CSS (CDN)
│   ├── Babel Standalone (CDN)
│   └── Custom CSS (scrollbar, loader animation)
│
└── <script type="text/babel">
    ├── translations{}        — i18n dictionary (ru/en)
    ├── <BotCard />           — Character card component (grid + list renderers)
    ├── <App />               — Root application component
    │   ├── State management  — useState + useEffect for persistence
    │   ├── Sorting & Filter  — useMemo for derived data
    │   ├── CRUD operations   — add / edit / delete / duplicate / pin
    │   ├── Import / Export   — FileReader API + Blob download
    │   └── Share             — Clipboard API
    └── <ErrorBoundary />     — Class component for graceful crash handling
```

### State Management

| State | Type | Persistence | Description |
|---|---|---|---|
| `bots` | `Array<Bot>` | `localStorage` | Primary data store for all character cards |
| `lang` | `'ru' \| 'en'` | In-memory | Current UI language |
| `theme` | `'dark' \| 'light'` | In-memory | Current color theme |
| `viewMode` | `'grid' \| 'list'` | In-memory | Current layout mode |
| `searchTerm` | `string` | In-memory | Active search filter |
| `selectedTags` | `string[]` | In-memory | Active tag filters |
| `sortType` | `string` | In-memory | Sort order key |
| `showBackupAlert` | `boolean` | Derived from `localStorage` | 14-day backup nudge |

### Bot Data Model

```json
{
  "id": "1710000000000",
  "name": "Character Name",
  "link": "https://janitorai.com/characters/...",
  "avatar": "https://...",
  "status": "active | wip | private | planned",
  "lastUpdated": "2025-01-15",
  "tags": ["Fantasy", "DC", "AU"],
  "notes": "Free-text notes for the character",
  "checklist": [
    { "id": 1710000000001, "text": "Write greeting", "done": false }
  ],
  "pinned": false
}
```

---

## 🛠️ Technology Stack

| Technology | Version | Role |
|---|---|---|
| **React** | 18.2.0 | UI rendering via ESM imports from `esm.sh` |
| **Babel Standalone** | 7.28.6 | In-browser JSX transformation |
| **Tailwind CSS** | CDN (latest) | Utility-first styling |
| **Lucide React** | 0.263.1 | Icon library (25+ icons used) |
| **LocalStorage API** | — | Client-side persistent storage |
| **FileReader API** | — | JSON file import |
| **Blob API** | — | JSON file export/download |
| **Clipboard API** | — | Copy-to-clipboard for sharing |

---

## 🚀 Getting Started

### Prerequisites
A modern web browser (Chrome, Firefox, Edge, Safari).  
**That's it.** No Node.js, no package manager, no build tools required.

### Usage
1. Download `index.html` from this repository.
2. Open it in your browser.
3. Start adding your characters.

### Data Transfer
To move your bot list between devices:
1. Click **"Backup"** in the footer — a `.json` file is downloaded.
2. Transfer the file to the target device.
3. Open JaniTracker on the target device, click **"Import"**, and select the file.

### Bulk Import from JanitorAI
1. Install [**Janitor Exporter**](https://github.com/itsfantomas/janitor-exporter) as a Tampermonkey userscript.
2. On JanitorAI, navigate to each character's page and click **"For Tracker (.json)"**.
3. In JaniTracker, click **"Import"** and select all downloaded `.json` files — they will be merged into your existing list.

---

## 📋 Changelog

### v1.1.2
- 📌 **Pinning** — pin cards to the top of the list
- 👯 **Card Duplication** — clone characters with one click
- 🎛️ **Grid / List toggle** — switch between card and compact views
- 🛡️ **Backup reminder** — 14-day alert banner

### v1.1.1
- Single-card JSON import (merge individual characters into existing list)

### v1.0.0
- Initial release: CRUD, tags, checklists, search, sorting, i18n, themes, backup/import, sharing

---

---

# 🇷🇺 Русский

## 📖 Описание

**JaniTracker** — бесплатный инструмент с открытым исходным кодом для авторов ботов на **JanitorAI**. Вместо того чтобы разбрасывать информацию по заметкам и таблицам, вы получаете специализированный интерфейс: отслеживайте статусы, планируйте обновления через чек-листы, организуйте ботов тегами и делитесь портфолио — всё, не покидая браузера.

Данные хранятся **локально в LocalStorage вашего браузера**. Никаких серверов, аккаунтов, трекинга. Просто откройте HTML-файл и работайте.

### Совместная работа с Janitor Exporter

[**Janitor Exporter**](https://github.com/itsfantomas/janitor-exporter) — это скрипт-компаньон для Tampermonkey, который работает прямо на сайте JanitorAI. Он добавляет кнопку **«For Tracker (.json)»** на страницы персонажей, позволяя скачать легковесные JSON-карточки с метаданными (имя, аватар, теги, описание и т.д.). Эти файлы можно **импортировать напрямую в JaniTracker** через кнопку «Импорт», автоматически заполняя карточки без ручного ввода. Этот рабочий процесс обеспечивает массовый перенос всей библиотеки персонажей в трекер.

---

## ✨ Возможности

### 🕵️ Полная приватность
Все данные хранятся **исключительно** в `LocalStorage` вашего браузера. Никаких серверов, регистрации, сбора данных. Ваши персонажи никуда не уходят.

### ⏰ Умные таймеры обновлений
Каждая карточка показывает индикатор свежести по количеству дней с последнего обновления:
- 🟢 **Зелёный** — обновлено недавно (< 30 дней)
- 🟡 **Жёлтый** — пора обновить (30–60 дней)
- 🔴 **Красный** — критически устарело (60+ дней)

### 🏷️ Гибкая система тегов
Организуйте персонажей по фэндомам, жанрам, сериям или любым категориям. Теги отображаются бейджиками на карточках и работают как **интерактивные фильтры** — нажмите на тег, чтобы показать только совпадающих персонажей.

### ✅ Чек-листы в каждой карточке
У каждого персонажа есть встроенный список задач. Отслеживайте детальные задачи: *«Прописать First Message»*, *«Найти новую аву»*, *«Добавить примеры диалогов»*. Задачи можно отмечать прямо с карточки без открытия редактора.

### 📌 Закрепление
Закрепляйте важных персонажей вверху списка. Закреплённые карточки всегда сортируются первыми, вне зависимости от выбранного порядка сортировки.

### 👯 Дублирование карточек
Дублируйте любую карточку одним кликом. Копия получает суффикс `(Copy)` и очищенную ссылку — идеально для создания альтернативных версий (AU).

### 🎛️ Плитка и Список
Переключайтесь между **Плиткой** (карточки с полной информацией) и **Списком** (компактные строки для быстрого просмотра). Переключатель доступен с панели инструментов.

### 🌍 Мультиязычность
Полная поддержка **русского** и **английского** языков. Мгновенное переключение одним кликом — весь интерфейс перерисовывается на выбранном языке.

### 🎨 Тёмная и светлая темы
Переключайтесь между тёмной темой (глубокий slate) и светлой. Все элементы — карточки, формы, фильтры, шапка — адаптируются бесшовно.

### 💾 Бэкап и восстановление
Экспортируйте всю базу персонажей в `.json`-файл. Импортируйте его на другом устройстве или в другом браузере для полного восстановления. Одиночные `.json`-файлы карточек (например, из [Janitor Exporter](https://github.com/itsfantomas/janitor-exporter)) также можно импортировать по одному или массово.

### 🛡️ Напоминание о бэкапе
Приложение отслеживает дату последнего бэкапа. Если прошло **14+ дней**, появляется ненавязчивая оранжевая полоска с предложением сохранить данные.

### 📤 Шэринг
Генерирует форматированный список персонажей (имена, ссылки, теги) и копирует в буфер обмена — готово для вставки в **Discord**, **Telegram** или любую платформу с поддержкой markdown.

---

## 🏗️ Архитектура

JaniTracker — это **Single File Application (SFA)** — всё приложение упаковано в один файл `index.html` (~42 КБ). Это осознанное решение: никаких сборщиков, зависимостей для установки, серверов. Просто скачайте и откройте.

### Иерархия компонентов

```
index.html
├── <head>
│   ├── Tailwind CSS (CDN)
│   ├── Babel Standalone (CDN)
│   └── Пользовательский CSS (скроллбар, анимация загрузки)
│
└── <script type="text/babel">
    ├── translations{}        — словарь i18n (ru/en)
    ├── <BotCard />           — Компонент карточки персонажа (рендеры для плитки + списка)
    ├── <App />               — Корневой компонент приложения
    │   ├── Управление состоянием  — useState + useEffect для персистенции
    │   ├── Сортировка и фильтры   — useMemo для производных данных
    │   ├── CRUD-операции           — добавление / редактирование / удаление / дублирование / закрепление
    │   ├── Импорт / Экспорт       — FileReader API + Blob-скачивание
    │   └── Шэринг                 — Clipboard API
    └── <ErrorBoundary />     — Классовый компонент для обработки критических ошибок
```

### Управление состоянием

| Состояние | Тип | Хранение | Описание |
|---|---|---|---|
| `bots` | `Array<Bot>` | `localStorage` | Основное хранилище всех карточек |
| `lang` | `'ru' \| 'en'` | Оперативная память | Текущий язык интерфейса |
| `theme` | `'dark' \| 'light'` | Оперативная память | Текущая цветовая тема |
| `viewMode` | `'grid' \| 'list'` | Оперативная память | Текущий режим отображения |
| `searchTerm` | `string` | Оперативная память | Активный поисковый фильтр |
| `selectedTags` | `string[]` | Оперативная память | Активные фильтры по тегам |
| `sortType` | `string` | Оперативная память | Ключ порядка сортировки |
| `showBackupAlert` | `boolean` | Вычисляется из `localStorage` | 14-дневное напоминание |

### Модель данных бота

```json
{
  "id": "1710000000000",
  "name": "Имя персонажа",
  "link": "https://janitorai.com/characters/...",
  "avatar": "https://...",
  "status": "active | wip | private | planned",
  "lastUpdated": "2025-01-15",
  "tags": ["Fantasy", "DC", "AU"],
  "notes": "Свободные заметки о персонаже",
  "checklist": [
    { "id": 1710000000001, "text": "Написать приветствие", "done": false }
  ],
  "pinned": false
}
```

---

## 🛠️ Технологии

| Технология | Версия | Роль |
|---|---|---|
| **React** | 18.2.0 | Рендеринг UI через ESM-импорты с `esm.sh` |
| **Babel Standalone** | 7.28.6 | Трансформация JSX в браузере |
| **Tailwind CSS** | CDN (latest) | Утилитарная стилизация |
| **Lucide React** | 0.263.1 | Библиотека иконок (25+ иконок) |
| **LocalStorage API** | — | Клиентское персистентное хранилище |
| **FileReader API** | — | Импорт JSON-файлов |
| **Blob API** | — | Экспорт/скачивание JSON-файлов |
| **Clipboard API** | — | Копирование в буфер обмена |

---

## 🚀 Начало работы

### Требования
Современный браузер (Chrome, Firefox, Edge, Safari).  
**Это всё.** Node.js, менеджер пакетов и инструменты сборки не нужны.

### Использование
1. Скачайте `index.html` из этого репозитория.
2. Откройте файл в браузере.
3. Начните добавлять своих персонажей.

### Перенос данных
Для переноса списка ботов между устройствами:
1. Нажмите **«Бэкап»** в подвале страницы — скачается `.json`-файл.
2. Перекиньте файл на целевое устройство.
3. Откройте JaniTracker на целевом устройстве, нажмите **«Импорт»** и выберите файл.

### Массовый импорт с JanitorAI
1. Установите [**Janitor Exporter**](https://github.com/itsfantomas/janitor-exporter) как Tampermonkey-скрипт.
2. На JanitorAI откройте страницу каждого персонажа и нажмите **«For Tracker (.json)»**.
3. В JaniTracker нажмите **«Импорт»** и выберите все скачанные `.json`-файлы — они объединятся с текущим списком.

---

## 📋 История изменений

### v1.1.2
- 📌 **Закрепление** — прикрепляйте карточки вверху списка
- 👯 **Дублирование** — клонирование персонажей одним кликом
- 🎛️ **Плитка / Список** — переключение между карточками и компактным видом
- 🛡️ **Напоминание о бэкапе** — алерт-баннер по прошествии 14 дней

### v1.1.1
- Импорт одиночных JSON-карточек (объединение с существующим списком)

### v1.0.0
- Первый релиз: CRUD, теги, чек-листы, поиск, сортировка, i18n, темы, бэкап/импорт, шэринг

---

## 📄 License / Лицензия

MIT — Use it, fork it, remix it. Free for everyone.

---

<div align="center">

*Created with ❤️ for the JanitorAI & SillyTavern community.*

[Telegram](https://t.me/itsfantomaslab)

</div>
