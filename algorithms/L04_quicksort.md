# **Лабораторная работа №4. Быстрая сортировка**

> **Цель работы**: Изучить алгоритм быстрой сортировки (quicksort), понять принцип «разделяй и властвуй», реализовать рекурсивный алгоритм, оценить его эффективность и поведение в разных случаях.

---

## **Теория**

В главе 4 книги *"Грокаем алгоритмы"* рассматривается **быстрая сортировка (quicksort)** — эффективный рекурсивный алгоритм сортировки, основанный на принципе **«разделяй и властвуй»**.

### Как работает quicksort:
1. Выбирается **опорный элемент** (pivot) — например, первый или средний.
2. Массив **разделяется** на две части:
   - Элементы **меньше или равны** опорному.
   - Элементы **больше** опорного.
3. Рекурсивно применяется quicksort к обеим частям.
4. Результат: `left + [pivot] + right`.

### Временная сложность:
- **Средний случай**: O(n log n)
- **Худший случай**: O(n²) — если опорный элемент всегда самый маленький или самый большой.
- **Преимущество**: На практике часто быстрее других O(n log n) алгоритмов.

---

## **Практическая часть**

### 1. Создайте рабочий файл
- Создайте папку `lab4_quicksort`.
- Внутри — файл `quicksort.py`.
- Откройте его в редакторе.

### 2. Реализуйте базовую версию быстрой сортировки
Напишите функцию `quicksort(arr)`, которая:
- Если длина массива ≤ 1 — возвращает его (базовый случай).
- Выбирает **первый элемент** как опорный (`pivot`).
- Разделяет остальные элементы на `less` и `greater`.
- Рекурсивно сортирует обе части и возвращает объединённый результат.

```python
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    else:
        pivot = arr[0]
        less = [x for x in arr[1:] if x <= pivot]
        greater = [x for x in arr[1:] if x > pivot]
        return quicksort(less) + [pivot] + quicksort(greater)
```

### 3. Протестируйте алгоритм
Добавьте тесты:

```python
data = [10, 5, 2, 3, 7, 6, 8, 9, 1, 4]
print("Исходный:", data)
print("Отсортированный:", quicksort(data))
# Ожидается: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

Запустите — убедитесь, что работает.

### 4. Добавьте подсчёт операций
Модифицируем функцию, чтобы считать количество **сравнений**:

```python
def quicksort_with_count(arr, comparisons=0):
    if len(arr) <= 1:
        return arr, comparisons
    else:
        pivot = arr[0]
        less = []
        greater = []
        # Подсчитываем сравнения при разделении
        for x in arr[1:]:
            comparisons += 1
            if x <= pivot:
                less.append(x)
            else:
                greater.append(x)
        sorted_less, comparisons = quicksort_with_count(less, comparisons)
        sorted_greater, comparisons = quicksort_with_count(greater, comparisons)
        return sorted_less + [pivot] + sorted_greater, comparisons
```

Тест:

```python
data = [10, 5, 2, 3, 7, 6, 8, 9, 1, 4]
result, comp_count = quicksort_with_count(data)
print("Результат:", result)
print("Всего сравнений:", comp_count)
```

### 5. Сравните с разными входными данными
Протестируйте на:
- Уже отсортированном массиве: `list(range(1, 11))`
- Обратном порядке: `list(range(10, 0, -1))`
- Случайном: `[3, 1, 4, 1, 5, 9, 2, 6]`

```python
datasets = [
    list(range(1, 11)),
    list(range(10, 0, -1)),
    [3, 1, 4, 1, 5, 9, 2, 6]
]

for data in datasets:
    _, comp = quicksort_with_count(data.copy())
    print(f"Для {data}: {comp} сравнений")
```

> ⚠️ На отсортированных данных — больше сравнений (худший случай).

### 6. Улучшите выбор опорного элемента
Создайте функцию `quicksort_random_pivot(arr)`, которая выбирает **случайный** опорный элемент для улучшения средней производительности:

```python
import random

def quicksort_random_pivot(arr):
    if len(arr) <= 1:
        return arr
    else:
        # Случайный выбор опорного
        pivot_idx = random.randint(0, len(arr) - 1)
        pivot = arr[pivot_idx]
        # Остальные элементы
        left = [x for i, x in enumerate(arr) if x <= pivot and i != pivot_idx]
        right = [x for i, x in enumerate(arr) if x > pivot and i != pivot_idx]
        return quicksort_random_pivot(left) + [pivot] + quicksort_random_pivot(right)
```

Тест:

```python
data = [10, 5, 2, 3, 7, 6, 8, 9, 1, 4]
print("Случайный pivot:", quicksort_random_pivot(data))
```

> Запускайте несколько раз — результат одинаковый, но структура дерева вызовов меняется.

### 7. Сравните с встроенной сортировкой
```python
print("Python sorted():", sorted(data))
```

Убедитесь, что результаты совпадают.

---

## **Самостоятельная часть** (оценивается на **5 баллов**)

### 1. Реализуйте in-place быструю сортировку (усложнённо)
Напишите функцию `quicksort_inplace(arr, low, high)`, которая сортирует массив **без создания новых списков** (экономит память).

```python
def partition(arr, low, high):
    pivot = arr[high]  # опорный — последний
    i = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

def quicksort_inplace(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quicksort_inplace(arr, low, pi - 1)
        quicksort_inplace(arr, pi + 1, high)
```

Тест:
```python
data = [10, 5, 2, 3, 7, 6, 8, 9, 1, 4]
quicksort_inplace(data, 0, len(data) - 1)
print("In-place результат:", data)
```

### 2. Анализ производительности
Создайте массив из 100 случайных чисел: `random.sample(range(1, 200), 100)`.  
Отсортируйте его с помощью:
- Вашего `quicksort()`
- `sorted()`

Измерьте время:

```python
import time
start = time.time()
quicksort(data.copy())
print("Время quicksort:", time.time() - start)
```

Сделайте вывод: почему `sorted()` быстрее?

---

## **Контрольные вопросы** (ответы устно)

1. В чём суть принципа «разделяй и властвуй»? Как он применяется в быстрой сортировке?  
2. Почему худший случай быстрой сортировки — O(n²)? Когда он возникает?

---

## **Слепая печать**
1. Пройдите два урока слепой печати на [Python](https://stamina-online.com/ru/workout/programming/15)

---

## **Критерии оценивания**

| Часть работы              | Оценка | Комментарий |
|--------------------------|--------|-------------|
| Практическая часть       | 3      | Все шаги выполнены, код работает |
| Слепая печать            | 4      | Подтверждено выполнение |
| Самостоятельная часть    | 5      | Задачи решены, особенно in-place версия — глубокое понимание |
