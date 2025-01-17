# Deskripsi
Sayed dan Parvez sedang bermain suatu permainan bilangan yang mereka sebut dengan **Permainan ABC**. Dalam permainan ini, Parvez akan memilih suatu bilangan \(A\). Selanjutnya, Sayed akan memilih suatu bilangan positif \(B\) dengan \( (B<A) \). Setelah itu, permainan akan disimulasikan dengan langkah-langkah berikut.

1. Definisikan variabel \(C\) yang bernilai \( A-B \).
2. Ubah nilai \( A \) menjadi \( B \).
3. Ubah nilai \( B \) menjadi \( C \).
4. Apabila nilai \( B \) masih positif, maka kembali ke langkah 1. Jika tidak, permainan selesai.

Sayed akan menang apabila nilai \( B=0 \) di akhir permainan. Sebaliknya, jika \( B < 0 \) maka Parvez yang akan menang. Bantulah Sayed untuk mencari nilai \( B \) yang dapat membuat dia menang atau beri tahu apabila tidak ada nilai \( B \) yang memenuhi! Apabila terdapat banyak kemungkinan, keluarkan yang mana saja.

## Batasan
* \( 2 <= A <= 10^9 \)

## Subsoal
1. `(14 poin)` \(A = 12 \)
2. `(14 poin)` \(A = 25 \)
3. `(20 poin)` \(A <= 1000 \)
4. `(10 poin)` \(A <= 10^6 \)
5. `(20 poin)` \(A\) habis dibagi \(9\).
6. `(22 poin)` Tidak ada batasan tambahan.

## Masukan
Masukan diberikan dalam format berikut:

```
A
```

## Keluaran
Sebuah baris berisi sebuah bilangan yang menyatakan nilai \(B\) atau `-1` apabila tidak ada nilai B yang memenuhi.

## Contoh Masukan
```
52
```

## Contoh Keluaran
```
32
```

## Penjelasan Contoh
Pada awal permainan, Parvez memilih bilangan \(A=52\). Selanjutnya, Sayed memilih bilangan \(B=32\). Langkah-langkah yang disimulasikan adalah sebagai berikut.

### Langkah ke-1
* \( C \leftarrow A-B = 52 - 32 = 20 \)
* \( A \leftarrow B = 32 \)
* \( B \leftarrow C = 20 \)
### Langkah ke-2
* \( C \leftarrow A-B = 32 - 20 = 20 \)
* \( A \leftarrow B = 20 \)
* \( B \leftarrow C = 12 \)
### Langkah ke-3
* \( C \leftarrow A-B = 20 - 12 = 8 \)
* \( A \leftarrow B = 12 \)
* \( B \leftarrow C = 8 \)
### Langkah ke-4
* \( C \leftarrow A-B = 12 - 8 = 4 \)
* \( A \leftarrow B = 8 \)
* \( B \leftarrow C = 4 \)
### Langkah ke-5
* \( C \leftarrow A-B = 8 - 4 = 4 \)
* \( A \leftarrow B = 4 \)
* \( B \leftarrow C = 4 \)
### Langkah ke-6
* \( C \leftarrow A-B = 4 - 4 = 0 \)
* \( A \leftarrow B = 4 \)
* \( B \leftarrow C = 0 \)
* Selesai karena \(B\) sudah tidak positif.

Pada akhir permainan, nilai \(B=0\) sehingga Sayed menang.

## Proposed Answer
```cpp
#include <iostream>
#include <vector>
using namespace std;

bool simulateGame(long long A, long long B) {
    while (B > 0) {
        long long C = A - B;
        A = B;
        B = C;
    }
    return B == 0; // Sayed menang jika B == 0
}

long long findWinningB(long long A) {
    for (long long B = 1; B < A; ++B) {
        if (simulateGame(A, B)) {
            return B;
        }
    }
    return -1; // Tidak ada nilai B yang membuat Sayed menang
}

int main() {
    long long A;
    cin >> A;

    long long result = findWinningB(A);
    cout << result << endl;

    return 0;
}
```