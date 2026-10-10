```
def swap(arr, i, j):
    """Міняє місцями два елементи в масиві."""
    arr[i], arr[j] = arr[j], arr[i]

def sink(arr, i, n):
    """Процедура 'занурення' елемента вниз по купі."""
    k = i
    while True:
        j = 2 * k + 1  # Індекс лівого дочірнього елемента
        if j >= n:
            break
        # Знаходимо індекс найбільшого дочірнього елемента
        if j + 1 < n and arr[j + 1] > arr[j]:
            j += 1
        # Якщо поточний елемент більший або дорівнює найбільшому дочірньому
        if arr[k] >= arr[j]:
            break
        swap(arr, k, j)
        k = j

def heapsort(arr):
    """Алгоритм пірамідального сортування (Heapsort)."""
    n = len(arr)
    
    # Фаза 1: Побудова максимальної купи
    for i in range(n // 2 - 1, -1, -1):
        sink(arr, i, n)
        
    # Фаза 2: Сортування
    for i in range(n - 1, 0, -1):
        swap(arr, 0, i)  # Переносимо корінь (найбільший) у кінець
        sink(arr, 0, i)  # Відновлюємо властивості купи для зменшеного масиву
        
    return arr

# Моделювання для Варіанту 6
A = [58, 5, 50, 99, 61, 32, 27, 45, 75]
print("Початковий масив:", A)
sorted_A = heapsort(A.copy())
print("Відсортований масив:", sorted_A)
[cite: 25]
```

#### Графічне представлення для Рисунка 3.9 та 3.10

```dot
digraph HeapTree {
    node [shape=circle, style="filled", fillcolor="#e0f7fa", fontname="Arial", fontsize=11, width=0.6];
    edge [color="#00838f", arrowhead=none];

    // Максимальна купа після Фази 1 (Корінь 99)
    root [label="99", fillcolor="#80deea"];
    n1 [label="75"];
    n2 [label="50"];
    n3 [label="58"];
    n4 [label="61"];
    n5 [label="32"];
    n6 [label="27"];
    n7 [label="45"];
    n8 [label="5"];

    root -> n1;
    root -> n2;
    
    n1 -> n3;
    n1 -> n4;
    
    n2 -> n5;
    n2 -> n6;
    
    n3 -> n7;
    n3 -> n8;
}
