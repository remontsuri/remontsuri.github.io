# Мои программы

Здесь представлены созданные мной программы и проекты из недавних диалогов.

## Список программ

### 1. Telegram-бот
**Описание:** Автоматизированный бот для Telegram, предназначенный для обработки запросов пользователей, автоматизации задач и взаимодействия с различными API.

**Технологии:** Python, python-telegram-bot API, asyncio

**Функционал:**
- Обработка команд пользователей
- Автоматические ответы
- Интеграция с внешними сервисами
- Управление подписками и уведомлениями

**Пример кода:**
```python
import telebot
from telebot import types

# Инициализация бота
bot = telebot.TeleBot('YOUR_BOT_TOKEN')

@bot.message_handler(commands=['start'])
def start_message(message):
    bot.send_message(message.chat.id, 'Привет! Я готов к работе.')

@bot.message_handler(content_types=['text'])
def handle_text(message):
    bot.send_message(message.chat.id, f'Вы написали: {message.text}')

if __name__ == '__main__':
    bot.polling(none_stop=True)
```

---

### 2. CS2 Marketplace скрипт
**Описание:** Скрипт для мониторинга и анализа цен на торговой площадке Counter-Strike 2. Отслеживает изменения цен на скины и предметы, помогает находить выгодные предложения.

**Технологии:** Python, requests, BeautifulSoup4, pandas

**Функционал:**
- Парсинг цен с торговых площадок
- Анализ динамики цен
- Уведомления о выгодных предложениях
- Экспорт данных в CSV/Excel

**Пример кода:**
```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

class CS2MarketParser:
    def __init__(self, url):
        self.url = url
        self.items = []
    
    def parse_items(self):
        response = requests.get(self.url)
        soup = BeautifulSoup(response.content, 'html.parser')
        
        # Парсинг предметов
        items = soup.find_all('div', class_='item')
        for item in items:
            name = item.find('span', class_='name').text
            price = item.find('span', class_='price').text
            self.items.append({'name': name, 'price': price})
        
        return pd.DataFrame(self.items)
    
    def save_to_csv(self, filename='cs2_items.csv'):
        df = pd.DataFrame(self.items)
        df.to_csv(filename, index=False)
        print(f'Данные сохранены в {filename}')
```

---

### 3. Бот мониторинга цен
**Описание:** Универсальный бот для отслеживания цен на различных интернет-площадках. Отправляет уведомления при изменении цен на интересующие товары.

**Технологии:** Python, Selenium/requests, SQLite, Schedule

**Функционал:**
- Мониторинг цен на заданные товары
- Хранение истории цен в БД
- Автоматические уведомления (email/telegram)
- Построение графиков изменения цен
- Настраиваемые интервалы проверки

**Пример кода:**
```python
import requests
import sqlite3
from datetime import datetime
import schedule
import time

class PriceMonitor:
    def __init__(self, db_name='prices.db'):
        self.conn = sqlite3.connect(db_name)
        self.create_table()
    
    def create_table(self):
        cursor = self.conn.cursor()
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS prices (
                id INTEGER PRIMARY KEY,
                product_name TEXT,
                price REAL,
                timestamp DATETIME
            )
        ''')
        self.conn.commit()
    
    def check_price(self, url, product_name):
        # Получение цены (упрощенный пример)
        response = requests.get(url)
        # Здесь должен быть парсинг цены
        price = self.parse_price(response.text)
        
        # Сохранение в БД
        cursor = self.conn.cursor()
        cursor.execute(
            'INSERT INTO prices (product_name, price, timestamp) VALUES (?, ?, ?)',
            (product_name, price, datetime.now())
        )
        self.conn.commit()
        return price
    
    def parse_price(self, html):
        # Логика парсинга цены
        pass

# Запуск мониторинга каждые 30 минут
monitor = PriceMonitor()
schedule.every(30).minutes.do(lambda: monitor.check_price('URL', 'Product'))

while True:
    schedule.run_pending()
    time.sleep(60)
```

---

### 4. PowerShell скрипты автоматизации Windows
**Описание:** Набор PowerShell скриптов для автоматизации рутинных задач в Windows: управление файлами, резервное копирование, настройка системы, автоматизация обновлений.

**Технологии:** PowerShell, Windows API, Task Scheduler

**Функционал:**
- Автоматическое резервное копирование
- Очистка системы от временных файлов
- Массовое переименование файлов
- Управление службами Windows
- Мониторинг системных ресурсов

**Примеры скриптов:**

**Резервное копирование:**
```powershell
# Скрипт резервного копирования
param(
    [string]$SourcePath,
    [string]$BackupPath
)

$timestamp = Get-Date -Format "yyyyMMdd_HHmmss"
$backupFolder = Join-Path $BackupPath "Backup_$timestamp"

New-Item -ItemType Directory -Path $backupFolder -Force
Copy-Item -Path $SourcePath\* -Destination $backupFolder -Recurse

Write-Host "Резервная копия создана: $backupFolder"
```

**Очистка системы:**
```powershell
# Очистка временных файлов
$tempFolders = @(
    $env:TEMP,
    "C:\Windows\Temp",
    "C:\Windows\Prefetch"
)

foreach ($folder in $tempFolders) {
    if (Test-Path $folder) {
        Get-ChildItem -Path $folder -Recurse -Force -ErrorAction SilentlyContinue | 
            Remove-Item -Force -Recurse -ErrorAction SilentlyContinue
        Write-Host "Очищена папка: $folder"
    }
}

Write-Host "Очистка завершена"
```

---

### 5. Whisper GPU (Транскрибация аудио)
**Описание:** Скрипт для транскрибации аудио и видео файлов с использованием модели OpenAI Whisper с ускорением на GPU. Поддерживает пакетную обработку и различные языки.

**Технологии:** Python, OpenAI Whisper, PyTorch, CUDA, ffmpeg

**Функционал:**
- Транскрибация аудио/видео в текст
- Поддержка GPU ускорения (NVIDIA CUDA)
- Определение языка автоматически
- Экспорт в различные форматы (txt, srt, vtt)
- Пакетная обработка файлов
- Временные метки для субтитров

**Пример кода:**
```python
import whisper
import torch
import os
from pathlib import Path

class WhisperTranscriber:
    def __init__(self, model_size='base', device='cuda'):
        # Проверка доступности GPU
        self.device = device if torch.cuda.is_available() else 'cpu'
        print(f"Используется устройство: {self.device}")
        
        # Загрузка модели
        self.model = whisper.load_model(model_size, device=self.device)
    
    def transcribe_file(self, audio_path, language='ru', output_format='txt'):
        """Транскрибация одного файла"""
        print(f"Обработка: {audio_path}")
        
        # Транскрибация
        result = self.model.transcribe(
            audio_path,
            language=language,
            fp16=(self.device == 'cuda')  # Использование FP16 для GPU
        )
        
        # Сохранение результата
        output_path = Path(audio_path).with_suffix(f'.{output_format}')
        
        if output_format == 'txt':
            with open(output_path, 'w', encoding='utf-8') as f:
                f.write(result['text'])
        elif output_format == 'srt':
            self.write_srt(result['segments'], output_path)
        
        print(f"Результат сохранен: {output_path}")
        return result
    
    def write_srt(self, segments, output_path):
        """Сохранение в формате SRT для субтитров"""
        with open(output_path, 'w', encoding='utf-8') as f:
            for i, segment in enumerate(segments, 1):
                start = self.format_timestamp(segment['start'])
                end = self.format_timestamp(segment['end'])
                text = segment['text'].strip()
                
                f.write(f"{i}\n")
                f.write(f"{start} --> {end}\n")
                f.write(f"{text}\n\n")
    
    def format_timestamp(self, seconds):
        """Форматирование времени для SRT"""
        hours = int(seconds // 3600)
        minutes = int((seconds % 3600) // 60)
        secs = int(seconds % 60)
        millis = int((seconds % 1) * 1000)
        return f"{hours:02d}:{minutes:02d}:{secs:02d},{millis:03d}"
    
    def batch_transcribe(self, folder_path, file_extensions=['.mp3', '.wav', '.m4a']):
        """Пакетная обработка файлов"""
        folder = Path(folder_path)
        files = []
        
        for ext in file_extensions:
            files.extend(folder.glob(f'*{ext}'))
        
        print(f"Найдено файлов: {len(files)}")
        
        for audio_file in files:
            try:
                self.transcribe_file(str(audio_file))
            except Exception as e:
                print(f"Ошибка при обработке {audio_file}: {e}")

# Использование
if __name__ == '__main__':
    transcriber = WhisperTranscriber(model_size='medium', device='cuda')
    
    # Одиночный файл
    transcriber.transcribe_file('audio.mp3', language='ru', output_format='srt')
    
    # Пакетная обработка
    # transcriber.batch_transcribe('./audio_files')
```

**Установка зависимостей:**
```bash
pip install openai-whisper torch torchaudio
# Для GPU поддержки:
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

---

## Общие особенности проектов

- **Автоматизация:** Все программы направлены на автоматизацию рутинных задач
- **Python ecosystem:** Основной язык разработки - Python с использованием популярных библиотек
- **Практичность:** Решение реальных задач из повседневной работы
- **GPU ускорение:** Использование GPU для ресурсоемких задач (Whisper)
- **Cross-platform:** Большинство скриптов кроссплатформенные (кроме PowerShell)

## Планы развития

- Создание веб-интерфейсов для некоторых скриптов
- Добавление Docker контейнеров для упрощения развертывания
- Расширение функционала ботов
- Интеграция различных сервисов между собой

## Контакты

Для вопросов и предложений можно связаться со мной через GitHub: [@remontsuri](https://github.com/remontsuri)
