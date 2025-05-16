## 🔬 Лабораторная работа: Бот погоды с уведомлениями и хранением данных

### Цель работы:
Научиться использовать библиотеку `aiogram`, взаимодействовать с внешним API, работать с базами данных SQLite и реализовывать логику отправки сообщений на основе полученных данных.

---

## 🧰 Необходимые библиотеки:
Linux or Mac OS
```bash
python3 -m venv env
source env/bin/activate
pip install aiogram requests aiosqlite
```
Windows
```bash
python -m venv env
.\env\Scripts\activate
pip install aiogram requests aiosqlite
```

Также нужен ключ к [weatherapi.com](https://www.weatherapi.com/)

---

## 📁 Структура проекта:

```
weather_bot/
│
├── .gitignore         
├── bot.py          # Основной файл бота
├── database.py     # Работа с SQLite
├── config.py       # Конфигурация (токен, API-ключ)
└── config_template.py       # Шаблон конфигурации (токен, API-ключ)
```

---

## 📄 config.py

```python
# config.py

BOT_TOKEN = "YOUR_BOT_TOKEN_HERE"
WEATHER_API_KEY = "YOUR_WEATHER_TOKEN_HERE"
```

---

## 🗃️ database.py

```python
# database.py
import aiosqlite
import os

DB_NAME = "users.db"

async def init_db():
    async with aiosqlite.connect(DB_NAME) as db:
        await db.execute("""
            CREATE TABLE IF NOT EXISTS users (
                telegram_id INTEGER PRIMARY KEY,
                username TEXT,
                gender TEXT,
                notification_time TEXT,
                notification_freq TEXT
            )
        """)
        await db.commit()

async def add_or_update_user(telegram_id, username, gender, notification_time, notification_freq):
    async with aiosqlite.connect(DB_NAME) as db:
        await db.execute("""
            INSERT INTO users (telegram_id, username, gender, notification_time, notification_freq)
            VALUES (?, ?, ?, ?, ?)
            ON CONFLICT(telegram_id) DO UPDATE SET
                username = excluded.username,
                gender = excluded.gender,
                notification_time = excluded.notification_time,
                notification_freq = excluded.notification_freq
        """, (telegram_id, username, gender, notification_time, notification_freq))
        await db.commit()

async def get_user(telegram_id):
    async with aiosqlite.connect(DB_NAME) as db:
        async with db.execute("SELECT * FROM users WHERE telegram_id=?", (telegram_id,)) as cursor:
            return await cursor.fetchone()

async def get_users():
    async with aiosqlite.connect(DB_NAME) as db:
        async with db.execute("SELECT * FROM users") as cursor:
            return await cursor.fetchall()
```

---

## 🤖 bot.py

```python
# bot.py
from aiogram import Bot, Dispatcher, F
from aiogram.types import Message, ReplyKeyboardMarkup, KeyboardButton
from aiogram.filters import CommandStart
from aiogram.fsm.context import FSMContext
from aiogram.fsm.state import State, StatesGroup
from aiogram.fsm.storage.memory import MemoryStorage
import asyncio
import requests
from config import BOT_TOKEN, WEATHER_API_KEY
from database import init_db, add_or_update_user, get_user

bot = Bot(token=BOT_TOKEN)
dp = Dispatcher(storage=MemoryStorage())

class Registration(StatesGroup):
    gender = State()
    notification_time = State()
    notification_freq = State()

# Клавиатура
keyboard = ReplyKeyboardMarkup(
    keyboard=[
        [KeyboardButton(text="Узнать погоду")],
        [KeyboardButton(text="Регистрация")]
    ],
    resize_keyboard=True
)

def get_weather_smile(temp_c):
    if temp_c < 0:
        return "❄️"
    elif 0 <= temp_c < 15:
        return "🧥"
    elif 15 <= temp_c < 25:
        return "🌤️"
    else:
        return "🔥"

@dp.message(CommandStart())
async def start(message: Message):
    await message.answer("Привет! Я бот для определения погоды.", reply_markup=keyboard)

@dp.message(F.text == "Узнать погоду")
async def get_weather(message: Message):
    url = f"https://api.weatherapi.com/v1/current.json?key={WEATHER_API_KEY}&q=Moscow"

    response = requests.get(url)
    data = response.json()

    if "error" in data:
        await message.answer("Ошибка получения погоды.")
        return

    temp_c = data["current"]["temp_c"]
    emoji = get_weather_smile(temp_c)
    await message.answer(f"{emoji} Температура в Москве: {temp_c}°C")

@dp.message(F.text == "Регистрация")
async def registration_start(message: Message, state: FSMContext):
    await state.set_state(Registration.gender)
    await message.answer("Введите ваш пол (муж/жен):")

@dp.message(Registration.gender)
async def registration_gender(message: Message, state: FSMContext):
    await state.update_data(gender=message.text)
    await state.set_state(Registration.notification_time)
    await message.answer("Введите время уведомлений (например, 09:00):")

@dp.message(Registration.notification_time)
async def registration_notification_time(message: Message, state: FSMContext):
    await state.update_data(notification_time=message.text)
    await state.set_state(Registration.notification_freq)
    await message.answer("Введите частоту уведомлений (ежедневно/еженедельно):")

@dp.message(Registration.notification_freq)
async def registration_notification_freq(message: Message, state: FSMContext):
    data = await state.get_data()
    user_data = {
        "telegram_id": message.from_user.id,
        "username": message.from_user.username,
        "gender": data["gender"],
        "notification_time": data["notification_time"],
        "notification_freq": message.text
    }
    await add_or_update_user(**user_data)
    await message.answer("Вы успешно зарегистрированы!")
    await state.clear()

async def main():
    await init_db()
    await dp.start_polling(bot)

if __name__ == "__main__":
    asyncio.run(main())
```

---

## Основное задание
1. Переделайте регистрацию чтобы пользователь мог взаимодействовать через кнопки. (пример: при выборе пола - появляются две кнопки - Мужской - Женский)
2. Выбор времени сделайте через **InlineKeyboardButton** часы с 0 до 24. Минуты с 0 до 60 с интервалом в 5 минут.
3. Если пользователь админ (то есть вы) добавьте кнопку **Пользователи** которая будет отоброжать список всех зарегестрированных пользователей. 
4. Все переменные *клавиатур* вынесите в файл mykeyboard.py. Другие функции которые не имют декоратора **@dp.message** вынесите в *utils.py* (кроме main())
5. **Обязательно** создайте файл .gitignore (который игнорирует env и config.py). Создайте req.txt, которых хранит список всех библиотек. Выгрузите ваш проект на GitHub.

## Дополнительное задание
1. Вместо смайликов отправляте PNG картинки той одежды что вы делали в **Лабораторной 7**

### Оценивание
1. Создание базового бота - 30%
2. Выполение основного задания - 40%
3. Дополнительное задание - 30%

### 80% - оценка 5
### 50% - оценка 4
### 30% - оценка 3
