# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi  
**Semester**: Genap / Tahun Ajaran 2024–2025  
**Nama**: `<abbi priyoguno>`  
**NIM**: `<240202848>`  
**Modul yang Dikerjakan**: Modul 5 – Audit dan Keamanan Sistem (xv6-public)  

---

## 📌 Deskripsi Singkat Tugas

Modul ini menambahkan fitur audit sistem panggilan di dalam xv6 dengan rincian:

- Mencatat setiap system call yang dijalankan ke dalam struktur log kernel.
- Menyediakan syscall baru `get_audit_log()` yang dapat digunakan untuk membaca log tersebut.
- Hanya proses dengan PID 1 (biasanya `init`) yang diizinkan mengakses log tersebut, sebagai langkah pengamanan.

---

## 🛠️ Rincian Implementasi

- Menambahkan struktur data `audit_log` dan `audit_entry` di `syscall.c`.
- Mengubah fungsi `syscall()` agar setiap pemanggilan system call dicatat ke dalam log.
- Menambahkan syscall `get_audit_log()` di `sysproc.c`, `syscall.h`, `user.h`, dan `usys.S`.
- Mencegah akses log oleh proses selain PID 1.
- Membuat program uji `audit.c` untuk membaca dan menampilkan log.

---

## 🗂️ File yang Diubah / Ditambahkan

| File        | Perubahan                              |
| ----------- | -------------------------------------- |
| `syscall.c` | Tambah pencatatan log setiap syscall   |
| `sysproc.c` | Implementasi syscall `get_audit_log()` |
| `defs.h`    | Deklarasi fungsi kernel tambahan       |
| `user.h`    | Deklarasi syscall user-level           |
| `usys.S`    | Registrasi syscall                     |
| `syscall.h` | Nomor syscall baru                     |
| `Makefile`  | Tambahkan program uji `audit.c`        |

---

## ✅ Uji Fungsionalitas

Program uji: `audit.c`  
Fungsi: Menampilkan isi audit log jika dijalankan oleh proses PID 1.

Cara uji:

1. Jalankan `make clean && make qemu-nox`
2. Ubah `init.c` agar menjalankan `audit` sebagai program pertama (`exec("audit", argv)`)
3. Lihat hasil log audit system call yang tercatat

---

## 📷 Hasil Uji

### 📍 Output jika bukan PID 1:

Access denied or error.


### 📍 Output jika sebagai PID 1:

=== Audit Log ===
[0] PID=1 SYSCALL=5 TICK=12
[1] PID=1 SYSCALL=6 TICK=13
...
###  *screenshots*

---<img width="967" height="638" alt="Screenshot 2025-07-26 223317" src="https://github.com/user-attachments/assets/b5ae42b7-a2ca-4b08-add5-7b4d40eee1ed" />


## ⚠️ Kendala yang Dihadapi

- Audit log tidak muncul saat dijalankan sebagai proses biasa karena validasi PID belum ditambahkan.
- Ukuran `audit_log` terbatas (128 entri); perlu perhatian untuk implementasi riwayat jangka panjang.
- Salah saat memetakan `argptr()` menyebabkan error segfault; perlu dicek ukuran buffer secara tepat.

---

## 🔐 Ringkasan Fitur Keamanan

| Fitur                 | Deskripsi                                            |
| ---------------------| ---------------------------------------------------- |
| Log system call       | Semua syscall dicatat: PID, nomor syscall, dan tick |
| Akses terbatas        | Hanya proses dengan PID 1 yang dapat membaca log    |
| Pemisahan kernel-user | Data hanya dapat diakses via syscall resmi          |

---

## 📚 Referensi

- Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
- Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
- Diskusi praktikum dan debugging manual syscall di xv6
