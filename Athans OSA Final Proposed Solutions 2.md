# Wawancara Peserta

## Deskripsi
Fae akan melakukan wawancara kepada \( N \) peserta OSA yang dinomori dari \( 1 \) sampai \( N \). Peserta \( i \) datang \( T_i \) menit setelah meja wawancara dibuka. Setiap peserta akan melakukan wawancara selama \( D \) menit tanpa jeda. Dalam satu waktu, Fae hanya bisa mewawancarai \( K \) peserta. Pergantian wawancara dari satu peserta ke peserta lain bisa dilakukan dalam sekejap (tidak butuh waktu). Berapa menit waktu minimal untuk menyelesaikan wawancara dari seluruh peserta sejak meja wawancara dibuka?

## Batasan
- \( 1 <= K <= N <= 100\ 000 \)
- \( 1 <= D <= 10\ 000 \)
- \( 0 <= T_i <= 10^9 \)

## Subsoal
1. `(14 poin)` \( N = 5, K = 1, D = 2, T = \{1,2,6,7,8\} \)
2. `(14 poin)` \( N = 10, K = 2, D = 3, T = \{1,3,3,5,5,5,5,7,7,7,7\} \)
3. `(20 poin)` \( N,D,T_i <= 100 \)
4. `(10 poin)` \( N = 1 \)
5. `(10 poin)` \( K = N \)
6. `(10 poin)` \( K = 1 \)
7. `(12 poin)` Tidak ada batasan tambahan.

## Masukan
Masukan diberikan dalam format berikut:
```
N K D
T_1 ... T_N
```

## Keluaran
Sebuah baris berisi bilangan yang menyatakan waktu minimal dalam satuan menit.

## Contoh Masukan
```
3 2 4
5 7 6
```

## Contoh Keluaran
```
13
```

## Penjelasan Contoh
Kronologi wawancara yang optimal adalah sebagai berikut.
1. Dari menit \(0\) sampai \(5\), belum ada peserta datang.
2. Dari menit \(5\) sampai \(6\), Fae mewawancarai peserta \(1\).
3. Dari menit \(6\) sampai \(9\), Fae mewawancarai peserta \(1\) dan \(3\).
4. Dari menit \(9\) sampai \(10\), Fae mewawancarai peserta \(2\) dan \(3\).
5. Dari menit \(10\) sampai \(13\), Fae mewawancarai peserta \(2\).
6. Seluruh wawancara selesai pada menit \(13\).

## Usulan Solusi
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

long long minimalInterviewTime(int N, int K, int D, vector<int>& T) {
    // Sort participants by arrival times
    sort(T.begin(), T.end());

    long long timeNeeded = 0;
    int i = 0;

    while (i < N) {
        // Start a new session
        long long sessionStart = max(timeNeeded, (long long)T[i]);
        int participants = 0;

        // Process up to K participants in the current session
        while (i < N && T[i] <= sessionStart + D && participants < K) {
            participants++;
            i++;
        }

        // Update the total time needed
        timeNeeded = sessionStart + D;
    }

    return timeNeeded;
}

int main() {
    int N, K, D;
    cin >> N >> K >> D;

    vector<int> T(N);
    for (int i = 0; i < N; ++i) {
        cin >> T[i];
    }

    cout << minimalInterviewTime(N, K, D, T) << endl;

    return 0;
}
```