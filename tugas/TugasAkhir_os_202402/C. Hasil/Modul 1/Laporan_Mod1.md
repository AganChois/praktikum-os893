# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Agan Chois`
**NIM**: `240202893`
**Modul yang Dikerjakan**:
`Modul 1 – System Call dan Instrumentasi Kernel`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 1 – System Call dan Instrumentasi Kernel**:
  Menambahkan dua system call baru, yaitu getpinfo() untuk melihat proses yang aktif, dan getreadcount() untuk menghitung jumlah pemanggilan read() sejak sistem booting. Informasi proses yang diambil mencakup PID, ukuran memori, dan nama proses, yang memungkinkan pengguna memantau aktivitas sistem secara langsung melalui antarmuka syscall buatan sendiri.
---

## 🛠️ Rincian Implementasi


### Contoh untuk Modul 1:
* Menambahkan dua system call getpinfo() dan getreadcount() di sysproc.c dan syscall.c, yang berfungsi sebagai handler utama untuk pemanggilan dari user space.
* Mengedit user.h, usys.S, dan syscall.h untuk mendaftarkan syscall, karena ketiga file ini menyediakan jembatan antara kode pengguna dan fungsi kernel.
* Menambahkan struktur struct pinfo di proc.h, yang dirancang untuk menyimpan data proses seperti PID, memori, dan nama proses saat dikirim ke user space.
* Menambahkan counter readcount di sysfile.c, tepatnya dalam fungsi sys_read(), agar setiap pemanggilan read() otomatis menambah nilai counter tersebut.
* Sinkronisasi akses ke ptable dilakukan dengan acquire() dan release(), untuk mencegah race condition saat data proses dibaca secara bersamaan.
* Membuat dua program uji yaitu ptest.c dan rtest.c, yang digunakan untuk menguji validitas fungsi kedua system call yang ditambahkan.
* Menambahkan kedua program ke UPROGS dalam Makefile, agar otomatis ter-compile dan tersedia di direktori bin sebagai bagian dari sistem.

---

## ✅ Uji Fungsionalitas
* Program ptest digunakan untuk menguji getpinfo(), dengan menampilkan semua proses aktif beserta PID, ukuran memori, dan nama proses, sehingga dapat diverifikasi bahwa data yang diambil dari kernel sesuai dan lengkap.
* Sementara itu, program rtest digunakan untuk menguji getReadCount(), dengan membandingkan nilai read count sebelum dan sesudah pembacaan input, untuk memastikan counter berjalan sesuai fungsi.

---

## 📷 Hasil Uji

### 📍 Contoh Output `ptest`:

```
== Info Proses Aktif ==
PID     MEM     NAME
1       4096    init
2       2048    sh
3       2048    ptest
```

### 📍 Contoh Output `rtest`:

```
Read Count Sebelum: 4
hello
Read Count Setelah: 5
```

### 📍 Screenshot :

![alt text](ptest_rtest_hello-output-1.jpg)

```

## ⚠️ Kendala yang Dihadapi

* Menangani pointer dari user space menggunakan argptr() cukup rumit karena kesalahan sedikit saja dapat menyebabkan kernel crash atau data gagal dibaca.
* Sinkronisasi akses ke ptable menjadi penting karena tanpa mekanisme locking, kondisi balapan dapat terjadi saat beberapa proses mengakses tabel proses secara bersamaan.
* Kesalahan umum seperti salah akses pointer (. vs ->) sering terjadi saat mengakses field dalam struktur melalui pointer.
* Lupa meng-include spinlock.h menyebabkan fungsi locking seperti acquire() tidak dikenali saat kompilasi.
* Gagal membaca hasil syscall juga sering disebabkan oleh kesalahan dalam mendefinisikan argumen antara user space dan kernel space, sehingga data tidak tertransmisikan dengan benar.

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Stack Overflow, GitHub Issues, diskusi praktikum

---

