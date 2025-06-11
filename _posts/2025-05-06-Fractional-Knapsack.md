---
title: FRACTIONAL KNAPSACK
date: 2025-05-06
categories: [DESAIN ANALISIS ALGORITMA, GREEDY ALGORITHM]
tags: [daa, algorithm, greedy]
---

# Presentasi Kelompok 2: Fractional Knapsack Problem

Halo semuanya! Setelah mempelajari **Activity Selection Problem**, kita akan lanjut membahas salah satu persoalan penting lain dalam algoritma greedy, yaitu **Fractional Knapsack**.

![Desktop View](assets/pertemuan/klp2/Screenshot 2025-06-09 220833.jpg){: width="500"}

---

## Apa Itu Fractional Knapsack?

**Fractional Knapsack** merupakan masalah optimasi di mana kita diberi sejumlah barang yang masing-masing memiliki nilai dan bobot tertentu. Tujuan kita adalah memasukkan barang-barang tersebut ke dalam tas (knapsack) yang memiliki kapasitas terbatas, agar total nilai yang dibawa maksimal.

Tidak seperti versi **0/1 Knapsack** yang hanya memperbolehkan kita mengambil seluruh barang atau tidak sama sekali, pada **Fractional Knapsack**, kita diperbolehkan mengambil sebagian dari barang.

---

## Strategi Penyelesaian

### 1. Hitung Nilai per Bobot
Langkah awal adalah menghitung rasio nilai terhadap bobot untuk setiap barang (value/weight). Semakin tinggi rasio ini, semakin prioritas barang tersebut untuk dimasukkan.

### 2. Urutkan Barang
Setelah mendapatkan rasio nilai per bobot, urutkan semua barang berdasarkan rasio tersebut dari yang tertinggi ke terendah.

### 3. Masukkan Barang ke Dalam Knapsack
Mulai dari barang dengan rasio tertinggi, masukkan ke dalam knapsack sampai kapasitasnya habis. Jika barang terakhir tidak muat seluruhnya, kita ambil sebagian saja sesuai sisa kapasitas.

---

## Langkah-langkah Algoritma

1. Hitung `value/weight` untuk setiap barang.
2. Urutkan barang berdasarkan rasio tersebut secara menurun.
3. Masukkan barang ke dalam knapsack satu per satu.
4. Jika barang tidak muat sepenuhnya, ambil bagiannya saja.
5. Hitung total nilai dari semua barang yang masuk.

---

## Implementasi dalam Bahasa C++

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Struktur data untuk menyimpan informasi barang
struct Item {
    int weight;
    int value;
    double valuePerWeight;

    Item(int w, int v) {
        weight = w;
        value = v;
        valuePerWeight = (double)v / w;
    }
};

// Urutkan barang berdasarkan nilai per bobot secara menurun
bool compare(Item a, Item b) {
    return a.valuePerWeight > b.valuePerWeight;
}

double fractionalKnapsack(int capacity, vector<Item>& items) {
    sort(items.begin(), items.end(), compare);

    double totalValue = 0.0;

    for (const Item& item : items) {
        if (capacity == 0) break;

        if (item.weight <= capacity) {
            totalValue += item.value;
            capacity -= item.weight;
        } else {
            totalValue += item.valuePerWeight * capacity;
            break;
        }
    }

    return totalValue;
}

int main() {
    int n, capacity;
    cout << "Masukkan jumlah barang: ";
    cin >> n;
    cout << "Masukkan kapasitas knapsack: ";
    cin >> capacity;

    vector<Item> items;

    for (int i = 0; i < n; i++) {
        int weight, value;
        cout << "Masukkan bobot dan nilai barang ke-" << i + 1 << ": ";
        cin >> weight >> value;
        items.push_back(Item(weight, value));
    }

    double maxValue = fractionalKnapsack(capacity, items);
    cout << "Nilai maksimal yang dapat dimasukkan ke dalam knapsack adalah: " << maxValue << endl;

    return 0;
}
