# Mengamati Semut

## Deskripsi

Fae sedang mengamati $N$ semut yang sedang berjalan di sebuah batang pohon. Semut-semut tersebut dinomori dari $1$ sampai $N$. Fae melakukan $M$ kali pengamatan. Pengamatan ke-$i$ menghasilkan kesimpulan bahwa semut $X_i$ lebih cepat $Z_i$ mm/s dari semut $Y_i$. Apabila $Z_i=0$ berarti kecepatan semut $X_i$ dan semut $Y_i$ sama.

Fae penasaran tentang berapa selisih absolut antara kecepatan semut $1$ dan semut $N$. Bantulah Fae untuk mendapatkan nilainya atau beri tahu jika nilainya belum bisa disimpulkan!

## Batasan
- $2 \leq N \leq 30\ 000$
- $1 \leq M \leq min( \frac{N-(N-1)}{2}, 30\ 000)$
- $1 \leq X_i,Y_i \leq N$
- $X_i \ne Y_i$, seluruh pasangan $(X_i,Y_i)$ berbeda.
- $0 \leq X_i \leq 30\ 000$
- Dijamin tidak ada informasi yang saling kontradiksi.

## Subsoal
- `(14 poin)` Hanya berisi kasus uji berikut.
    ```
    5 3
    1 3 10
    2 4 10
    3 5 10
    ```
- `(14 poin)` Hanya berisi kasus uji berikut.
    ```
    10 9
    1 2 6
    1 3 3
    2 4 8
    4 5 5
    4 6 1
    5 7 2
    6 8 7
    6 9 9
    7 10 4
    ```
- `(20 poin)` $M=N-1, X_i=i, Y_i=i+1$
- `(10 poin)` $M=N-1, X_i<Y_i, Y_i=i+1$
- `(20 poin)` $Z_i=0$
- `(22 poin)` Tidak ada batasan tambahan.

## Masukan
Masukan diberikan dalam format berikut.

```
N M
X1 Y1 Z1
...
Xm Ym Zm
```

## Keluaran
Sebuah baris berisi sebuah bilangan yang menyatakan selisih absolut kecepatan atau `-1` apabila belum bisa disimpulkan.

## Contoh Masukan
```
4 3
1 2 5
2 3 2
4 2 3
```

## Contoh Keluaran
```
2
```

## Penjelasan Contoh
Semut $1$ adalah yang tercepat dengan selisih kecepatan dengan semut lain sebagai berikut.
- Dengan semut $2$, selisih $5$ mm/s
- Dengan semut $3$, selisih $7$ mm/s
- Dengan semut $4$, selisih $2$ mm/s

<detail>
<summary><h2>Proposed Solution</h2></summary>

Penyelesaian soal ini menggunakan Graf dan BFS.

- Graf diimplementasikan sebagai daftar adjacency $(graph)$ dengan $(neighbor,diff)$ menyatakan hubungan kecepatan semut:
    - $(diff)$: selisih kecepatan antara semut $x$ dan $y$.
- BFS digunakan untuk menghitung kecepatan relatif setiap semut terhadap semut $1$.
- Setiap kali mengunjungi node, jarak (kecepatan relatif) dihitung dan disimpan dalam array $distances$.
- Jika ada kontradiksi (dua jalur memberikan nilai berbeda untuk kecepatan semut), langsung keluarkan $-1$. Ini memang tidak akan terjadi karena dijamin tidak ada kontradiksi (di soal), tapi tidak ada salahnya lebih siap dari awal.
- Jika tidak ada kontradiksi, selisih absolut antara kecepatan semut $1$ dan $N$ dihitung sebagai $|distances[N]-distances[1]|$.

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
using namespace std;

struct Observation {
    int x, y, z;
};

bool bfs(vector<vector<pair<int, int>>>& graph, vector<int>& distances, int N) {
    queue<int> q;
    vector<bool> visited(N + 1, false);
    distances[1] = 0;
    q.push(1);
    visited[1] = true;

    while (!q.empty()) {
        int current = q.front();
        q.pop();

        for (auto& [neighbor, diff] : graph[current]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                distances[neighbor] = distances[current] + diff;
                q.push(neighbor);
            } else if (distances[neighbor] != distances[current] + diff) {
                return false; // Contradiction detected
            }
        }
    }
    return true;
}

int main() {
    int N, M;
    cin >> N >> M;

    vector<vector<pair<int, int>>> graph(N + 1);

    for (int i = 0; i < M; ++i) {
        int x, y, z;
        cin >> x >> y >> z;
        graph[x].emplace_back(y, z);
        graph[y].emplace_back(x, -z);
    }

    vector<int> distances(N + 1, INT_MAX);

    if (!bfs(graph, distances, N)) {
        cout << -1 << endl;
    } else {
        cout << abs(distances[N] - distances[1]) << endl;
    }

    return 0;
}
```

</detail>