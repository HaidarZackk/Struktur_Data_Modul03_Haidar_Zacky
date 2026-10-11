# <h1 align="center"> Modul 3 ABSTRACT DATA TYPE (ADT) (BAGIAN KETIGA) </h1>
<p align="center">Muhammad Haidar Az Zacky - 109082530035</p>

## Dasar Teori

### A. Abstract Data Type (ADT)

ADT (Abstract Data Type) adalah sebuah tipe data yang dibungkus bersama sekumpulan operasi dasar (primitif) yang boleh dilakukan terhadap tipe tersebut. ADT yang lengkap juga menyertakan invarian (aturan yang harus selalu benar) dan aksioma. Sifatnya statik, dan sebuah ADT boleh disusun dari ADT lain, misalnya ADT Waktu dari ADT Jam dan ADT Date, atau ADT Garis dari dua ADT Point.

Primitif pada ADT dikelompokkan menjadi: konstruktor (pembentuk nilai, biasanya berawalan *Make*), selector (pengakses komponen, biasanya berawalan *Get*), prosedur pengubah komponen, validator, destruktor, baca/tulis, operator relasional, aritmatika, dan konversi tipe.

### B. Implementasi ADT

ADT diwujudkan dalam **dua modul** dan **satu driver**:

| Modul | Isi |
|---|---|
| **Header (`.h`)** | Spesifikasi type dan prototipe primitif (fungsi: nama, domain, range, prekondisi; prosedur: initial state, final state, proses) |
| **Body (`.cpp`)** | Realisasi dari primitif yang sudah dispesifikasikan di header |
| **Driver (`main.cpp`)** | Program utama yang memakai ADT |

Pemisahan ini membuat deklarasi dan implementasi tidak tercampur, sehingga kode lebih rapi dan mudah dipakai ulang.

## Guided

### 1. Program Mahasiswa.h

```C++
#ifndef MAHASISWA_H_INCLUDED
#define MAHASISWA_H_INCLUDED

struct Mahasiswa{
    char nim[10];
    int nilai1, nilai2;
};

void inputMhs(Mahasiswa &m);
float rata2(Mahasiswa m);

#endif

```
**Penjelasan:** Ini adalah *header* yang berperan sebagai "kontrak" ADT Mahasiswa.
- `#ifndef / #define / #endif` (header guard) mencegah file ini terbaca dua kali saat di-include berulang.
- `struct Mahasiswa` menyimpan `nim` dan dua nilai (`nilai1`, `nilai2`).
- `inputMhs` menerima parameter by reference (`&`) supaya isian langsung tersimpan di variabel asli, sedangkan `rata2` cukup by value karena hanya membaca data lalu mengembalikan hasil `float`.

### 2. Program Mahasiswa.cpp

```C++
#include <iostream>
#include "mahasiswa.h"

using namespace std;

void inputMhs(Mahasiswa &m){
    cout << "Masukkan NIM: ";
    cin >> m.nim;
    cout << "Masukkan Nilai 1: ";
    cin >> m.nilai1;
    cout << "Masukkan Nilai 2: ";
    cin >> m.nilai2;
}

float rata2(Mahasiswa m){
    return float(m.nilai1 + m.nilai2) / 2.0;
}
```
**Penjelasan:** File ini berisi *isi* dari fungsi yang sebelumnya hanya dideklarasikan di header.
- `inputMhs` meminta tiga masukan (NIM dan dua nilai) lalu menyimpannya ke `m`.
- `rata2` menjumlahkan kedua nilai, lalu membaginya 2.0 agar hasilnya desimal, bukan terpotong menjadi bilangan bulat.

### 3. Program Main.cpp

```C++
#include "mahasiswa.h"
#include <iostream>

using namespace std;

int main() {
    Mahasiswa mhs;
    inputMhs(mhs);
    cout << "Rata - Rata = " << rata2 (mhs);
    return 0;
}

```
**Penjelasan:** Ini adalah *driver*. Ia membuat satu variabel `Mahasiswa`, mengisinya lewat `inputMhs`, lalu mencetak hasil `rata2`. Karena logika sudah ada di modul lain, `main` tinggal memanggil. Kompilasi harus menyertakan kedua file `.cpp`, misalnya `g++ main.cpp mahasiswa.cpp -o program`.

## Unguided

### 1. Program data mahasiswa (maks. 10) dengan nilai akhir dari fungsi

```C++
#include <iostream>
#include <string>
using namespace std;

struct Mahasiswa {
    string nama, nim;
    float uts, uas, tugas, nilaiAkhir;
};

float hitungNilaiAkhir(float uts, float uas, float tugas) {
    return 0.3f * uts + 0.4f * uas + 0.3f * tugas;
}

int main() {
    const int MAKS = 10;
    Mahasiswa data[MAKS];
    int n;

    do {
        cout << "Jumlah mahasiswa (1-" << MAKS << "): ";
        cin >> n;
    } while (n < 1 || n > MAKS);
    cin.ignore();

    for (int i = 0; i < n; i++) {
        cout << "\nData mahasiswa ke-" << i + 1 << endl;
        cout << "Nama  : "; getline(cin, data[i].nama);
        cout << "NIM   : "; getline(cin, data[i].nim);
        cout << "UTS   : "; cin >> data[i].uts;
        cout << "UAS   : "; cin >> data[i].uas;
        cout << "Tugas : "; cin >> data[i].tugas;
        cin.ignore();

        data[i].nilaiAkhir = hitungNilaiAkhir(data[i].uts, data[i].uas, data[i].tugas);
    }

    cout << "\n=== DAFTAR MAHASISWA ===\n";
    for (int i = 0; i < n; i++) {
        cout << "\n[" << i + 1 << "] " << data[i].nama << " (" << data[i].nim << ")\n";
        cout << "    UTS " << data[i].uts << " | UAS " << data[i].uas
             << " | Tugas " << data[i].tugas << endl;
        cout << "    Nilai Akhir : " << data[i].nilaiAkhir << endl;
    }
    return 0;
}
```
### Output Unguided 1 :

##### Output 1
![Output 1](laprak3-1.jpeg)

**Penjelasan:**
- Data disimpan dalam *array of struct* berukuran 10, sehingga satu elemen array mewakili satu mahasiswa lengkap dengan semua field-nya.
- Nilai akhir tidak dihitung langsung di `main`, melainkan lewat fungsi `hitungNilaiAkhir` sesuai tuntutan soal. Fungsi ini menerima tiga nilai dan mengembalikan satu hasil, sehingga mudah diuji dan dipakai ulang.
- Perulangan `do-while` menjaga jumlah input tetap di rentang 1-10 agar array tidak melebihi batas, dan `cin.ignore()` membersihkan sisa *newline* supaya `getline` tidak terlewat.

### 2. ADT Pelajaran

```C++
// pelajaran.h
#ifndef PELAJARAN_H_INCLUDED
#define PELAJARAN_H_INCLUDED

#include <iostream>
#include <string>
using namespace std;

struct pelajaran {
    string namaMapel;
    string kodeMapel;
};

pelajaran create_pelajaran(string namapel, string kodepel);

void tampil_pelajaran(pelajaran pel);

#endif
```
```C++
// pelajaran.cpp
#include "pelajaran.h"

pelajaran create_pelajaran(string namapel, string kodepel) {
    pelajaran pel;
    pel.namaMapel = namapel;
    pel.kodeMapel = kodepel;
    return pel;
}

void tampil_pelajaran(pelajaran pel) {
    cout << "nama pelajaran : " << pel.namaMapel << endl;
    cout << "nilai : " << pel.kodeMapel << endl;
}
```
```C++
// main.cpp
#include "pelajaran.h"

using namespace std;

int main() {
    string namapel = "Struktur Data";
    string kodepel = "STD";

    pelajaran pel = create_pelajaran(namapel, kodepel);
    tampil_pelajaran(pel);

    return 0;
}
```
### Output Unguided 2 :

##### Output 2
![Output 2](laprak3-2.jpeg)

**Penjelasan:**
- Latihan ini menerapkan pola ADT secara utuh: **type** (`struct pelajaran`) dan **primitif**-nya dideklarasikan di `.h`, direalisasikan di `.cpp`, lalu dipakai oleh `main.cpp` sebagai driver.
- `create_pelajaran` berperan sebagai **konstruktor**, yaitu satu-satunya jalan membentuk nilai `pelajaran`, sedangkan `tampil_pelajaran` adalah primitif **tulis** ke layar.
- Karena implementasi terpisah, kompilasi harus menyertakan semua file `.cpp`: `g++ main.cpp pelajaran.cpp -o pelajaran`.

### 3. Array 2D, Pointer, dan Prosedur Penukaran

```C++
#include <iostream>
using namespace std;

void tampilArray(int arr[3][3]) {
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << arr[i][j] << "\t";
        }
        cout << endl;
    }
}

void tukarArray(int a[3][3], int b[3][3], int baris, int kolom) {
    int temp = a[baris][kolom];
    a[baris][kolom] = b[baris][kolom];
    b[baris][kolom] = temp;
}

void tukarPointer(int *p, int *q) {
    int temp = *p;
    *p = *q;
    *q = temp;
}

int main() {
    int A[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
    int B[3][3] = {{10, 20, 30}, {40, 50, 60}, {70, 80, 90}};

    int x = 100, y = 200;
    int *p1 = &x;
    int *p2 = &y;

    cout << "--- Sebelum tukar elemen [1][1] ---\n";
    cout << "Array A:\n"; tampilArray(A);
    cout << "Array B:\n"; tampilArray(B);

    tukarArray(A, B, 1, 1);

    cout << "\n--- Sesudah tukar elemen [1][1] ---\n";
    cout << "Array A:\n"; tampilArray(A);
    cout << "Array B:\n"; tampilArray(B);

    cout << "\n--- Pointer sebelum tukar ---\n";
    cout << "*p1 = " << *p1 << ", *p2 = " << *p2 << endl;

    tukarPointer(p1, p2);

    cout << "--- Pointer sesudah tukar ---\n";
    cout << "*p1 = " << *p1 << ", *p2 = " << *p2 << endl;

    return 0;
}
```
### Output Unguided 3 :

##### Output 3
![Output 3](laprak3-3.jpeg)

**Penjelasan:**
- `tampilArray` memakai dua perulangan bersarang: perulangan luar untuk baris, perulangan dalam untuk kolom.
- `tukarArray` menukar satu elemen di posisi yang sama pada dua array. Array di C++ otomatis dikirim sebagai alamat, sehingga perubahan di dalam fungsi langsung berlaku pada array aslinya.
- `tukarPointer` menukar *isi* variabel, bukan alamatnya, dengan operator dereference (`*`). Setelah dipanggil, `x` menjadi 200 dan `y` menjadi 100, terlihat dari `*p1` dan `*p2`.

## Kesimpulan

Modul ini memperkenalkan Abstract Data Type (ADT), yaitu cara membungkus sebuah tipe data beserta operasi dasarnya menjadi satu kesatuan. Di C++, ADT diwujudkan dengan memecah program menjadi tiga bagian: header (`.h`) untuk spesifikasi type dan primitif, file implementasi (`.cpp`) untuk realisasinya, serta driver (`main.cpp`) sebagai pengguna ADT. Dengan pemisahan ini, program menjadi lebih terstruktur, mudah dirawat, dan bisa dipakai ulang.

Pada bagian Guided, konsep tersebut dipraktikkan melalui ADT Mahasiswa. File `mahasiswa.h` menyimpan struct dan prototipe fungsi, `mahasiswa.cpp` berisi `inputMhs` (pass by reference agar data tersimpan ke variabel asli) dan `rata2` (pass by value karena hanya membaca), sedangkan `main.cpp` hanya memanggil keduanya. Dari sini terlihat jelas bagaimana pembagian tugas antar file bekerja.

Pada bagian Unguided, tiga latihan menguatkan pemahaman tersebut. Latihan pertama memakai array of struct untuk menyimpan hingga 10 mahasiswa dengan nilai akhir dihitung oleh fungsi berbobot 0.3 UTS, 0.4 UAS, dan 0.3 Tugas. Latihan kedua membangun ADT Pelajaran lengkap dengan konstruktor `create_pelajaran` dan prosedur `tampil_pelajaran`. Latihan ketiga melatih array 2D dan pointer lewat prosedur menampilkan array, menukar elemen antar-array, serta menukar isi dua variabel melalui pointer.

Secara keseluruhan, modul ini menunjukkan bahwa array, pointer, struct, dan fungsi yang dipelajari sebelumnya menjadi bahan dasar untuk membangun ADT. Pemahaman ini penting sebagai bekal sebelum mempelajari struktur data yang lebih kompleks dan pemrograman berorientasi objek.

## Referensi

[1] Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2022). *Introduction to Algorithms* (4th ed.). Cambridge, MA: MIT Press.
<br>[2] Stroustrup, B. (2022). *A Tour of C++* (3rd ed.). Boston: Addison-Wesley.
<br>[3] Farrier, J. (2024). *Data Structures and Algorithms with the C++ STL*. Birmingham: Packt Publishing.
