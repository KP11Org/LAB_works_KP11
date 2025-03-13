# **Лабораторная работа 4 (2 урока)**
### Практический урок: Создание Telegram Echo Bot на Python с использованием venv и Git
### [Дополнительный ресурс](https://habr.com/ru/companies/amvera/articles/820527/)

В этом уроке мы создадим простого Telegram-бота, который будет повторять сообщения пользователя (echo bot).

---

### Шаг 1: Подготовка окружения

#### 1.1 Создание виртуального окружения
1. Создайте папку для проекта:
   ```bash
   mkdir tg_echo_your_last_name
   cd tg_echo_your_last_name
   ```

2. Создайте виртуальное окружение:
   ```bash
   python3 -m venv env
   ```

3. Активируйте виртуальное окружение:
   - На Windows:
     ```bash
     env\Scripts\activate
     ```
   - На macOS/Linux:
     ```bash
     source env/bin/activate
     ```

4. Убедитесь, что виртуальное окружение активировано. В командной строке должно появиться `(env)` перед приглашением.

---

### Шаг 2: Установка необходимых библиотек

Для работы с Telegram API нам понадобится библиотека `python-telegram-bot`. Установим её:

```bash
pip install aiogram asyncio
```

---

#### 3.2 Написание кода бота
Создайте файл `bot.py` в папке проекта:

```python
from aiogram import Bot, Dispatcher, types, Router, filters, F
import asyncio
# Токен вашего бота
API_TOKEN = "ВАШ_ТОКЕН_ЗДЕСЬ"

# Инициализация бота и диспетчера
bot = Bot(token=API_TOKEN)
dp = Dispatcher()
start_router = Router()

# Обработчик команды /start
@start_router.message(filters.CommandStart())
async def send_welcome(message: types.Message):
    await message.reply("Привет! Я echo bot. Напиши мне что-нибудь, и я повторю.")

# Обработчик текстовых сообщений
@start_router.message(F.text)
async def echo(message: types.Message):
    await message.answer(message.text)

async def main():
    dp.include_router(start_router)
    await bot.delete_webhook(drop_pending_updates=True)
    await dp.start_polling(bot)

# Запуск бота
if __name__ == "__main__":
    asyncio.run(main())
```

Замените `ВАШ_ТОКЕН_ЗДЕСЬ` на токен, который вы получили от BotFather.

---

### Шаг 4: Запуск бота

1. Убедитесь, что виртуальное окружение активировано.
2. Запустите бота:
   ```bash
   python bot.py
   ```
3. Откройте Telegram, найдите своего бота и отправьте ему сообщение. Он должен ответить тем же текстом.

---

### Шаг 5: Работа с Git

#### 5.1 Инициализация репозитория
1. Инициализируйте Git в папке проекта:
   ```bash
   git init
   ```

2. Создайте файл `.gitignore`, чтобы исключить виртуальное окружение и другие ненужные файлы:
   ```
   env/
   __pycache__/
   *.pyc
   config.py
   ```

3. Добавьте файлы в репозиторий:
   ```bash
   git add .
   git commit -m "Initial commit: создан echo bot с использованием Aiogram"
   ```

#### 5.2 Создание удаленного репозитория
1. Создайте репозиторий на GitHub.
2. Добавьте удаленный репозиторий:
   ```bash
   git remote add link https://github.com/ваш_username/tg_echo_bot_your_last_name.git
   ```
3. Отправьте код на удаленный репозиторий:
   ```bash
   git push -u link main
   ```

---

### Задачи для самостоятельного выполнения

1. **Добавьте команду `/help`**, которая будет выводить список доступных команд.
2. **Измените бота так**, чтобы он добавлял к каждому сообщению текст "Вы сказали: " перед повторением.
3. **Преместите API_TOKEN** в `config.py` (данный файл не должен оказаться на гитхабе). Создайте шаблон конфига для других разработчиков.
4. **Используйте `requirements.txt`** для управления зависимостями. Создайте файл:
   ```bash
   pip freeze > requirements.txt
   ```
5. После выполнения самостоятельных задач повторите шаг 5.1 и 5.2, чтобы выгрузить ваши изменения на Git

---

### Оценивание
1. Выполнены первые 5 шагов - 50%
2. Задачи для самостоятельного выполнения - 1 шаг 10%

### 80% - оценка 5
### 60% - оценка 4
### 30% - оценка 3
