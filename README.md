### 🚀 Panduan Instalasi n8n Self-Hosted di Windows (Docker)

Panduan praktis untuk menjalankan **n8n Community Edition** secara gratis selamanya di komputer lokal Windows menggunakan Docker Desktop. Alur ini menggunakan konfigurasi *persistent storage*, sehingga seluruh *workflow* dan data Anda tetap tersimpan aman saat komputer dimatikan. 

### 💻 1. Spesifikasi Sistem Minimal

Sebelum memulai, pastikan perangkat Windows Anda memenuhi standar berikut: 

* **Sistem Operasi:** Windows 10/11 (Home atau Pro)
* **Prosesor:** Minimal 2 Core (Intel Core i3 / AMD Ryzen 3 ke atas)
* **RAM:** Minimal 4 GB (Disarankan 8 GB agar multitasking lancar)
* **Penyimpanan:** Menyediakan sisa ruang kosong sekitar 1-2 GB

### 🛠️ 2. Langkah-Langkah Instalasi

### 🔹 Langkah 1: Persiapan Windows (Instalasi WSL 2)

Docker Desktop di Windows membutuhkan komponen inti Linux bernama WSL 2. 

1. Buka **Command Prompt (CMD)** atau **PowerShell** sebagai Administrator (*Run as administrator*).
2. Jalankan perintah berikut: 

bash

wsl --install

Gunakan kode dengan hati-hati.
3. Tunggu hingga proses selesai, lalu **Restart (Mulai Ulang) komputer Anda**.

### 🔹 Langkah 2: Pasang Docker Desktop

1. Unduh installer resmi melalui tautan ini: **[Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/)**.
2. Jalankan file .exe yang telah diunduh.
3. **Penting:** Saat proses instalasi berjalan, pastikan opsi **"Use WSL 2 instead of Hyper-V"** dalam kondisi dicentang ✅.
4. Klik **OK**, tunggu hingga selesai, lalu buka aplikasi **Docker Desktop**.
5. Terima syarat & ketentuan (*Accept*), lalu tunggu hingga indikator di pojok kiri bawah berubah menjadi warna **Hijau** 🟢 (artinya Docker siap digunakan).

### 🔹 Langkah 3: Menjalankan n8n di Docker

Buka **Command Prompt (CMD)** biasa (tidak perlu akses administrator), kemudian eksekusi dua baris perintah ini secara berurutan: 

1. **Buat Volume Penyimpanan Data:**
Perintah ini membuat wadah penyimpanan khusus agar semua *workflow* Anda tidak terhapus otomatis saat kontainer n8n dihentikan. 

bash

docker volume create n8n_data

Gunakan kode dengan hati-hati.
2. **Unduh dan Jalankan Kontainer n8n:** 

bash

docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n --restart always n8nio/n8n

Gunakan kode dengan hati-hati.

  * *Opsi -d*: Menjalankan n8n di latar belakang agar CMD bisa ditutup.
  * *Opsi --restart always*: Mengatur agar n8n otomatis menyala setiap kali komputer hidup dan Docker Desktop aktif.

### 🎨 3. Akses Dashboard n8n (Klik-Klik)

1. Buka peramban/browser Anda (Chrome, Edge, dll.).
2. Masuk ke alamat URL berikut: 

http

http://localhost:5678

Gunakan kode dengan hati-hati.
3. Anda akan diarahkan untuk membuat akun lokal (Email & Password) terlebih dahulu.
4. Setelah itu, kanvas visual n8n siap digunakan untuk membuat otomatisasi dengan metode *drag-and-drop*.

### 🔄 4. Cara Akses di Hari Berikutnya

Jika komputer Anda baru dinyalakan kembali, Anda tidak perlu mengetik perintah kode di CMD lagi. Alurnya sangat mudah: 

1. Pastikan aplikasi **Docker Desktop** sudah terbuka di Windows Anda.
2. Buka browser dan langsung akses kembali **http://localhost:5678**.

### ⚠️ Catatan Penting

* **Konektivitas Lokal:** Karena n8n ini berjalan di laptop/PC pribadi, otomatisasi hanya akan berjalan aktif **selama komputer dalam keadaan menyala dan terhubung ke internet**.
* **Fitur Webhook:** Jika Anda membutuhkan fitur *Webhook* instan (menerima data langsung dari pihak ketiga seperti WhatsApp atau Google Forms), Anda memerlukan konfigurasi tambahan seperti **ngrok** atau memindahkan instalasi ini ke VPS (*Virtual Private Server*).
