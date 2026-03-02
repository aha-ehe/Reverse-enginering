# Laporan Analisis APK ML+

## 1. Analisis `AndroidManifest.xml`
Dari file manifest aplikasi (`com.hagaseca.skeli`), kami menemukan penggunaan beberapa *permissions* (izin) tingkat tinggi yang perlu diwaspadai:
- `android.permission.INTERNET`: Untuk akses internet (standar).
- `android.permission.SYSTEM_ALERT_WINDOW`: Digunakan untuk menampilkan *overlay* atau *floating window* di atas aplikasi lain. Ini sering digunakan oleh aplikasi *mod menu* atau *cheat* untuk menampilkan menu di dalam game.
- `android.permission.QUERY_ALL_PACKAGES`: Mengizinkan aplikasi untuk melihat daftar semua aplikasi yang terinstal di perangkat.
- `android.permission.WRITE_SECURE_SETTINGS` dan `android.permission.WRITE_SETTINGS`: Mengizinkan modifikasi pengaturan sistem perangkat.
- `android.permission.FOREGROUND_SERVICE` dan `android.permission.FOREGROUND_SERVICE_SPECIAL_USE`: Digunakan untuk menjalankan *service* di latar belakang yang terus aktif (dalam kasus ini: `FloatingService` dan `PairingService`).

## 2. Analisis Fungsionalitas Aplikasi
Berdasarkan pembacaan kode sumber dekompilasi (khususnya di `MainActivity`, `ModsActivity`, dan `FloatingService`), berikut adalah cara kerja aplikasi ini:
- **Mod Menu / Cheat Overlay**: Aplikasi menggunakan `FloatingService` untuk menggambar *overlay UI* di atas layar menggunakan izin `SYSTEM_ALERT_WINDOW`. Layar *overlay* tersebut memiliki pengaturan seperti mengatur ukuran "hero", "map size", dan posisi X/Y dari suatu map.
- **Fitur Maphack**: Di dalam `FloatingService.java` dan `ModsActivity.java` secara eksplisit ditemukan referensi kata "maphack". Terdapat tombol (`bt_maphack`) dan sebuah switch (`sw_maphack`) di antarmuka penggunanya, dan status maphack ditampilkan pada text view (`tv_maphack_status`). Ini merupakan indikasi kuat bahwa APK ini berfungsi sebagai aplikasi modifikasi/curang untuk sebuah game MOBA (kemungkinan besar Mobile Legends, terlihat dari singkatan "ML+").
- **Fungsi Injeksi**: Terdapat tombol "Inject" (`bt_inject`) di `ModsActivity`, yang digunakan untuk menyuntikkan (inject) modifikasi langsung ke aplikasi game target.
- **Penanganan Obfuscation**: String pada kode telah di-obfuscate menggunakan sebuah class `Oh.r(Zj...)`. Cara ini sering digunakan pembuat aplikasi mod untuk menghindari deteksi string statis.

## 3. Kesimpulan Keamanan
Aplikasi ini **TIDAK** dapat diklasifikasikan sebagai *malware* atau *spyware* pencuri data tradisional berdasarkan analisis statis awal (tidak ada aktivitas mencurigakan berupa eksfiltrasi data pribadi, file, atau SMS ke server luar yang ditemukan dengan jelas).

Namun, aplikasi ini adalah sebuah **Game Mod / Cheat Tool (Maphack)**. Aplikasi semacam ini termasuk dalam kategori *Risky Software* (Perangkat Lunak Berisiko):
1. Penggunaannya melanggar *Term of Service* (ToS) dari game yang bersangkutan dan dapat menyebabkan akun game Anda diblokir (*banned*).
2. Meminta izin yang sangat kuat seperti `WRITE_SECURE_SETTINGS`.

**Tujuan Aplikasi:** Aplikasi dirancang untuk memberikan keuntungan tidak adil dalam permainan (Maphack) dengan menggambar overlay *floating window* di atas game target.