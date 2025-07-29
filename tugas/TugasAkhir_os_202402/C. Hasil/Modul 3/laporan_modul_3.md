# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `<ABBI PRIYOGUNO>`
**NIM**: `<240202848>`
**Modul yang Dikerjakan**:
`Modul 3 – Manajemen Memori Tingkat Lanjut (Copy-on-Write dan Shared Memory)
`

---

## 📌 Deskripsi Singkat Tugas

Modul ini berfokus pada peningkatan efisiensi manajemen memori di sistem operasi xv6 dengan dua fitur utama:

Copy-on-Write (CoW):
Optimalisasi fork() agar tidak menyalin seluruh memori proses anak, melainkan hanya menyalin saat terjadi penulisan (write) melalui page fault handler.

Shared Memory ala System V:
Menambahkan system call shmget() dan shmrelease() untuk memungkinkan dua proses atau lebih berbagi satu halaman memori secara langsung, menggunakan pendekatan reference count.

## 🛠️ Rincian Implementasi

Copy-on-Write (CoW)
Menambahkan array ref_count[] di vm.c untuk menghitung referensi per halaman fisik.
Menambahkan fungsi incref() dan decref() untuk manajemen refcount dan kfree().
Menambahkan flag PTE_COW di mmu.h.
Membuat fungsi baru cowuvm() menggantikan copyuvm() di vm.c.
Mengubah fork() di proc.c untuk menggunakan cowuvm() agar halaman tidak langsung disalin.
Menangani page fault T_PGFLT di trap.c, dengan memeriksa flag PTE_COW dan melakukan salinan memori jika diperlukan.
---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

cowtest.c: Menguji apakah perubahan pada anak tidak memengaruhi memori induk melalui mekanisme CoW.
shmtest.c: Menguji berbagi memori antar proses melalui shmget() dan sinkronisasi konten memori.


* `audit`: untuk melihat isi log system call (jika dijalankan oleh PID 1)

---

## 📷 Hasil Uji

Lampirkan hasil uji berupa screenshot atau output terminal. Contoh:

### 📍 Contoh Output `cowtest`:

```
Child sees: Y
Parent sees: X

```

### 📍 Contoh Output `shmtest`:

```
Child reads: A
Parent reads: B

```

### 📍 Contoh Output `chmodtest`:

```
Write blocked as expected
```

Jika ada screenshot:

```
![hasil cowtest](.Screenshot 2025-07-26 212116.png)
```

---

## ⚠️ Kendala yang Dihadapi

Awalnya page fault tidak tertangani dengan benar karena lupa memeriksa flag PTE_COW.
Beberapa panic terjadi karena lupa memanggil incref() pada kalloc().
Salah mapping shared memory di luar batas USERTOP menyebabkan segmentation fault.
Sistem belum membatasi jumlah maksimum shared memory per proses, berpotensi menimbulkan konflik alokasi.

---

## 📚 Referensi

Tuliskan sumber referensi yang Anda gunakan, misalnya:

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Stack Overflow, GitHub Issues, diskusi praktikum

---

