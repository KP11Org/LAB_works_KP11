# **Лабораторная работа 8 (8 уроков)**
### HTML CSS Figma

---
# Нужно сверстать две страницы, используя HTML and CSS (презентация одежды, страница с выбором параметров)
![image](https://github.com/user-attachments/assets/20a1a582-5d60-43ca-a27c-dbd04d28883b)
![image](https://github.com/user-attachments/assets/cdcbc686-1e59-485c-b41a-01fbe26c7da9)

## Техническое задание
#### 1. Шрифт: [Handjet](https://fonts.google.com/specimen/Handjet)
#### 2. Темная и светля тема (Разработайте темную и светлую тему согласно вашему дизайну)
#### 3. Ресурсы (Повторите данные картинки в Figma, все необходимые ресурсы такие как солнышко, тучка и звездочка экспортируйте как SVG)
#### 4. Вы можете изменять дизайн приложения не меняя основную концепцию (5 полей для выбора, кнопка Confirm, Темная и светлая тема, Наличие солнца и луны)
#### 5. Комплект одежды который представлен на макете замените тем что вы разработали в 7-ой лабораторной.
##### - Комплект (5-7 элементов) должен быть одной картинкой
##### - Данная картинка должна добавляться в HTML разметку с помощью JavaScript.
##### - Для упрощения работы с комплектами называйте картинки согласно паттерну (man_15_more.svg, man_15_less.svg, man_5_5.svg, man_plus_5_15.svg, man_minus_5_15.svg)
#### 6. Select Gender - поле выбора с двумя зачениями (Man - Woman)
![image](https://github.com/user-attachments/assets/bfafea1b-dba4-41de-b99e-a31fc41c3d90)
#### 7. Time - поле выбора времени
![image](https://github.com/user-attachments/assets/54eb61a5-3a51-4f76-8261-01ffb66adfd5)
#### 8. Day of week - поле выбора с 5 значениями
![image](https://github.com/user-attachments/assets/a2d1f2c0-042b-4d1e-9aff-e10a0e42b675)
#### 9. AREA - Область/Регион - поле выбора со списком областей/регионов. (Данные значения необходимо взять из папки [materals](./materials/) файл [towns.json](./materials/towns.json) )
![image](https://github.com/user-attachments/assets/d8a41b1a-a394-4f0a-9ec2-85d391327ded)
#### 10. TOWN - Город - поле выбора со списком городов. Данное поле не доступно пока не выбрана ЗОНА (AREA). Города отображаются согласно выбранной зоне. (Данные значения необходимо взять из папки [materals](./materials/) файл [towns.json](./materials/towns.json) )
![image](https://github.com/user-attachments/assets/1b3ec47b-a990-4f27-af10-dfd637192011)
#### 11. Все поля обязательны для заполнения (required)
#### 12. После нажатия на кнопку Confirm проверьте все поля и обозначте пользователю.
![image](https://github.com/user-attachments/assets/237b459e-3c55-4cfa-a4dc-bcd0020e2e67)
#### 13. Реализуйте с помощью JavaScript движение солнца и луны (для решения задачи получайте текущее время)

---
# Шаблоны для упрощения создания базовой структуры
1. **Структура проекта**
![image](https://github.com/user-attachments/assets/4e780032-7763-479e-b4ee-8d42f3c1cd61)
2. **HTML**
- *Head*
- ![image](https://github.com/user-attachments/assets/223833cf-9aa4-49fb-bc62-1ea7135a4119)
- *Sun*, *Moon*, *Select Gender*
- ![image](https://github.com/user-attachments/assets/c9c50baf-8df1-4078-9177-af1c87720ce8)
- *Day of week* and *Time*
- ![image](https://github.com/user-attachments/assets/e3fbeb61-3c52-46f9-8174-3eb50b29593d)
- *Select region*, *Select town*, *Confirm*, *Result* (For notify user)
- ![image](https://github.com/user-attachments/assets/d6a1f251-3264-44a1-8199-3e0844496c39)

3. **CSS**
- *body*, *inputs*, *selects*, *labels*
- ![image](https://github.com/user-attachments/assets/2cde6d49-a269-4335-8aaa-66d2aeb8b62b)
- *result*, *red and green styles*, *base for sun and moon*
- ![image](https://github.com/user-attachments/assets/5ccbab78-83d4-41af-8fb1-ebb1a36727f7)
- *sun* and *moon*
- ![image](https://github.com/user-attachments/assets/5905add8-da8a-4ce4-aed9-88ae777d6f6f)

4. **JavaScript**
- ![image](https://github.com/user-attachments/assets/10dc1d46-a6ae-48fc-8856-e1ab34ec6323)
- ![image](https://github.com/user-attachments/assets/f89b5893-88e4-4e1e-a8c1-ac36b2648f15)
- ![image](https://github.com/user-attachments/assets/5b12515c-ebde-42e3-80ff-04d01d02bcca)
- ![image](https://github.com/user-attachments/assets/194e4dd9-713c-489e-afa9-73fb517d4795)
- ![image](https://github.com/user-attachments/assets/8bd5e186-1a61-4dd7-9612-ad0b594e076c)
- ![image](https://github.com/user-attachments/assets/45fc4de1-7b0e-4a8e-9297-38c70e00e5e2)
- ![image](https://github.com/user-attachments/assets/269389b6-a364-43d3-b7e0-b7800b6eef2a)
- ![image](https://github.com/user-attachments/assets/bfc966ea-c35e-459f-b477-92c45aed7f3c)
- ![image](https://github.com/user-attachments/assets/0bab6ce0-b90b-424b-af91-d316a3562c9d)

---
# Основные задачи 
1. **Сделайте страницу для демонстрации комплектов одежды (show.html)** Напишите функцию которая принимает в себя JS объек (
и в зависимости от этих двух параметров показывает соответствующий комплект одежды.):
```json
{
"gender":"man",
"temp": 15.6,
}
```
2. **Добавьте звезды для ночи и облака для дня**
3. **Добавьте мерцание для этого объекта** ![image](https://github.com/user-attachments/assets/f41cfe10-bccc-454a-8873-0996929bb6c5)
4. **Настройти стили чтобы обе страницы соответствовал разработанному дизайну**
5. **Проект нужно выгрузить в Github и задиплоить в GitHub Pages.**
---
### Оценивание
1. Макет в фигме - 10%
2. Выполнить шаги 1-13 - 25%
3. Основные задачи (не учитываются без 2го пункта ^ ):
   - 1(show.html) - 20%
   - 2(Облака и звезды) - 10%
   - 3(Мерцание) - 15%
   - 4(ГОСТ) - 10%
   - 5(GitHub Pages) - 10%

### 91% - оценка 5
### 66% - оценка 4
### 33% - оценка 3












