# Officer's Voice — Firebase Edition

Paket ini mengganti Supabase dengan Firebase. Hosting tetap GitHub Pages. Tidak memakai Google Apps Script.

## Isi paket
- `index.html` — formulir publik Officer's Voice.
- `ia.html` — portal login dan daftar laporan untuk petugas IA.
- `firestore.rules` — aturan keamanan database.
- `README.md` — panduan setup.

## Setup Firebase (sekali saja)
1. Buka https://console.firebase.google.com/ lalu buat project baru, misalnya `officers-voice-lspd`.
2. Di halaman project, klik ikon Web `</>` untuk menambahkan Web App. Beri nama `Officer's Voice`, lalu Register app. Tidak perlu mengaktifkan Firebase Hosting.
3. Firebase akan menampilkan `firebaseConfig`. Salin nilai `apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, dan `appId`.
4. Konfigurasi Firebase project `iadcurhat` sudah diisikan ke `index.html` dan `ia.html`. Tidak perlu mengedit `firebaseConfig` lagi. Jangan menaruh password atau private key di file website.
5. Tambahkan email petugas IA ke allowlist `isIA()` di `firestore.rules` dan daftar `IA_EMAILS` di `ia.html`; keduanya harus cocok. Setelah itu, di Firebase Console → Build → Firestore Database → Create database. Pilih production mode dan lokasi yang sesuai. Buka tab Rules, ganti seluruh isi dengan file rules yang sudah diedit, lalu Publish. Rules memberikan izin hapus hanya kepada akun IA yang ada di allowlist.
6. Di Firebase Console → Build → Authentication → Get started → Sign-in method, aktifkan **Email/Password**. Jangan aktifkan pendaftaran publik di website.
7. Di Authentication → Users → Add user, buat akun email/password untuk petugas IA yang dipercaya. Buat satu akun bersama hanya jika memang diperlukan; akun per petugas lebih mudah diaudit. Jangan bagikan akun ke officer umum.
8. Upload `index.html` dan `ia.html` ke root repository GitHub yang sama. Aktifkan GitHub Pages dari Settings → Pages → Deploy from a branch → `main` → `/ (root)`.
9. Formulir publik ada di URL GitHub Pages utama. Petugas IA membuka `.../ia.html`, login memakai akun allowlist, lalu tekan **Tampilkan laporan** untuk melihat jawaban. Di portal, laporan tampil dalam tabel; detail bisa dibuka per baris, laporan dapat difilter berdasarkan tanggal pilihan atau dikelompokkan per tanggal, dan tombol **Hapus** menghapus dokumen dari Firestore setelah konfirmasi.

## Perilaku & keamanan
- Formulir publik hanya boleh membuat laporan baru.
- Formulir publik tidak dapat membaca, mengubah, atau menghapus laporan. Penghapusan hanya dapat dilakukan akun IA yang tercantum di allowlist.
- Laporan hanya dapat dibaca oleh akun yang sudah login DAN emailnya tercantum dalam allowlist di `firestore.rules`. Akun lain, walaupun berhasil login, tidak dapat membaca laporan.
- Mode Anonymous mengirim `null` untuk nama, callsign, rank, dan divisi. Pengguna tetap perlu menghindari detail pengenal di kolom cerita.
- Portal IA memakai Firebase Authentication dan Firestore. Pastikan hanya akun IA yang dibuat administrator yang ada di Authentication.
- Rules ini adalah baseline untuk komunitas roleplay, bukan sistem investigasi rahasia atau pelaporan dunia nyata. Rules tidak membatasi jumlah spam; bila website dibuka luas, tambahkan App Check / rate limiting atau backend tepercaya.

## Penting
Firebase Web API key bukan password dan memang digunakan di browser. Keamanan data bergantung pada Firestore Security Rules dan pembatasan akun Authentication. Jangan pernah memasukkan password akun, service account JSON, atau private key ke HTML/GitHub.
