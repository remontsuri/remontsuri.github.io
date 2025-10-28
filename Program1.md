# Program 1: Telegram-бот

## Описание
Автоматизированный бот для Telegram, предназначенный для обработки запросов пользователей, автоматизации задач и взаимодействия с различными API.

**Технологии:** Python, python-telegram-bot API, asyncio

**Функционал:**
- Обработка команд пользователей
- Автоматические ответы
- Интеграция с внешними сервисами
- Управление подписками и уведомлениями

## Код

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

[← Назад к списку программ](PROGRAMS.md) | [Назад на главную](README.md)
