---
title: RAT IN THE MAZE PROBLEM
date: 2025-05-27
categories: [DESAIN ANALISIS ALGORITMA, DYNAMIC PROGRAMMING, BACKTRACKING ALGORITHM]
tags: [daa, algorithm, rat-in-the-maze, dynamic-programming]
---

# Presentasi Kelompok 6: Rat in the Maze Problem

Masalah *Rat in the Maze* merupakan salah satu contoh klasik dalam algoritma pencarian jalur, di mana seekor tikus harus mencari jalan dari titik awal ke tujuan di dalam sebuah labirin yang diwakili sebagai matriks dua dimensi. Labirin ini terdiri dari jalur yang bisa dilewati (`1`) dan rintangan (`0`). Masalah ini banyak diaplikasikan pada bidang seperti pencarian rute otomatis dan navigasi robot.

![Rat in the Maze](assets/pertemuan/klp6/Screenshot 2025-06-09 221153.jpg){: width="500"}

---

## Konsep Dasar

Tujuan utama dari masalah ini adalah menemukan satu atau lebih jalur dari posisi awal ke akhir dalam labirin, dengan menghindari sel yang terblokir.

### Strategi Penyelesaian

1. **Penjelajahan Jalur Valid**  
   Telusuri semua jalur memungkinkan dari titik awal menuju akhir.

2. **Backtracking**  
   Jika menemui jalan buntu, kembali ke posisi sebelumnya dan coba rute lain.

3. **Optimalisasi Pencarian**  
   Gunakan *backtracking* untuk menghindari eksplorasi jalur yang jelas buntu.

---

## Tahapan Implementasi

### 1. Representasi Labirin  
Gunakan matriks `n x n` berisi `1` (jalan) dan `0` (rintangan).

### 2. Logika Rekursif dengan Backtracking  
Cek langkah-langkah legal (atas, bawah, kiri, kanan) dan lanjutkan jika masih valid.

### 3. Perekaman Jalur  
Simpan jalur dalam bentuk string (misalnya: `D` untuk bawah, `R` untuk kanan) saat menjelajah.

---

## Implementasi C++: Rat in the Maze

```cpp
#include <iostream>
#include <vector>
#include <string>

void solve(int i, int j, std::vector<std::vector<int>> &m, int n,
           std::vector<std::string> &ans, std::string move,
           std::vector<std::vector<int>> &vis) {

    if (i == n - 1 && j == n - 1) {
        ans.push_back(move);
        return;
    }

    // Gerak ke bawah
    if (i + 1 < n && !vis[i + 1][j] && m[i + 1][j] == 1) {
        vis[i][j] = 1;
        solve(i + 1, j, m, n, ans, move + 'D', vis);
        vis[i][j] = 0;
    }

    // Gerak ke kiri
    if (j - 1 >= 0 && !vis[i][j - 1] && m[i][j - 1] == 1) {
        vis[i][j] = 1;
        solve(i, j - 1, m, n, ans, move + 'L', vis);
        vis[i][j] = 0;
    }

    // Gerak ke kanan
    if (j + 1 < n && !vis[i][j + 1] && m[i][j + 1] == 1) {
        vis[i][j] = 1;
        solve(i, j + 1, m, n, ans, move + 'R', vis);
        vis[i][j] = 0;
    }

    // Gerak ke atas
    if (i - 1 >= 0 && !vis[i - 1][j] && m[i - 1][j] == 1) {
        vis[i][j] = 1;
        solve(i - 1, j, m, n, ans, move + 'U', vis);
        vis[i][j] = 0;
    }
}
