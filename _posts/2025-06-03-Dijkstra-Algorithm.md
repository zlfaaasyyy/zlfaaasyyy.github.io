---
title: DIJKSTRA'S ALGORITHM
date: 2025-06-11
categories: [DESAIN ANALISIS ALGORITMA, SHORT PATH ALGORITHMS]
tags: [daa, algorithm, dijkstra]
---

# PRESENTASI KELOMPOK 7: DIJKSTRA'S ALGORITHM

Dijkstra's Algorithm merupakan metode yang digunakan untuk menentukan jarak terpendek dari satu titik awal (source node) ke seluruh titik lain dalam graf berbobot tanpa sisi bernilai negatif. Algoritma ini banyak dimanfaatkan dalam sistem navigasi, perencanaan rute, dan pengolahan jaringan.

Prinsip kerja algoritma ini bersifat **greedy**, yaitu memilih solusi lokal terbaik pada setiap langkah untuk mencapai hasil akhir yang optimal.


---

## Konsep Dasar Dijkstra's Algorithm

Langkah-langkah utama dalam algoritma ini:

1. **Inisialisasi**  
   Tentukan node sumber dan atur jaraknya menjadi 0, sementara node lainnya diset sebagai ∞ (tak hingga). Semua node dianggap belum dikunjungi.

2. **Pilih Node dengan Jarak Minimum**  
   Pilih node yang memiliki jarak terkecil dari sumber dan belum dikunjungi.

3. **Perbarui Jarak Tetangga**  
   Untuk setiap node tetangga dari node yang sedang diproses, hitung jarak baru melalui node tersebut. Jika lebih pendek, perbarui jarak.

4. **Tandai Node Telah Dikunjungi**  
   Setelah memproses node dan semua tetangganya, tandai node tersebut agar tidak diproses lagi.

5. **Ulangi Proses**  
   Proses diulang sampai seluruh node dikunjungi atau tidak ada lagi jarak yang bisa diperbarui.

---

## Struktur Data yang Digunakan

- **Min-Heap (Priority Queue)**: Memilih node dengan jarak terkecil secara efisien.
- **Visited Set**: Menyimpan informasi tentang node yang sudah diproses.
- **Distance Array**: Menyimpan jarak minimum dari node sumber ke tiap node lainnya.

---

## Langkah-Langkah Implementasi

1. Tentukan nilai jarak awal semua node, di mana jarak node sumber adalah 0.
2. Gunakan priority queue untuk memilih node dengan jarak terpendek.
3. Lakukan proses pembaruan jarak ke node-node tetangga.
4. Ulangi hingga seluruh node telah diproses atau tidak ada alternatif jalur lebih pendek.

---

## Implementasi Dijkstra's Algorithm dalam C++

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
using namespace std;

struct Edge {
    int destination, weight;
};

void dijkstra(int start, int n, const vector<vector<Edge>>& graph) {
    vector<int> distance(n, INT_MAX);
    distance[start] = 0;

    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    pq.push({0, start});

    while (!pq.empty()) {
        int dist = pq.top().first;
        int node = pq.top().second;
        pq.pop();

        if (dist > distance[node]) continue;

        for (const Edge& edge : graph[node]) {
            int nextNode = edge.destination;
            int weight = edge.weight;

            if (distance[node] + weight < distance[nextNode]) {
                distance[nextNode] = distance[node] + weight;
                pq.push({distance[nextNode], nextNode});
            }
        }
    }

    cout << "Jarak terpendek dari node " << start << " ke semua node lainnya:" << endl;
    for (int i = 0; i < n; i++) {
        if (distance[i] == INT_MAX) {
            cout << "Node " << i << ": Tidak dapat dijangkau" << endl;
        } else {
            cout << "Node " << i << ": " << distance[i] << endl;
        }
    }
}

int main() {
    int n = 6;
    vector<vector<Edge>> graph(n);

    graph[0].push_back({1, 4});
    graph[0].push_back({2, 1});
    graph[1].push_back({3, 1});
    graph[2].push_back({1, 2});
    graph[2].push_back({3, 5});
    graph[3].push_back({4, 3});
    graph[4].push_back({5, 1});
    graph[5].push_back({0, 3});

    int startNode = 0;
    dijkstra(startNode, n, graph);

    return 0;
}
