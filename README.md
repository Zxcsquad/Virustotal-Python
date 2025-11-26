# English
# Simple Virus Scanner (Python)

This repository contains a small, educational Python script to scan files using a local ClamAV daemon (`clamd`) and — as a fallback — query VirusTotal for a file's SHA256 reputation (requires an API key).

Files:
- `scanner.py`: main scanner script.
- `requirements.txt`: dependencies (`clamd`, `requests`).

Quick start (Windows PowerShell):

1. Install Python 3.8+ and pip.
2. Install ClamAV if you want local scanning (optional). Make sure `clamd` is running and reachable.
3. Install dependencies:

```powershell
python -m pip install -r requirements.txt
```

4. Run the scanner:

```powershell
# Scan a single file using ClamAV if available
python scanner.py --path C:\path\to\file.exe

# Scan recursively and use VirusTotal (replace YOUR_KEY)
python scanner.py --path C:\path\to\folder --recursive --vt-api-key YOUR_KEY

# New features
- Concurrent scanning: `--workers N` (default 4) to scan files in parallel.
- Progress bar: shows per-file progress if `tqdm` is installed.
- Colored output: clearer OK/DETECTED/ERROR statuses (Windows requires `colorama`).
- JSON summary: `--json-output report.json` to write results.
- Quarantine: `--quarantine-dir quarantined` will move detected files to that folder.

Examples (PowerShell):

```powershell
# Parallel scan with JSON summary and quarantine
python scanner.py --path "C:\path\to\folder" --recursive --workers 8 --vt-api-key YOUR_KEY --json-output "C:\temp\report.json" --quarantine-dir "C:\temp\quarantine"
```
```

Notes:
- To use VirusTotal you need an API key. Visit https://www.virustotal.com to obtain one.
- Local scanning requires the ClamAV daemon (`clamd`) and the `clamd` Python package.
- This tool is for basic scanning and demonstration purposes — for production use, consider hardened solutions and proper error handling.

Security & Privacy:
- When you query VirusTotal, you are sending file hashes (not the raw file) to a third-party service, and the service may already contain the file. Do not upload private secrets if that is a concern.











# Руский


# Инструкция по использованию скрипта `scanner.py`

Ниже — подробная пошаговая инструкция на русском языке по установке, настройке и запуску сканера, который находится в `c:\Users\мит\Downloads\пп\scanner.py`.

**Содержание**
- Быстрый старт
- Установка зависимостей
- Настройка ClamAV (опционально)
- Получение VirusTotal API ключа (опционально)
- Примеры запуска (PowerShell)
- Параметры скрипта
- Что выводит скрипт и структура JSON-отчёта
- Важные замечания и отладка

---

## Быстрый старт

1. Убедитесь, что установлен Python 3.8+ и `pip`.
2. (Опционально) Настройте `clamd` (если хотите локальное сканирование).
3. Установите зависимости из `requirements.txt`.

## Установка зависимостей (PowerShell)

```powershell
# (опционально) создаём виртуальное окружение и активируем
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# установка библиотек
python -m pip install -r "c:\Users\мит\Downloads\пп\requirements.txt"
```

## Настройка ClamAV (опционально)

- На Windows проще запускать ClamAV в WSL или Docker; полноценная установка `clamd` в Windows сложнее.
- Если `clamd` не доступен, скрипт всё равно будет работать — он либо использует VirusTotal (если указан ключ), либо только вычисляет SHA256 и сообщает об этом.

## Получение VirusTotal API ключа (опционально)

- Зарегистрируйтесь на https://www.virustotal.com и получите API key.
- Ключ передаётся в скрипт через параметр `--vt-api-key`.

## Примеры запуска (PowerShell)

- Просканировать один файл (локальный ClamAV если доступен):

```powershell
python "c:\Users\мит\Downloads\пп\scanner.py" --path "C:\path\to\file.exe"
```

- Просканировать папку (файлы в корне папки):

```powershell
python "c:\Users\мит\Downloads\пп\scanner.py" --path "C:\path\to\folder"
```

- Рекурсивно (включая поддиректории):

```powershell
python "c:\Users\мит\Downloads\пп\scanner.py" --path "C:\path\to\folder" --recursive
```

- Использовать VirusTotal (замените `YOUR_KEY`):

```powershell
python "c:\Users\мит\Downloads\пп\scanner.py" --path "C:\path\to\folder" --recursive --vt-api-key YOUR_KEY
```

- Параллельное сканирование, JSON-отчёт и карантин:

```powershell
python "c:\Users\мит\Downloads\пп\scanner.py" --path "C:\path\to\folder" --recursive --workers 8 --vt-api-key YOUR_KEY --json-output "C:\temp\report.json" --quarantine-dir "C:\temp\quarantine"
```

## Параметры скрипта

- `--path` : путь к файлу или папке (обязательно).
- `--recursive` : рекурсивно проходить поддиректории.
- `--vt-api-key` : ключ VirusTotal (опционально).
- `--workers N` : количество потоков/воркеров (по умолчанию 4).
- `--json-output file.json` : записать JSON-отчёт с результатами.
- `--quarantine-dir DIR` : переместить обнаруженные файлы в указанную папку.

## Что выводит скрипт

- В терминале для каждого файла выводится одна строка:
  - `[OK] path` — файл не обнаружен как вредоносный.
  - `[DETECTED] path` — файл помечен как обнаруженный (ClamAV или VirusTotal).
  - `[ERROR] path: message` — произошла ошибка при проверке.

- Если указан `--json-output`, создаётся JSON файл со структурой:

```json
{
  "summary": { "total": 42, "detected": 1 },
  "results": [
    {
      "path": "C:\\file.exe",
      "sha256": "...",
