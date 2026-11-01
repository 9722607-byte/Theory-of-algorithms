## 3. Програмна реалізація та результат алгоритму Прима (Завдання 3)
Лістинг 3.1 - Програма алгоритму Прима
```python
#include <iostream>
#include <vector>
#include <cstring>

using namespace std;

#define INF 9999999
#define V 8 // Кількість вершин для Варіанту 6

// Матриця суміжності графа (0 - відсутність петлі, INF - відсутність ребра)
int G[V][V] = {
    {0, 2, 5, 1, INF, INF, 1, INF},
    {2, 0, INF, 3, INF, INF, INF, INF},
    {5, INF, 0, INF, 2, 4, 4, INF},
    {1, 3, INF, 0, 9, 5, 9, INF},
    {INF, INF, 2, 9, 0, 3, INF, INF},
    {INF, INF, 4, 5, 3, 0, 6, 1},
    {1, INF, 4, 9, INF, 6, 0, INF},
    {INF, INF, INF, INF, INF, 1, INF, 0}
};

int main() {
    int no_edge = 0; // Лічильник вибраних ребер у МКД
    bool selected[V]; // Масив для відстеження включених у дерево вершин
    
    // Ініціалізуємо всі вершини як ще не відвідані
    memset(selected, false, sizeof(selected));
    
    // Починаємо побудову з вершини 0 (що відповідає вершині 1 у варіанті)
    selected[0] = true;

    cout << "Edge : Weight" << endl;
    int total_weight = 0;

    // Шукаємо V-1 ребер для побудови кістякового дерева
    while (no_edge < V - 1) {
        int min = INF;
        int x = 0, y = 0;

        // Перебираємо всі відвідані вершини
        for (int i = 0; i < V; i++) {
            if (selected[i]) {
                // Для кожної відвіданої вершини шукаємо найдешевше ребро до невідвіданої
                for (int j = 0; j < V; j++) {
                    if (!selected[j] && G[i][j]) {
                        if (min > G[i][j]) {
                            min = G[i][j];
                            x = i;
                            y = j;
                        }
                    }
                }
            }
        }
        
        // Виводимо обране мінімальне ребро (+1 для переведення 0-індексації в 1-індексацію)
        cout << x + 1 << " - " << y + 1 << " : " << G[x][y] << endl;
        total_weight += G[x][y];
        
        // Позначаємо нову вершину як включену до дерева
        selected[y] = true;
        no_edge++;
    }
    
    cout << "Total Weight: " << total_weight << endl;
    return 0;
}
```

## 5. Програмна реалізація та результат алгоритму Крускала (Завдання 5)
Лістинг 5.1 - Програма алгоритму Крускала
```python
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Визначаємо тип ребра як пару (початкова_вершина, кінцева_вершина)
#define edge pair<int, int>

class Graph {
private:
    vector<pair<int, edge>> G; // Граф: вектор пар (вага, пара(u, v))
    vector<pair<int, edge>> T; // Ребра побудованого МКД
    int *parent;               // Масив предків для системи непересічних множин (DSU)
    int V;                     // Кількість вершин

public:
    Graph(int V);
    void AddWeightedEdge(int u, int v, int w);
    int find_set(int i);
    void union_set(int u, int v);
    void kruskal();
    void print();
};

// Конструктор: ініціалізація DSU (кожна вершина є власному предком)
Graph::Graph(int V) {
    this->V = V;
    parent = new int[V];
    for (int i = 0; i < V; i++)
        parent[i] = i;
    G.clear();
    T.clear();
}

// Додавання зваженого ребра до списку ребер графа
void Graph::AddWeightedEdge(int u, int v, int w) {
    G.push_back(make_pair(w, edge(u, v)));
}

// Пошук представника множини (знаходження кореня) з рекурсивним ущільненням шляху
int Graph::find_set(int i) {
    if (i == parent[i])
        return i;
    return parent[i] = find_set(parent[i]);
}

// Об'єднання двох множин
void Graph::union_set(int u, int v) {
    int root_u = find_set(u);
    int root_v = find_set(v);
    parent[root_u] = root_v;
}

// Основний алгоритм Крускала
void Graph::kruskal() {
    int uRep, vRep;
    
    // Сортуємо ребра за зростанням їхньої ваги
    sort(G.begin(), G.end());
    
    for (int i = 0; i < G.size(); i++) {
        // Знаходимо компоненти зв'язності для кінців поточного ребра
        uRep = find_set(G[i].second.first);
        vRep = find_set(G[i].second.second);
        
        // Якщо вершини в різних компонентах — додавання ребра не утворить цикл
        if (uRep != vRep) {
            T.push_back(G[i]);
            union_set(uRep, vRep);
        }
    }
}

// Вивід результатів побудови МКД
void Graph::print() {
    cout << "Edge : Weight" << endl;
    int total_weight = 0;
    for (int i = 0; i < T.size(); i++) {
        // Додаємо +1 до індексів вершин для відповідності позначенням (1-8)
        cout << T[i].second.first + 1 << " - " << T[i].second.second + 1 << " : " << T[i].first << endl;
        total_weight += T[i].first;
    }
    cout << "Total Weight: " << total_weight << endl;
}

int main() {
    // Граф для Варіанту 6 (8 вершин: 0..7)
    Graph g(8);
    
    // Заповнення графа ребрами
    g.AddWeightedEdge(0, 1, 2);
    g.AddWeightedEdge(0, 3, 1);
    g.AddWeightedEdge(0, 2, 5);
    g.AddWeightedEdge(0, 6, 1);
    g.AddWeightedEdge(1, 3, 3);
    g.AddWeightedEdge(2, 4, 2);
    g.AddWeightedEdge(2, 5, 4);
    g.AddWeightedEdge(2, 6, 4);
    g.AddWeightedEdge(3, 4, 9);
    g.AddWeightedEdge(3, 5, 5);
    g.AddWeightedEdge(3, 6, 9);
    g.AddWeightedEdge(4, 5, 3);
    g.AddWeightedEdge(5, 6, 6);
    g.AddWeightedEdge(5, 7, 1);

    // Запуск алгоритму та друк МКД
    g.kruskal();
    g.print();

    return 0;
}
```
