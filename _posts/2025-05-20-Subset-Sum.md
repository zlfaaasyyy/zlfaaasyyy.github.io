---
title: SUBSET SUM PROBLEM
date: 2025-05-20
categories: [DESAIN ANALISIS ALGORITMA, DYNAMIC PROGRAMMING, BACKTRACKING ALGORITHM]
tags: [daa, algorithm, subset-sum, dynamic-programming]
---

# Presentasi Kelompok 6: Subset Sum Problem

Masalah Subset Sum adalah salah satu masalah klasik dalam teori algoritma dan pemrograman yang berfokus pada pencarian subset dari sebuah himpunan angka yang jumlahnya sama dengan nilai target tertentu. Masalah ini digunakan dalam banyak aplikasi seperti kriptografi, pengelolaan sumber daya, dan perencanaan.


---

# Penjelasan tentang Subset Sum Problem

**Subset Sum Problem** adalah masalah pencarian subset dari sebuah himpunan yang jumlah elemennya sama dengan nilai target yang telah ditentukan. Misalnya, kita memiliki himpunan angka `{3, 34, 4, 12, 5, 2}` dan kita ingin mengetahui apakah ada subset dari himpunan tersebut yang jumlahnya sama dengan angka 9. Dalam hal ini, jawabannya adalah *ya*, karena subset `{4, 5}` memiliki jumlah 9.

## Prinsip Dasar Subset Sum Problem

1. **Pencarian Subset**  
   Mencoba berbagai subset dari himpunan angka untuk memeriksa apakah jumlahnya sama dengan nilai target.

2. **Pemeriksaan Jumlah**  
   Memeriksa apakah jumlah elemen dalam subset sama dengan target.

3. **Pencarian Solusi yang Efisien**  
   Menggunakan algoritma seperti *dynamic programming* agar lebih efisien daripada brute-force.

---

## Langkah-Langkah Implementasi Subset Sum Problem

### 1. Menyiapkan Himpunan Angka dan Target
Tentukan himpunan angka dan nilai target yang ingin dicapai.

### 2. Pemrograman Dinamis
Gunakan tabel `dp` untuk menyimpan status sub-masalah yang telah diselesaikan sebelumnya.

### 3. Pencarian Subset
Gunakan nilai-nilai dari tabel untuk mengetahui apakah target bisa dibentuk dari subset himpunan.

---

## Implementasi Subset Sum Problem dalam C++

```cpp
#include <iostream>
#include <vector>

using namespace std;

// Fungsi untuk memeriksa apakah ada subset dengan jumlah yang sama dengan target
bool isSubsetSum(const vector<int>& set, int n, int target) {
    vector<vector<bool>> dp(n + 1, vector<bool>(target + 1, false));

    for (int i = 0; i <= n; i++) {
        dp[i][0] = true;
    }

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= target; j++) {
            if (set[i - 1] <= j) {
                dp[i][j] = dp[i - 1][j] || dp[i - 1][j - set[i - 1]];
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }

    return dp[n][target];
}

int main() {
    vector<int> set = {3, 34, 4, 12, 5, 2};
    int target = 9;
    int n = set.size();

    if (isSubsetSum(set, n, target)) {
        cout << "Ada subset yang jumlahnya sama dengan " << target << endl;
    } else {
        cout << "Tidak ada subset yang jumlahnya sama dengan " << target << endl;
    }

    return 0;
}
