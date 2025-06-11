---
title: HUFFMAN CODING
date: 2025-05-20
categories: [DESAIN ANALISIS ALGORITMA, GREEDY ALGORITHM]
tags: [daa, algorithm, huffman]
---

# Presentasi Kelompok 3: Huffman Coding

HI!. Di era digital saat ini, di mana kita terus menerus berinteraksi dengan sejumlah besar informasi mulai dari teks, gambar, audio, hingga video—kebutuhan akan penyimpanan dan transmisi data yang efisien menjadi sangat krusial. Salah satu cara untuk mencapai efisiensi ini adalah melalui kompresi data. Kompresi data memungkinkan kita untuk mengurangi ukuran data asli tanpa kehilangan informasi penting, atau dengan kehilangan yang dapat diterima (tergantung jenis kompresinya).

---

## Penjelasan tentang Huffman Coding

**Huffman Coding** adalah algoritma kompresi data tanpa kehilangan (*lossless compression*) yang digunakan untuk mengurangi ukuran data dengan cara menggantikan simbol-simbol yang lebih sering muncul dengan kode yang lebih pendek, dan simbol-simbol yang lebih jarang muncul dengan kode yang lebih panjang. Algoritma ini ditemukan oleh David A. Huffman pada tahun 1952.

### Prinsip Dasar Huffman Coding

1. **Hitung Frekuensi Simbol**  
   Hitung frekuensi kemunculan setiap simbol dalam data yang ingin dikompresi.

2. **Buat Pohon Huffman**  
   Bangun pohon Huffman dari simbol dan frekuensinya menggunakan struktur *min-heap*.

3. **Assign Kode pada Simbol**  
   Turunkan kode Huffman dari akar ke setiap daun, dengan kiri sebagai `0` dan kanan sebagai `1`.

---

## Proses Pengkodean dan Penguraian

- **Pengkodean**: Setiap karakter diganti dengan bit string berdasarkan pohon Huffman.
- **Penguraian (Decoding)**: Kode bit dibaca dan dilacak kembali ke pohon untuk membentuk karakter asli.

---

## Langkah-Langkah Implementasi Huffman Coding

1. Membaca input dan menghitung frekuensi karakter.
2. Membangun pohon Huffman dari frekuensi.
3. Menghasilkan kode Huffman untuk setiap simbol.
4. Mengompresi data dan menampilkan hasil kompresi.

---

## Implementasi Huffman Coding dalam C++

```cpp
#include <iostream>
#include <queue>
#include <unordered_map>
#include <vector>
#include <string>

using namespace std;

// Struktur data untuk merepresentasikan node Huffman
struct Node {
    char data;
    int freq;
    Node *left, *right;

    Node(char data, int freq) {
        this->data = data;
        this->freq = freq;
        left = right = nullptr;
    }
};

// Membandingkan dua node berdasarkan frekuensi (min-heap)
struct compare {
    bool operator()(Node* left, Node* right) {
        return left->freq > right->freq;
    }
};

// Membangun pohon Huffman
Node* buildHuffmanTree(const unordered_map<char, int>& freq_map) {
    priority_queue<Node*, vector<Node*>, compare> minHeap;
    for (auto pair : freq_map) {
        minHeap.push(new Node(pair.first, pair.second));
    }

    while (minHeap.size() > 1) {
        Node* left = minHeap.top(); minHeap.pop();
        Node* right = minHeap.top(); minHeap.pop();
        Node* newNode = new Node('$', left->freq + right->freq);
        newNode->left = left;
        newNode->right = right;
        minHeap.push(newNode);
    }

    return minHeap.top();
}

// Menghasilkan kode Huffman untuk setiap karakter
void generateHuffmanCodes(Node* root, string str, unordered_map<char, string>& huffmanCodes) {
    if (!root) return;
    if (root->data != '$') huffmanCodes[root->data] = str;
    generateHuffmanCodes(root->left, str + "0", huffmanCodes);
    generateHuffmanCodes(root->right, str + "1", huffmanCodes);
}

// Fungsi utama untuk kompresi Huffman
void huffmanCompress(const string& text) {
    unordered_map<char, int> freq_map;
    for (char ch : text) freq_map[ch]++;

    Node* root = buildHuffmanTree(freq_map);

    unordered_map<char, string> huffmanCodes;
    generateHuffmanCodes(root, "", huffmanCodes);

    cout << "Kode Huffman untuk setiap karakter:\n";
    for (auto pair : huffmanCodes) {
        cout << pair.first << ": " << pair.second << endl;
    }

    string compressedText = "";
    for (char ch : text) compressedText += huffmanCodes[ch];

    cout << "\nTeks setelah dikompresi: " << compressedText << endl;
}

int main() {
    string text;
    cout << "Masukkan teks untuk dikompresi: ";
    getline(cin, text);
    huffmanCompress(text);
    return 0;
}
