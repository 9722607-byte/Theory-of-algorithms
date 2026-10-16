# Константа розміру таблиці
M = 13
WORDS = ["Скільки", "вовка", "не", "годуй", "а", "він", "усе", "в", "ліс", "дивиться"]

# Словник позицій українського алфавіту
UKR_POSITIONS = {
    'а': 1, 'б': 2, 'в': 3, 'г': 4, 'ґ': 5, 'д': 6, 'е': 7, 'є': 8, 'ж': 9, 'з': 10,
    'и': 11, 'і': 12, 'ї': 13, 'й': 14, 'к': 15, 'л': 16, 'м': 17, 'н': 18, 'о': 19, 'п': 20,
    'р': 21, 'с': 22, 'т': 23, 'у': 24, 'ф': 25, 'х': 26, 'ц': 27, 'ч': 28, 'ш': 29, 'щ': 30,
    'ь': 31, 'ю': 32, 'я': 33
}

```
def simple_hash(key: str) -> int:
    """Хеш-функція методом ділення: h(k) = (сума позицій букв) mod 13."""
    sum_pos = sum(UKR_POSITIONS.get(char.lower(), 0) for char in key)
    return sum_pos % M

def build_open_hash_table(words: list, m: int) -> list:
    """Будує відкриту хеш-таблицю з ланцюжками (писами)."""
    hash_table = [[] for _ in range(m)]
    for word in words:
        address = simple_hash(word)
        hash_table[address].append(word)
    return hash_table

def display_open_hash_table(table: list):
    """Виводить хеш-таблицю з ланцюжками у зручному форматі."""
    print(f"\n--- Відкрита хеш-таблиця (Ланцюжки, M={M}) ---")
    for i, chain in enumerate(table):
        print(f"Індекс {i:02d}: {chain}")

# Виконання
if __name__ == "__main__":
    open_table = build_open_hash_table(WORDS, M)
    display_open_hash_table(open_table)
[cite: 37, 38]
```

---

### 2. Код для Лабораторної роботи №4 - Закрите хешування 
Цей файл реалізує метод відкритої адресації з використанням лінійного дослідження (*linear probing*) для вирішення колізій у фіксованій таблиці[cite: 38, 40].

```python
def build_closed_hash_table(words: list, m: int) -> list:
    """Будує закриту хеш-таблицю з відкритою адресацією (лінійне дослідження)."""
    hash_table = [None] * m
    for word in words:
        start_address = simple_hash(word)
        inserted = False
        for i in range(m):
            address = (start_address + i) % m
            if hash_table[address] is None:
                hash_table[address] = word
                inserted = True
                break
        if not inserted:
            print(f"Помилка: Таблиця заповнена. Не вдалося додати слово: [{word}]")
    return hash_table

def display_closed_hash_table(table: list):
    """Виводить закриту хеш-таблицю у форматі таблиці."""
    print(f"\n--- Закрита хеш-таблиця (Відкрита адресація, M={M}) ---")
    print("Індекс | Слово")
    print("-" * 15)
    for i, item in enumerate(table):
        val = item if item is not None else "(NULL)"
        print(f"{i:02d}     | {val}")

# Виконання
if __name__ == "__main__":
    closed_table = build_closed_hash_table(WORDS, M)
    display_closed_hash_table(closed_table)
```[cite: 41, 42]
