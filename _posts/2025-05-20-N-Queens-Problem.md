---
title: N-QUEENS PROBLEM
date: 2025-05-20
categories: [DESAIN ANALISIS ALGORITMA, BACKTRACKING ALGORITHM]
tags: [daa, algorithm, backtracking, n-queen]     
---

# Presentasi Kelompok 4: N-Queen Problem

Masalah **N-Queen** adalah salah satu persoalan klasik dalam teori algoritma dan pemrograman, yang melibatkan penempatan *N* buah ratu pada papan catur berukuran *N × N*. Tujuannya adalah menempatkan ratu-ratu tersebut sedemikian rupa sehingga tidak ada dua ratu yang saling menyerang satu sama lain — baik secara horizontal, vertikal, maupun diagonal.

---

## Apa itu N-Queen Problem?

**N-Queen Problem** termasuk dalam kategori *optimisasi kombinatorial* dan diselesaikan dengan teknik *backtracking*. Tugasnya adalah menempatkan N buah ratu pada papan catur N × N, dengan syarat bahwa tidak ada dua ratu yang menyerang satu sama lain.

---

## Pentingnya N-Queen Problem

1. **Studi Kasus Algoritma dan AI**
   - Digunakan sebagai contoh dalam teknik pencarian seperti *backtracking*, *branch and bound*, dan algoritma *heuristic*.
2. **Model Constraint Satisfaction Problem (CSP)**
   - Mewakili masalah penugasan dengan batasan (constraint).
3. **Aplikasi Dunia Nyata**
   - Penjadwalan tugas tanpa konflik
   - Penempatan modul elektronik
   - Pengalokasian sumber daya eksklusif
4. **Skalabilitas dan Kompleksitas**
   - Kompleksitas bertumbuh eksponensial seiring kenaikan N → ideal untuk uji efisiensi algoritma.

---

## Kenapa Disebut Backtracking?

1. **Pendekatan Rekursif Bertahap**
   - Tempatkan ratu baris demi baris, dan jika posisi tidak valid, kembali ke posisi sebelumnya.
2. **Pohon Solusi**
   - Setiap penempatan membentuk simpul pohon, dan jalur ke solusi adalah traversal dari akar ke daun.
3. **Non-deterministik**
   - Solusi tidak diketahui sejak awal, jadi semua kemungkinan diuji satu per satu.

---

## Konsep Backtracking

**Backtracking** adalah metode pencarian solusi dengan membangun solusi secara bertahap dan membatalkan langkah ketika menemui jalan buntu.

### Tahapan Umum:

1. **Pilih Kandidat Solusi**
2. **Periksa Validitas**
3. **Rekursif**
4. **Backtrack jika Gagal**
5. **Cek Basis Kasus (Solusi Lengkap)**

---

## Prinsip Dasar N-Queen

1. **Penempatan Ratu**
   - Dimulai dari baris pertama.
2. **Validasi Posisi**
   - Tidak boleh ada dua ratu saling serang.
3. **Backtracking**
   - Jika gagal, mundur ke langkah sebelumnya dan coba posisi lain.

---

## Proses Penyelesaian

- **Coba Posisi Valid**: Coba tiap kolom dan periksa validitas.
- **Backtrack**: Jika tidak valid, mundur dan ganti posisi.

---

## Implementasi N-Queen dalam C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

// Cek apakah posisi aman untuk menempatkan ratu
bool isSafe(const vector<vector<int>>& board, int row, int col, int N) {
    for (int i = 0; i < row; i++) {
        if (board[i][col] == 1) return false;
    }

    for (int i = row, j = col; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 1) return false;
    }

    for (int i = row, j = col; i >= 0 && j < N; i--, j++) {
        if (board[i][j] == 1) return false;
    }

    return true;
}

// Backtracking utama
bool solveNQueen(vector<vector<int>>& board, int row, int N) {
    if (row >= N) return true;

    for (int col = 0; col < N; col++) {
        if (isSafe(board, row, col, N)) {
            board[row][col] = 1;

            if (solveNQueen(board, row + 1, N)) return true;

            board[row][col] = 0; // Backtrack
        }
    }
    return false;
}

// Cetak papan solusi
void printBoard(const vector<vector<int>>& board, int N) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            cout << (board[i][j] ? "Q " : ". ");
        }
        cout << endl;
    }
}

int main() {
    int N;
    cout << "Masukkan ukuran papan catur (N): ";
    cin >> N;

    vector<vector<int>> board(N, vector<int>(N, 0));

    if (solveNQueen(board, 0, N)) {
        cout << "Solusi ditemukan:\n";
        printBoard(board, N);
    } else {
        cout << "Tidak ada solusi untuk N = " << N << endl;
    }

    return 0;
}
