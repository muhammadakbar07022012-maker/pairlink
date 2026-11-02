# PairLink

PairLink adalah aplikasi web pairing dua HP berbasis Node.js, Socket.IO, dan WebRTC.

## Fitur
- Kode pairing acak 6 digit.
- Kode berlaku 5 menit dan cooldown pembuatan kode 5 menit per ID pengontrol.
- HP target harus menyetujui permintaan pairing.
- Berbagi layar dimulai secara manual dari HP target.
- Pengontrol dapat mengirim tulisan dan meminta senter menyala/mati.
- Tidak ada akses file, pesan, kamera diam-diam, atau kendali sentuh jarak jauh.

## Jalankan lokal
1. Pasang Node.js 18 atau lebih baru di komputer.
2. Jalankan `npm install`.
3. Jalankan `npm start`.
4. Buka `http://localhost:3000/controller.html` dan `http://localhost:3000/target.html`.

Untuk uji dua HP pada jaringan lokal, gunakan alamat LAN komputer. Berbagi layar dan kamera memerlukan secure context (HTTPS; localhost merupakan pengecualian). Karena itu, gunakan hosting HTTPS untuk pemakaian online.

## Deploy online dari HP Android
1. Ekstrak ZIP PairLink.
2. Buat repositori GitHub baru, misalnya `pairlink`.
3. Unggah seluruh isi folder ini ke repositori (bukan folder ZIP-nya).
4. Di layanan hosting Node.js seperti Render, buat Web Service dari repositori tersebut.
5. Build command: `npm install`
6. Start command: `npm start`
7. Tunggu deploy selesai, lalu buka URL HTTPS yang diberikan hosting.
8. HP pengontrol membuka `/controller.html`; HP target membuka `/target.html`.

Jangan gunakan `file://` atau alamat HTTP biasa untuk berbagi layar/kamera. Browser Android mensyaratkan HTTPS untuk API media.

## Catatan kompatibilitas
- `getDisplayMedia()` pada Android tidak didukung merata oleh semua browser/perangkat. Jika tombol berbagi layar memberi pesan tidak didukung, gunakan Chrome versi terbaru; bila tetap tidak tersedia, fitur screen share membutuhkan aplikasi Android native.
- Kontrol torch memakai kemampuan `torch` pada track kamera. Tidak semua browser/perangkat mengekspos kemampuan ini. HP target harus menekan "Aktifkan izin kamera" terlebih dahulu.
- STUN publik membantu koneksi WebRTC. Jaringan tertentu memerlukan TURN server agar video dapat tersambung; TURN berbayar/gratis pihak ketiga perlu dikonfigurasi tersendiri.
- Data pairing disimpan di memori server. Restart server menghapus kode dan sesi aktif.
- Kode hanya satu kali pakai. Cooldown server disimpan di memori dan akan reset jika server restart.
- Gunakan satu sesi per pengontrol pada satu waktu.

## Keamanan produksi
Untuk penggunaan publik, tambahkan HTTPS dari hosting, pembatasan request, autentikasi pemilik, TURN dengan kredensial sementara, dan penyimpanan cooldown terdistribusi. Jangan memasukkan secret TURN ke kode frontend.
