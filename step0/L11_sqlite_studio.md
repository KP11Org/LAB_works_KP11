# Лабораторная работа: Разработка структуры БД

## Цель работы:
Изучить возможности SQLite Studio.

---

## Задание:

Создайте базу данных с помощью **SQLite Studio [Гайд дял новичков](https://comp.post-ap.ru/sqlite-studio-kak-posmotret-skhemu-dannykh/)**, которое помогает пользователям изучать достопримечательности. Приложение должно поддерживать:

1. Хранение информации о локациях (название, координаты, описание, категорию).
2. Регистрацию пользователей с разными ролями (например, `user`, `moderator`, `admin`).
3. Систему ачивок (достижений), которые могут получать пользователи за действия (например, "Первый шаг", "Путешественник", "Эксперт").

---

## Этапы выполнения:

### 1. Установка и настройка SQLite Studio
- Скачайте и установите [SQLite Studio Portable](https://github.com/pawelsalawa/sqlitestudio/releases) (если ещё не установлено).
- Откройте программу.

---

### 2. Создание новой базы данных
- Файл → Создать новую базу данных.
- Назовите файл `tourist_app.db`.
- Сохраните его в удобном месте.

---

### 3. Проектирование таблиц

#### Таблица: `users`
Хранит данные о пользователях.

| Поле | Тип | Описание |
|------|-----|----------|
| id | INTEGER PRIMARY KEY AUTOINCREMENT | Уникальный ID пользователя |
| username | TEXT UNIQUE NOT NULL | Имя пользователя |
| email | TEXT UNIQUE NOT NULL | Email |
| password_hash | TEXT NOT NULL | Хэш пароля |
| role | TEXT NOT NULL DEFAULT 'user' | Роль ('user', 'moderator', 'admin') |

#### Создаём таблицу `categories`

| Поле | Тип | Описание |
|------|-----|----------|
| id | INTEGER PRIMARY KEY AUTOINCREMENT | Уникальный ID категории |
| name | TEXT UNIQUE NOT NULL | Название категории (например, "памятник", "музей") |

#### Создаём таблицу `locations`

| Поле | Тип | Описание |
|------|-----|----------|
| id | INTEGER PRIMARY KEY AUTOINCREMENT | Уникальный ID локации |
| name | TEXT NOT NULL | Название локации |
| description | TEXT | Описание |
| latitude | REAL | Широта |
| longitude | REAL | Долгота |
| category_id | INTEGER | Ссылка на категорию |
| FOREIGN KEY(category_id) REFERENCES categories(id) |

#### Таблица: `achievements`
Хранит информацию о доступных достижениях.

| Поле | Тип | Описание |
|------|-----|----------|
| id | INTEGER PRIMARY KEY AUTOINCREMENT | Уникальный ID ачивки |
| title | TEXT NOT NULL | Заголовок достижения |
| description | TEXT | Описание |
| points_required | INTEGER DEFAULT 0 | Количество баллов или действий для получения |

#### Таблица: `user_achievements`
Связывает пользователей с их достижениями.

| Поле | Тип | Описание |
|------|-----|----------|
| user_id | INTEGER | ID пользователя |
| achievement_id | INTEGER | ID достижения |
| achieved_at | DATETIME DEFAULT CURRENT_TIMESTAMP | Время получения достижения |
| FOREIGN KEY(user_id) REFERENCES users(id) |
| FOREIGN KEY(achievement_id) REFERENCES achievements(id) |

---

### 4. Создание таблиц в SQLite Studio

1. Перейдите во вкладку **"Database" → "Add a table"**.
2. Последовательно создайте все четыре таблицы согласно вышеуказанным спецификациям.
3. Для внешних ключей укажите соответствующие ссылки (`FOREIGN KEY`).

---

### 5. Заполнение тестовыми данными
- от 5 до 10 записей в каждой из таблиц.
### Основное задание:
1. Измените структуру БД.
   - для ролей пользователей создайте отдельную таблицу ```role```
   - переиминуйте таблицу ```categories``` -> ```location_tag``` и организуйте связь многие ко многим между **location_tag** and **locations**.
2. Создайте новую базу данных со следующими таблицами
### 📋 Таблица: `users`

| Поле                    | Тип      | Описание |
|-------------------------|----------|----------|
| `id`                    | INTEGER PRIMARY KEY | Уникальный ID записи |
| `telegram_id`           | INTEGER UNIQUE NOT NULL | ID пользователя из Telegram |
| `username`              | TEXT     | Имя пользователя (может отсутствовать) |
| `gender`                | TEXT     | Пол (`male`, `female`, `other`) |
| `notification_frequency_id` | INTEGER | Внешний ключ на таблицу `notification_frequencies` |
| `notification_time`     | TEXT     | Время уведомления в формате `HH:MM` (например, `09:00`) |

---

### 📋 Таблица: `notification_frequencies`

| Поле       | Тип    | Описание |
|------------|--------|----------|
| `id`       | INTEGER PRIMARY KEY | Уникальный ID частоты |
| `frequency`| TEXT   | Название частоты (`daily`, `weekly`, `monthly`, `never`) |

---
3. Экспортируйте файлы БД (*.sql) на Git (**обязательный пункт**)

### Оценивание
1. Первые 5 шагов - 40%
2. Основное задание - 60%

### 80% - оценка 5
### 60% - оценка 4
### 30% - оценка 3

