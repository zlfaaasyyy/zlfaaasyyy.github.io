---
title: ACTIVITY SELECTION PROBLEM
date: 2025-05-06
categories: [DESAIN ANALISIS ALGORITMA, GREEDY ALGORITHM]
tags: [daa, algorithm, greedy]
---

# Presentasi Kelompok 1: Activity Selection Problem

Halo semuanya! Kali ini kita akan membahas salah satu topik penting dalam dunia algoritma dan optimasi, yaitu **Activity Selection Problem**, yang dibawakan oleh Kelompok 1.


---

## Apa Itu Activity Selection Problem?

Dalam banyak situasi di dunia nyata—seperti pemesanan ruang rapat, penjadwalan CPU, atau manajemen proyek—kita sering dihadapkan pada beberapa aktivitas yang saling berebut satu sumber daya yang terbatas. Masing-masing aktivitas memiliki waktu mulai dan waktu selesai.

Tantangannya adalah memilih sebanyak mungkin aktivitas yang **tidak saling bertabrakan (overlap)** agar penggunaan sumber daya bisa **maksimal**. Inilah inti dari **Activity Selection Problem**.

Masalah ini merupakan contoh klasik dari penerapan **algoritma greedy**, di mana kita mengambil keputusan terbaik di setiap langkah dengan harapan hasil akhirnya juga optimal.

---

## 1. Pendahuluan

**Activity Selection Problem** sering dijadikan contoh dalam pembelajaran strategi algoritmik, khususnya untuk memperkenalkan metode **greedy**.

Tujuannya sederhana: dari sekumpulan aktivitas dengan waktu mulai dan selesai tertentu, pilih sebanyak mungkin aktivitas yang tidak tumpang tindih.

Masalah ini banyak ditemukan dalam aplikasi penjadwalan (scheduling), seperti:

- Pemakaian ruang rapat
- Penjadwalan kelas
- Pembagian waktu CPU dalam sistem komputer

---

## 2. Rumusan Masalah

Diberikan sejumlah aktivitas, masing-masing dengan waktu mulai dan selesai. Tugas kita adalah memilih sebanyak mungkin aktivitas tanpa ada dua aktivitas yang saling bertabrakan dalam waktu.

### Contoh Dataset:

| Aktivitas | Mulai | Selesai |
|----------:|------:|--------:|
| 1         | 1     | 4       |
| 2         | 3     | 5       |
| 3         | 0     | 6       |
| 4         | 5     | 7       |
| 5         | 3     | 8       |
| 6         | 5     | 9       |
| 7         | 6     | 10      |
| 8         | 8     | 11      |

Dari data di atas, kita ingin memilih sebanyak mungkin aktivitas yang tidak saling bertumpuk waktunya.

---

## 3. Strategi Greedy: Pendekatan yang Efisien

Pendekatan **greedy** dalam masalah ini adalah dengan selalu memilih aktivitas yang memiliki **waktu selesai paling awal**.

Kenapa? Karena semakin cepat suatu aktivitas selesai, semakin cepat kita bisa beralih ke aktivitas lain yang tidak konflik waktunya.

### Langkah-langkah Algoritma:

1. **Urutkan aktivitas berdasarkan waktu selesai.**
2. **Pilih aktivitas pertama** dalam daftar (pasti memiliki waktu selesai paling cepat).
3. **Iterasi aktivitas berikutnya**, hanya pilih jika waktu mulai ≥ waktu selesai dari aktivitas yang terakhir dipilih.
4. **Ulangi** hingga semua aktivitas sudah dipertimbangkan.

---

## 4. Contoh Implementasi dalam C++

Berikut adalah kode C++ yang menerapkan pendekatan greedy untuk menyelesaikan Activity Selection Problem:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Struktur untuk menyimpan informasi aktivitas
struct Activity {
    int start;   // Waktu mulai
    int finish;  // Waktu selesai
};

// Fungsi pembanding untuk mengurutkan aktivitas berdasarkan waktu selesai
bool compare(Activity a, Activity b) {
    return a.finish < b.finish;
}

// Fungsi utama untuk memilih aktivitas yang tidak tumpang tindih
vector<Activity> activitySelection(vector<Activity>& activities) {
    // Urutkan aktivitas berdasarkan waktu selesai
    sort(activities.begin(), activities.end(), compare);

    vector<Activity> selectedActivities;
    selectedActivities.push_back(activities[0]); // Pilih aktivitas pertama

    int lastFinishTime = activities[0].finish;

    // Periksa aktivitas berikutnya
    for (int i = 1; i < activities.size(); i++) {
        if (activities[i].start >= lastFinishTime) {
            selectedActivities.push_back(activities[i]);
            lastFinishTime = activities[i].finish;
        }
    }

    return selectedActivities;
}

int main() {
    vector<Activity> activities = {
        {1, 4}, {3, 5}, {0, 6}, {5, 7},
        {3, 8}, {5, 9}, {6, 10}, {8, 11}
    };

    vector<Activity> selected = activitySelection(activities);

    cout << "Aktivitas yang dipilih:" << endl;
    for (const auto& activity : selected) {
        cout << "Start: " << activity.start
             << ", Finish: " << activity.finish << endl;
    }

    return 0;
}
