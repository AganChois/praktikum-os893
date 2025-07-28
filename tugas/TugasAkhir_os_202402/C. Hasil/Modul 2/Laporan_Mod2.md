# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Agan Chois`
**NIM**: `240202893`
**Modul yang Dikerjakan**:
`Modul 2 – Penjadwalan CPU Non-Preemptive Berbasis Prioritas`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 2 – Penjadwalan CPU Non-Preemptive Berbasis Prioritas:**:
  Tugas ini berfokus pada penggantian scheduler default Round-Robin pada sistem operasi xv6 menjadi scheduler berbasis prioritas non-preemptive, di mana proses dengan prioritas lebih rendah (nilai angka prioritas lebih kecil) akan dieksekusi lebih dulu. Selain itu, ditambahkan juga system call set_priority() yang memungkinkan pengguna mengatur prioritas proses dari user space.

## 🛠️ Rincian Implementasi


### Untuk Modul 2:
* Menambahkan field int priority pada struct proc di proc.h:Ini dilakukan agar setiap proses memiliki nilai prioritas yang bisa diatur dan digunakan dalam proses penjadwalan.
* Menyesuaikan allocproc() dan scheduler() di proc.c:Fungsi allocproc() diubah untuk memberi nilai default pada field priority, sementara fungsi scheduler() diubah agar memilih proses berdasarkan nilai prioritas terendah.
* Menambahkan syscall set_priority(int):System call ini memungkinkan proses mengatur nilai prioritasnya sendiri melalui pemanggilan dari user space.
* Tambahan dilakukan pada syscall.h, user.h, usys.S, syscall.c, dan sysproc.c:Perubahan ini diperlukan agar syscall baru dikenali oleh sistem dan dapat dipanggil dari program pengguna.
* Program uji ptest.c dimodifikasi untuk menguji perilaku scheduler:Program ini dibuat untuk menjalankan beberapa child process dengan prioritas berbeda, sehingga bisa diuji apakah proses dengan prioritas lebih tinggi dijalankan lebih dulu.

---

## ✅ Uji Fungsionalitas
* ptest modul2: menguji penjadwalan berdasarkan prioritas
Uji coba ini menggunakan dua child process dengan nilai prioritas yang berbeda.
  * Child 2 diberi prioritas tinggi (nilai lebih kecil, misalnya 10):
Ini bertujuan agar child 2 dijadwalkan lebih dulu oleh scheduler.
  * Child 1 diberi prioritas rendah (nilai lebih besar, misalnya 90):
Dengan prioritas lebih rendah, child 1 seharusnya dijalankan setelah child 2.
* Diharapkan output: Child 2 selesai muncul sebelum Child 1 selesai:
Jika scheduler berjalan sesuai harapan, output ini menjadi indikator keberhasilan sistem baru.


---

## 📷 Hasil Uji
### 📍 Output `ptest`:

```
Child 2 selesai
Child 1 selesai
Parent selesai

```

### 📍 Screenshot :

![hasil ptest](./Screenshot/ptest_modul2-output.jpg)

```

## ⚠️ Kendala yang Dihadapi
---
* Pada fungsi scheduler(), struktur proc dan cpu tidak dikenali, sehingga perlu ditambahkan extern struct cpu *cpu; serta pemanggilan mycpu() untuk mendapatkan informasi CPU yang sedang aktif.
* Terdapat masalah di mana proses dengan prioritas rendah tetap berjalan lebih dahulu. Untuk mengatasi ini, ditambahkan sleep(1) setelah proses fork, agar proses dengan prioritas tinggi sempat masuk ke scheduler sebelum proses dengan prioritas lebih rendah dieksekusi. Hal ini penting untuk memastikan urutan eksekusi sesuai dengan skema prioritas yang ditentukan.


---

## 📚 Referensi
---
* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Diskusi praktikum,Stack Overflow, GitHub Issues.

---

