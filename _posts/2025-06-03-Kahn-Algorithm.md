---
title: KHAN'S ALGORITHM
date: 2025-05-27
categories: [DESAIN ANALISIS ALGORITMA, KHAN'S ALGORITHM, TOPOLOGICAL SORT]
tags: [daa, algorithm, khan]
---

# PRESENTASI KELOMPOK 9: KHAN'S ALGORITHM

Khan's Algorithm adalah metode yang digunakan untuk melakukan **topological sorting** pada graf berarah yang tidak mengandung siklus (DAG – Directed Acyclic Graph). Algoritma ini menjamin bahwa setiap simpul (node) hanya diproses setelah semua ketergantungan (prasyarat) terhadap simpul lain telah diselesaikan.

Topological sort sangat bermanfaat dalam berbagai kasus, seperti penjadwalan aktivitas, urutan tugas berdasarkan dependensi, serta sistem build software.

## Konsep Dasar Khan's Algorithm

Langkah-langkah utama algoritma ini adalah:

1. **Hitung In-degree (Derajat Masuk)**  
   Hitung jumlah edge yang masuk ke tiap node. Node yang memiliki in-degree 0 bisa langsung diproses karena tidak memiliki ketergantungan.

2. **Masukkan Node In-degree 0 ke Queue**  
   Node yang tidak memiliki ketergantungan dimasukkan ke dalam antrian sebagai titik awal pemrosesan.

3. **Proses Node dan Perbarui In-degree**  
   Ambil node dari antrian, masukkan ke dalam hasil topological sort, kemudian kurangi in-degree dari semua tetangganya. Bila in-degree tetangga menjadi 0, masukkan ke antrian.

4. **Lanjutkan Hingga Semua Node Diproses**  
   Ulangi proses hingga antrian kosong. Jika masih ada node yang tidak terproses dan memiliki in-degree > 0, berarti graf memiliki siklus dan tidak dapat diurutkan secara topologis.

### Struktur Data yang Dibutuhkan

- **Queue (Antrian)**: Menyimpan node-node yang siap diproses (in-degree = 0).
- **Array In-degree**: Menyimpan jumlah ketergantungan (edge masuk) tiap node.

## Langkah-Langkah Implementasi

1. Hitung derajat masuk setiap node.
2. Masukkan semua node dengan in-degree 0 ke dalam antrian.
3. Proses node dari antrian dan kurangi in-degree tetangganya.
4. Masukkan tetangga yang in-degree-nya menjadi 0 ke antrian.
5. Ulangi sampai seluruh node diproses atau antrian kosong.

---

## Contoh Implementasi C++

```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

void khanAlgorithm(int n, const vector<vector<int>>& graph) {
    vector<int> in_degree(n, 0);

    // Hitung in-degree setiap node
    for (int i = 0; i < n; i++) {
        for (int neighbor : graph[i]) {
            in_degree[neighbor]++;
        }
    }

    queue<int> q;
    for (int i = 0; i < n; i++) {
        if (in_degree[i] == 0) {
            q.push(i);
        }
    }

    vector<int> topologicalOrder;

    while (!q.empty()) {
        int node = q.front();
        q.pop();
        topologicalOrder.push_back(node);

        for (int neighbor : graph[node]) {
            in_degree[neighbor]--;
            if (in_degree[neighbor] == 0) {
                q.push(neighbor);
            }
        }
    }

    if (topologicalOrder.size() != n) {
        cout << "Graf mengandung siklus!" << endl;
    } else {
        cout << "Hasil Topological Sort: ";
        for (int node : topologicalOrder) {
            cout << node << " ";
        }
        cout << endl;
    }
}

int main() {
    int n = 6;
    vector<vector<int>> graph = {
        {2, 3},
        {3},
        {4},
        {5},
        {5},
        {}
    };

    khanAlgorithm(n, graph);

    return 0;
}
