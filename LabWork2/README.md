
## Лістинг 2.1 - Python-код ітеративної реалізації сортування злиттям

```python
def merge_iterative(a, left, mid, right):
    comparisons = 0
    assignments = 0
    n1 = mid - left
    n2 = right - mid
    
    L = a[left:mid]
    R = a[mid:right]
    assignments += (n1 + n2)
    
    it1 = 0
    it2 = 0
    k = left
    assignments += 3
    
    # Злиття двох підмасивів
    while it1 < n1 and it2 < n2:
        comparisons += 1
        if L[it1] < R[it2]:
            a[k] = L[it1]
            it1 += 1
        else:
            a[k] = R[it2]
            it2 += 1
        k += 1
        assignments += 2
        
    # Докопіювання залишків
    while it1 < n1:
        a[k] = L[it1]
        it1 += 1
        k += 1
        assignments += 2
        
    while it2 < n2:
        a[k] = R[it2]
        it2 += 1
        k += 1
        assignments += 2
        
    return comparisons, assignments


def merge_sort_iterative(a):
    n = len(a)
    comparisons = 0
    assignments = 0
    i = 1
    
    while i < n:
        j = 0
        while j < n - i:
            left = j
            mid = j + i
            right = min(j + 2 * i, n)
            
            c, a_count = merge_iterative(a, left, mid, right)
            comparisons += c
            assignments += a_count
            j += 2 * i
        i *= 2
        
    return a, comparisons, assignments


# Приклад запуску для Варіанту 6
if __name__ == "__main__":
    my_list = [58, 5, 50, 99, 61, 32, 27, 45, 75]
    sorted_list, comps, assigs = merge_sort_iterative(my_list.copy())
    
    print("Оригінальний список:", my_list)
    print("Відсортований список:", sorted_list)
    print(f"Кількість порівнянь: {comps}")
    print(f"Кількість присвоювань: {assigs}")
```


## Лістинг 2.2 - Python-код рекурсивної реалізації сортування злиттям
```python
def merge(left, right):
    merged_arr = []
    comparisons = 0
    assignments = 0
    i = 0
    j = 0
    
    # Злиття двох відсортованих списків
    while i < len(left) and j < len(right):
        comparisons += 1
        if left[i] <= right[j]:
            merged_arr.append(left[i])
            i += 1
        else:
            merged_arr.append(right[j])
            j += 1
        assignments += 1
        
    # Додавання залишків з лівого списку
    while i < len(left):
        merged_arr.append(left[i])
        i += 1
        assignments += 1
        
    # Додавання залишків з правого списку
    while j < len(right):
        merged_arr.append(right[j])
        j += 1
        assignments += 1
        
    return merged_arr, comparisons, assignments


def merge_sort_recursive(arr):
    comparisons = 0
    assignments = 0
    recursive_calls = 1
    
    # Базовий випадок рекурсії
    if len(arr) <= 1:
        return arr, comparisons, assignments, recursive_calls
        
    mid = len(arr) // 2
    assignments += 1
    
    # Рекурсивний поділ масиву на дві частини
    left_half, c1, a1, r1 = merge_sort_recursive(arr[:mid])
    right_half, c2, a2, r2 = merge_sort_recursive(arr[mid:])
    
    comparisons += (c1 + c2)
    assignments += (a1 + a2)
    recursive_calls += (r1 + r2)
    
    # Злиття відсортованих половин
    merged_arr, c_merge, a_merge = merge(left_half, right_half)
    comparisons += c_merge
    assignments += a_merge
    
    return merged_arr, comparisons, assignments, recursive_calls


# Приклад запуску для Варіанту 6
if __name__ == "__main__":
    my_list = [58, 5, 50, 99, 61, 32, 27, 45, 75]
    sorted_list, comps, assigs, rec_calls = merge_sort_recursive(my_list)
    
    print("Оригінальний список:", my_list)
    print("Відсортований список:", sorted_list)
    print(f"Загальна кількість порівнянь: {comps}")
    print(f"Загальна кількість присвоювань: {assigs}")
    print(f"Загальна кількість рекурсивних викликів: {rec_calls}")
```
## Лістинг 2.3 - Python-код рекурсивної реалізації швидкого сортування за схемою Хоара

```python
def partition(a, l, r):
    """
    Процедура розбиття масиву за схемою Хоара.
    Вибирає опорний елемент (pivot) та переставляє елементи так,
    щоб менші були зліва, а більші — справа.
    """
    comparisons = 0
    assignments = 0
    
    # Вибір опорного елемента (перший елемент діапазону)
    pivot = a[l]
    assignments += 1
    
    i = l - 1
    j = r + 1
    assignments += 2
    
    while True:
        # Рух лівого індексу вправо до елемента, більшого або рівного pivot
        i += 1
        assignments += 1
        while a[i] < pivot:
            comparisons += 1
            i += 1
            assignments += 1
        comparisons += 1
        
        # Рух правого індексу вліво до елемента, меншого або рівного pivot
        j -= 1
        assignments += 1
        while a[j] > pivot:
            comparisons += 1
            j -= 1
            assignments += 1
        comparisons += 1
        
        comparisons += 1
        # Якщо індекси зійшлися або перетнулися — завершуємо розбиття
        if i >= j:
            return j, comparisons, assignments
            
        # Обмін елементів місцями
        a[i], a[j] = a[j], a[i]
        assignments += 3


def quicksort(a, l, r):
    """
    Рекурсивна функція швидкого сортування (Quick Sort).
    """
    comparisons = 0
    assignments = 0
    recursive_calls = 1
    
    if l < r:
        # Отримуємо індекс розбиття та лічильники операцій
        q, c1, a1 = partition(a, l, r)
        comparisons += c1
        assignments += a1
        
        # Рекурсивні виклики для лівої та правої частини
        c2, a2, r2 = quicksort(a, l, q)
        c3, a3, r3 = quicksort(a, q + 1, r)
        
        comparisons += (c2 + c3)
        assignments += (a2 + a3)
        recursive_calls += (r2 + r3)
        return comparisons, assignments, recursive_calls
    else:
        return 0, 0, 1


# --- Приклад запуску для Варіанту 6 ---
if __name__ == "__main__":
    my_list = [58, 5, 50, 99, 61, 32, 27, 45, 75]
    original_list = my_list.copy()
    
    total_comps, total_assigs, total_calls = quicksort(my_list, 0, len(my_list) - 1)
    
    print("Оригінальний список:", original_list)
    print("Відсортований список:", my_list)
    print(f"Загальна кількість порівнянь: {total_comps}")
    print(f"Загальна кількість присвоювань: {total_assigs}")
    print(f"Загальна кількість рекурсивних викликів: {total_calls}")
```
