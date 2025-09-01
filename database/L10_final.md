# **Лабораторная работа №10 (2 урока)**  
## **Тема: Итоговая проектная работа. Создание и управление комплексной базой данных**

---

### **1. Немного теории**

На завершающем этапе вы объедините все полученные знания в **единый проект** — создание полноценной базы данных для учебного заведения.

Вы научитесь:
- Проектировать структуру БД (нормализация).
- Создавать таблицы с правильными связями.
- Использовать `JOIN`, `VIEW`, `INDEX`, `GROUP BY`, подзапросы.
- Работать с транзакциями.
- Формировать отчёты на основе данных.

**Цель проекта:**  
Создать базу данных, управляющую студентами, преподавателями, группами, предметами и оценками.

---

### **2. Практическая часть**

**Цель:** Спроектировать и реализовать БД с 5 таблицами и выполнить комплексные запросы.

#### **Шаг 1: Создание новой базы данных**
1. В SQLite Studio нажмите **"Add a database"** → **"Create"**.
2. Сохраните как: `school_final.db`.
3. Подключите её.

#### **Шаг 2: Создание таблицы `Groups`**
1. Создайте таблицу:
   - `id` — INTEGER, PK, Autoincrement
   - `group_name` — TEXT, NOT NULL
   - `specialty` — TEXT
   - `course` — INTEGER

2. Добавьте данные:
```sql
INSERT INTO Groups (group_name, specialty, course) VALUES
('ИТ-101', 'Информационные технологии', 1),
('ИТ-102', 'Информационные технологии', 1),
('ПД-201', 'Программная инженерия', 2);
```

#### **Шаг 3: Создание таблицы `Students`**
1. Поля:
   - `id` — INTEGER, PK, Autoincrement
   - `name` — TEXT, NOT NULL
   - `age` — INTEGER
   - `group_id` — INTEGER, NOT NULL (ссылка на `Groups.id`)

2. Добавьте 4–5 студентов:
```sql
INSERT INTO Students (name, age, group_id) VALUES
('Иван Петров', 20, 1),
('Мария Сидорова', 19, 1),
('Алексей Козлов', 21, 2),
('Елена Фролова', 20, 3),
('Дмитрий Морозов', 22, 1);
```

#### **Шаг 4: Создание таблицы `Subjects`**
1. Поля:
   - `id` — INTEGER, PK, Autoincrement
   - `subject_name` — TEXT, NOT NULL
   - `hours` — INTEGER (количество часов)

2. Данные:
```sql
INSERT INTO Subjects (subject_name, hours) VALUES
('Базы данных', 60),
('Математика', 80),
('Программирование', 100),
('Дизайн интерфейсов', 40);
```

#### **Шаг 5: Создание таблицы `Teachers`**
1. Поля:
   - `id` — INTEGER, PK, Autoincrement
   - `full_name` — TEXT, NOT NULL
   - `subject_id` — INTEGER (ссылка на `Subjects.id`)
   - `experience` — INTEGER

2. Данные:
```sql
INSERT INTO Teachers (full_name, subject_id, experience) VALUES
('Анна Волкова', 1, 7),
('Олег Иванов', 2, 5),
('Марина Петрова', 3, 9),
('Сергей Николаев', 4, 4);
```

#### **Шаг 6: Создание таблицы `Grades` (оценки)**
1. Поля:
   - `id` — INTEGER, PK, Autoincrement
   - `student_id` — INTEGER
   - `subject_id` — INTEGER
   - `grade` — INTEGER (от 2 до 5)
   - `date` — TEXT (формат: '2025-04-01')

2. Данные:
```sql
INSERT INTO Grades (student_id, subject_id, grade, date) VALUES
(1, 1, 5, '2025-04-01'),
(1, 2, 4, '2025-04-02'),
(2, 1, 4, '2025-04-01'),
(2, 3, 5, '2025-04-03'),
(3, 2, 3, '2025-04-02'),
(4, 1, 5, '2025-04-01');
```

#### **Шаг 7: Создание индексов для ускорения**
```sql
CREATE INDEX idx_grades_student ON Grades (student_id);
CREATE INDEX idx_grades_subject ON Grades (subject_id);
CREATE INDEX idx_students_group ON Students (group_id);
```

#### **Шаг 8: Создание VIEW — отчёт "Успеваемость студентов"**
```sql
CREATE VIEW StudentPerformance AS
SELECT 
    s.name AS student,
    sub.subject_name,
    g.grade,
    t.full_name AS teacher
FROM Grades g
INNER JOIN Students s ON g.student_id = s.id
INNER JOIN Subjects sub ON g.subject_id = sub.id
INNER JOIN Teachers t ON sub.id = t.subject_id;
```
Проверьте:
```sql
SELECT * FROM StudentPerformance;
```

#### **Шаг 9: Запрос — средний балл по группам**
```sql
SELECT 
    gr.group_name,
    AVG(g.grade) AS avg_grade
FROM Grades g
INNER JOIN Students s ON g.student_id = s.id
INNER JOIN Groups gr ON s.group_id = gr.id
GROUP BY gr.id;
```

#### **Шаг 10: Транзакция — добавление нового студента и его оценки**
```sql
BEGIN TRANSACTION;

INSERT INTO Students (name, age, group_id) VALUES ('Ольга Кузнецова', 20, 2);
INSERT INTO Grades (student_id, subject_id, grade, date) VALUES (6, 1, 4, '2025-04-05');

-- Если всё хорошо:
COMMIT;
-- Если ошибка — ROLLBACK;
```

---

### **3. Самостоятельная часть**

> Выполняется самостоятельно.

1. Создайте представление `TeacherLoad`, которое показывает:
   - имя преподавателя
   - предмет
   - количество студентов, которым он поставил оценки

2. Найдите **студента с самым высоким средним баллом** (используйте подзапрос).

3. Удалите оценку с `id = 1` в транзакции, но откатите удаление с помощью `ROLLBACK`. Убедитесь, что оценка осталась.

---

### **4. Контрольные вопросы**

1. Почему важно соблюдать связи между таблицами? Что будет при их отсутствии?  
2. Какие элементы вашей БД можно было бы оптимизировать с помощью индексов?

> Ответы дайте устно при защите работы.

---

### **5. Слепая печать**

1. Перейдите на сайт [stamina-online](https://stamina-online.com/ru/programming)
2. Пройдите два урока по [языку программирования SQL](https://stamina-online.com/ru/workout/programming/18)

---

### **6. Критерии оценивания**

| Критерий                  | Оценка | Примечание |
|---------------------------|--------|------------|
| Практическая часть        | 3      | Все таблицы созданы, связи, данные, индексы, VIEW, транзакции работают |
| Слепая печать             | 4      | Работа выполнена |
| Самостоятельная часть     | 5      | VIEW, подзапрос и транзакция реализованы корректно, логика соблюдена |
