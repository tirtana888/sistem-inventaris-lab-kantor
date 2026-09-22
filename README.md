# 📦 Sistem Inventaris Barang Lab & Kantor (SPA)

Aplikasi Web *Single Page Application* (SPA) modern dan responsif untuk pencatatan dan pengelolaan inventaris laboratorium & kantor secara realtime. Dibangun menggunakan **HTML5**, **Bootstrap 5.3**, dan **Firebase Modular SDK v10** (Authentication & Realtime Database).

---

## 🚀 Fitur Utama

1. **Autentikasi Pengguna (Firebase Auth)**:
   - Form Login & Registrasi dengan validasi input (email & panjang password minimal 6 karakter).
   - Pengalihan tampilan instan (*reactive state management*) menggunakan `onAuthStateChanged()`.
   - Error handling ramah pengguna dalam Bahasa Indonesia (alert Bootstrap).
   - Logout session yang aman dengan `signOut()`.

2. **Manajemen Data Inventaris Realtime (CRUD)**:
   - **Create**: Tambah barang baru via modal interaktif (`push()` + `set()`).
   - **Read / Sync**: Sinkronisasi data realtime multi-user via `onValue()`.
   - **Update**: Edit rincian barang langsung dari modal (`update()`).
   - **Delete**: Hapus barang dengan dialog konfirmasi aman (`remove()`).

3. **Skema Data Node (`/inventaris`)**:
   - `namaBarang` (String)
   - `kategori` ("Elektronik", "ATK", "Perabot", "Hardware Jaringan")
   - `jumlah` (Number, minimal 1)
   - `kondisi` ("Baik", "Rusak Ringan", "Rusak Berat")
   - `lokasi` (String)
   - `createdBy` (Email petugas)
   - `updatedAt` (ISO Timestamp)

4. **Fitur Tambahan (UI/UX)**:
   - Ringkasan statistik (KPI Cards): Total Item, Total Unit Fisik, Kondisi Baik, Perlu Perbaikan.
   - Live Search (pencarian nama & lokasi) dan Filter ganda berdasarkan Kategori & Kondisi.
   - Badge warna status kondisi barang (Hijau = Baik, Kuning = Rusak Ringan, Merah = Rusak Berat).
   - Toast notification feedback untuk setiap aksi data.

---

## 🛠️ Panduan Konfigurasi Firebase Console

### 1. Buat Proyek Firebase
1. Buka [Firebase Console](https://console.firebase.google.com/).
2. Klik **Add project** (Tambah Proyek), masukkan nama proyek (misal: `inventaris-lab`), lalu lanjutkan hingga selesai.

### 2. Aktifkan Firebase Authentication
1. Di bilah menu sebelah kiri, masuk ke menu **Build** > **Authentication**.
2. Klik tombol **Get Started**.
3. Di tab **Sign-in method**, pilih penyedia **Email/Password**.
4. Aktifkan opsi **Email/Password** (opsi Email link tidak perlu dicentang), lalu klik **Save**.

### 3. Buat & Aktifkan Firebase Realtime Database
1. Di bilah menu sebelah kiri, masuk ke **Build** > **Realtime Database**.
2. Klik **Create Database**, pilih lokasi server terdekat (misal: `Singapore (asia-southeast1)` atau default `United States`).
3. Pilih mode awal **Start in locked mode** (atau test mode), lalu klik **Enable**.

### 4. Atur Security Rules Realtime Database
Pilih tab **Rules** pada Realtime Database, lalu masukkan aturan berikut yang **mewajibkan user terautentikasi (`auth != null`)**:

```json
{
  "rules": {
    "inventaris": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```
Klik tombol **Publish** untuk menerapkan aturan.

### 5. Atur Kredensial Firebase
Salin file `config.example.js` menjadi `config.js` (file ini otomatis diabaikan oleh `.gitignore` sehingga aman tidak ter-upload ke publik):

```bash
cp config.example.js config.js
```

Lalu sesuaikan nilai kredensial dengan yang didapat dari Firebase Console Anda:

```javascript
export const firebaseConfig = {
  apiKey: "YOUR_API_KEY_HERE",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT_ID-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

---

## 💻 Cara Menjalankan Aplikasi

Aplikasi ini bersifat Single Page Application (SPA) murni tanpa build tool.
Cukup buka file `index.html` di browser Anda:
- Bisa menggunakan ekstensi **Live Server** di VS Code / Antigravity, atau
- Jalankan via terminal PowerShell sederhana:
  ```powershell
  npx serve .
  # atau
  python -m http.server 8000
  ```
  Lalu buka `http://localhost:8000` di browser.
