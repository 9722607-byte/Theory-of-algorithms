## Лістинг 2.1 - Програма пошуку найкоротших шляхів за алгоритмом Дейкстри (C++)
```python
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

const int INF = 1e9;

void dijkstra(int start, const vector<vector<pair<int, int>>>& adj, int V) {
    // Ініціалізація масивів найкоротших відстаней та попередників
    vector<int> dist(V, INF);
    vector<int> pred(V, -1);
    
    // Черга з пріоритетами (min-heap): пара (відстань, вершина)
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

    dist[start] = 0;
    pq.push({0, start});

    while (!pq.empty()) {
        int d = pq.top().first;
        int u = pq.top().second;
        pq.pop();

        // Пропускаємо застарілий запис у черзі
        if (d > dist[u]) continue;

        // Перебір усіх суміжних вершин v для u
        for (auto& edge : adj[u]) {
            int v = edge.first;
            int weight = edge.second;

            // Релаксація ребра (u, v)
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pred[v] = u;
                pq.push({dist[v], v});
            }
        }
    }

    // Вивід результатів роботи програми
    cout << "З вершини\tВ вершину\tВага\tШлях" << endl;
    for (int i = 0; i < V; i++) {
        if (i == start) continue;
        cout << start + 1 << "\t\t" << i + 1 << "\t\t" << dist[i] << "\t";
        
        // Відновлення шляху через масив pred[]
        vector<int> path;
        for (int curr = i; curr != -1; curr = pred[curr]) {
            path.push_back(curr + 1);
        }
        for (int j = path.size() - 1; j >= 0; j--) {
            cout << path[j] << (j == 0 ? "" : " -> ");
        }
        cout << endl;
    }
}

int main() {
    int V = 8; // Кількість вершин графа за Варіантом 6
    vector<vector<pair<int, int>>> adj(V);

    // Додавання неорієнтованих зважених ребер (індексація 0..7)
    auto addEdge = [&](int u, int v, int w) {
        adj[u].push_back({v, w});
        adj[v].push_back({u, w});
    };

    addEdge(0, 1, 2); // (1,2) = 2
    addEdge(0, 3, 1); // (1,4) = 1
    addEdge(0, 2, 5); // (1,3) = 5
    addEdge(0, 6, 1); // (1,7) = 1
    addEdge(1, 3, 3); // (2,4) = 3
    addEdge(2, 4, 2); // (3,5) = 2
    addEdge(2, 5, 4); // (3,6) = 4
    addEdge(2, 6, 4); // (3,7) = 4
    addEdge(3, 4, 9); // (4,5) = 9
    addEdge(3, 5, 5); // (4,6) = 5
    addEdge(3, 6, 9); // (4,7) = 9
    addEdge(4, 5, 3); // (5,6) = 3
    addEdge(5, 6, 6); // (6,7) = 6
    addEdge(5, 7, 1); // (6,8) = 1

    dijkstra(0, adj, V); // Пошук від вершини 1 (індекс 0)
    return 0;
}
```

## Лістинг 4.1 - Програма побудови матриці найкоротших відстаней за алгоритмом Флойда (C++)
```python
#include <iostream>
#include <vector>
#include <iomanip>

using namespace std;

const int INF = 1e9;

void floydWarshall(int V, vector<vector<int>>& D) {
    // Послідовний перебір проміжних вершин k від 0 до V-1
    for (int k = 0; k < V; k++) {
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                // Перевірка наявності шляхів i->k та k->j та оновлення мінімуму
                if (D[i][k] < INF && D[k][j] < INF) {
                    if (D[i][k] + D[k][j] < D[i][j]) {
                        D[i][j] = D[i][k] + D[k][j];
                    }
                }
            }
        }
    }

    // Вивід фінальної матриці відстаней D^(8)
    cout << "Матриця найкоротших відстаней D:" << endl;
    cout << setw(5) << " ";
    for (int i = 0; i < V; i++) cout << setw(5) << i + 1;
    cout << endl;

    for (int i = 0; i < V; i++) {
        cout << setw(5) << i + 1;
        for (int j = 0; j < V; j++) {
            if (D[i][j] == INF)
                cout << setw(5) << "INF";
            else
                cout << setw(5) << D[i][j];
        }
        cout << endl;
    }
}

int main() {
    int V = 8; // Кількість вершин для Варіанту 6
    vector<vector<int>> W(V, vector<int>(V, INF));

    // Нулі на головній діагоналі
    for (int i = 0; i < V; i++) W[i][i] = 0;

    // Заповнення початкової вагової матриці графа
    auto addEdge = [&](int u, int v, int w) {
        W[u][v] = w;
        W[v][u] = w;
    };

    addEdge(0, 1, 2); // (1,2) = 2
    addEdge(0, 3, 1); // (1,4) = 1
    addEdge(0, 2, 5); // (1,3) = 5
    addEdge(0, 6, 1); // (1,7) = 1
    addEdge(1, 3, 3); // (2,4) = 3
    addEdge(2, 4, 2); // (3,5) = 2
    addEdge(2, 5, 4); // (3,6) = 4
    addEdge(2, 6, 4); // (3,7) = 4
    addEdge(3, 4, 9); // (4,5) = 9
    addEdge(3, 5, 5); // (4,6) = 5
    addEdge(3, 6, 9); // (4,7) = 9
    addEdge(4, 5, 3); // (5,6) = 3
    addEdge(5, 6, 6); // (6,7) = 6
    addEdge(5, 7, 1); // (6,8) = 1

    floydWarshall(V, W);
    return 0;
}
```
