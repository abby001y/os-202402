# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: `<abbi priyoguno>`  
**NIM**: `<240202848>`  
**Modul yang Dikerjakan**: Modul 4 – Subsistem Kernel Alternatif (xv6-public)  

---

## 📌 Deskripsi Singkat Tugas

Modul ini terdiri dari dua bagian utama:

1. Implementasi syscall `chmod(path, mode)` untuk mengatur mode file (`read-only` atau `read-write`) secara sederhana.
2. Penambahan driver pseudo-device `/dev/random` untuk menghasilkan byte acak secara deterministik di kernel.

---

## 🛠️ Rincian Implementasi

### A. System Call `chmod()`

- Menambahkan field `mode` pada `struct inode` di `fs.h`.
- Implementasi syscall `chmod()` di `sysfile.c`.
- Menambahkan entri syscall baru pada `syscall.c`, `syscall.h`, `user.h`, dan `usys.S`.
- Modifikasi fungsi `filewrite()` pada `file.c` untuk mencegah penulisan ke file yang diatur sebagai read-only.
- Membuat program uji `chmodtest.c`.

### B. Device Driver `/dev/random`

- Menambahkan file `random.c` berisi fungsi `randomread()` sebagai generator acak sederhana.
- Registrasi fungsi tersebut pada array `devsw[]` di `file.c`.
- Membuat node device `/dev/random` di `init.c` menggunakan `mknod()`.
- Menyediakan program uji `randomtest.c`.

---

## 📂 File yang Diubah / Ditambahkan

| File                  | Tujuan                         |
| --------------------- | ------------------------------ |
| `fs.h`                | Tambah field `mode` di inode   |
| `fs.c`, `file.c`      | Cek mode akses sebelum tulis   |
| `sysfile.c`           | Implementasi syscall `chmod()` |
| `syscall.c`           | Registrasi syscall             |
| `syscall.h`           | Nomor syscall baru             |
| `user.h`              | Deklarasi syscall              |
| `usys.S`              | Entry syscall                  |
| `Makefile`            | Daftar program user uji        |
| `random.c` (baru)     | Device handler `/dev/random`   |
| `devsw[]` di `file.c` | Registrasi driver device       |

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

- `chmodtest`: menguji bahwa file tidak bisa ditulis setelah diset sebagai read-only.
- `randomtest`: menguji output byte acak dari `/dev/random`.

---

## 📷 Hasil Uji

### 📍 Output `chmodtest`:

Write blocked as expected

### 📍 Output `randomtest`:

Output berbeda-beda, contoh:

241 6 82 99 12 201 44 73


---
### *screenshots*

<img width="969" height="642" alt="Screenshot 2025-07-26 220521" src="https://github.com/user-attachments/assets/9f36c1b3-9273-4891-a5e1-07e6dc900cc4" />




## ⚠️ Kendala yang Dihadapi

- Kesalahan awal saat membaca argumen syscall di `sys_chmod()` karena lupa memanggil `argstr()`.
- Nilai `mode` tidak tervalidasi, menyebabkan perilaku tidak diharapkan saat menerima angka selain `0` atau `1`.
- Lupa menambahkan `randomtest` ke `Makefile`, sehingga tidak ter-compile oleh `make`.

---

## 📘 Penutup

Dengan menyelesaikan modul ini, saya telah:

- ✅ Mengintegrasikan fitur perizinan file dasar (`chmod`) ke dalam sistem file xv6.
- ✅ Menambahkan driver perangkat `/dev/random` sebagai contoh device I/O.
- ✅ Memperluas pemahaman mengenai sistem syscall, driver, dan inode.

---

## 📚 Referensi

- Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
- xv6-public repository: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
- Diskusi praktikum dan dokumentasi kernel Linux

---
