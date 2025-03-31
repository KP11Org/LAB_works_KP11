# **Лабораторная работа 5 (2 урока)**
### Основы HTML и CSS

---

### Теоретическая часть

#### 1. Введение в HTML
HTML (HyperText Markup Language) — это язык разметки, используемый для создания структуры веб-страницы. Основные элементы HTML:
- `<html>` — корневой элемент страницы.
- `<head>` — содержит метаинформацию (заголовок, ссылки на стили и т.д.).
- `<body>` — основное содержимое страницы.
- Заголовки: `<h1>`, `<h2>`, ..., `<h6>`.
- Параграфы: `<p>`.
- Ссылки: `<a>`.
- Изображения: `<img>`.
- Списки: `<ul>`, `<ol>`, `<li>`.
- Контейнеры: `<div>`, `<span>`.

#### 2. Введение в CSS
CSS (Cascading Style Sheets) — язык стилей, используемый для оформления HTML-документов. Основные понятия:
- Селекторы: выбирают элементы для стилизации (например, `h1`, `.class`, `#id`).
- Свойства: определяют, как стилизовать элемент (например, `color`, `font-size`, `margin`).
- Значения: конкретные параметры свойств (например, `red`, `16px`, `20px`).

---

### Практическая часть

#### Задание 1: Создание базовой HTML-страницы
0. Создайте папку L5_html_css_your_last_name
1. Создайте файл `index.html`.
2. Добавьте базовую структуру HTML-документа:
   ```html
   <!DOCTYPE html>
   <html lang="ru">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Ваше ФИО</title>
   </head>
   <body>
       <h1>Привет, мир!</h1>
       <p>Это моя первая веб-страница.</p>
       <ul>
           <li>Пункт 1</li>
           <li>Пункт 2</li>
           <li>Пункт 3</li>
       </ul>
   </body>
   </html>
   ```
3. Откройте файл в браузере, чтобы увидеть результат.

#### Задание 2: Добавление CSS
1. Создайте файл `styles.css` в той же папке, что и `index.html`.
2. Подключите CSS к HTML, добавив в `<head>`:
   ```html
   <link rel="stylesheet" href="styles.css">
   ```
3. Добавьте стили в `styles.css`:
   ```css
   body {
       font-family: Arial, sans-serif;
       background-color: #f0f0f0;
       color: #333;
   }

   h1 {
       color: #0066cc;
   }

   ul {
       list-style-type: square;
   }

   p {
       font-size: 16px;
       line-height: 1.5;
   }
   ```
4. Обновите страницу в браузере, чтобы увидеть изменения.

#### Задание 3: Работа с классами и идентификаторами
1. Добавьте класс и идентификатор в HTML:
   ```html
   <p class="highlight">Этот текст будет выделен.</p>
   <p id="special">Этот текст особенный.</p>
   ```
2. Добавьте стили для класса и идентификатора в CSS:
   ```css
   .highlight {
       background-color: yellow;
       padding: 5px;
   }

   #special {
       font-weight: bold;
       color: red;
   }
   ```
3. Проверьте результат в браузере.

#### Задание 4: Создание простого макета
1. Создайте структуру макета в HTML:
   ```html
   <div class="container">
       <header>
           <h1>Привет, мир!</h1>
           <p>Это моя первая веб-страница.</p>
       </header>
       <nav>
           <ul>
               <li><a href="#">Главная</a></li>
               <li><a href="#">О нас</a></li>
               <li><a href="#">Контакты</a></li>
           </ul>
       </nav>
       <main>
           <p>Добро пожаловать на мой сайт!</p>
       </main>
       <footer>
           <p>&copy; 2023 Мой сайт</p>
       </footer>
   </div>
   ```
2. Добавьте стили для макета в CSS:
   ```css
   .container {
       width: 80%;
       margin: 0 auto;
   }

   header {
       background-color: #0066cc;
       color: white;
       padding: 10px;
       text-align: center;
   }

   nav {
       background-color: #333;
       padding: 10px;
   }

   nav ul {
       list-style-type: none;
       margin: 0;
       padding: 0;
   }

   nav ul li {
       display: inline;
       margin-right: 10px;
   }

   nav ul li a {
       color: white;
       text-decoration: none;
   }

   main {
       padding: 20px;
   }

   footer {
       background-color: #333;
       color: white;
       text-align: center;
       padding: 10px;
   }
   ```
3. Проверьте результат в браузере.

---

### Дополнительные задания
1. Добавьте изображение на страницу с помощью тега `<img>`.
2. Создайте форму с полями ввода (`<input>`, `<textarea>`, `<button>`).
3. Поэкспериментируйте с CSS-свойствами, такими как `border`, `box-shadow`.
4. Выгрузите ваш проект на GitHub в репозиторий L5_html_your_last_name (публичный). Настройте GitHub Pages.
![image](https://github.com/user-attachments/assets/c261ce2f-e2eb-4209-8199-4e91374d0e82)
5. Убедитесь что ваш сайт корректно отображается на сервере. (Подключены стили и изображения)

---

### Оценивание
1. Выполнены первые 4 Задания - 50%
2. Дополнительные задания - 50% (1-3 - 20%) (4-5 - 30%)

### 80% - оценка 5
### 60% - оценка 4
### 30% - оценка 3
