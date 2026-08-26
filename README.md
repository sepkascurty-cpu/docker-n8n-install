# 🚀 n8n Self-Hosted on Windows — Docker

> **Run n8n locally on Windows using Docker Desktop + WSL 2.**
> Cocok untuk belajar automation, membuat workflow AI, integrasi API, webhook, Google Sheets, WhatsApp, dan berbagai kebutuhan automation lainnya.

<p align="center">

![Windows](https://img.shields.io/badge/Windows-10%2F11-0078D6?style=for-the-badge\&logo=windows\&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Desktop-2496ED?style=for-the-badge\&logo=docker\&logoColor=white)
![WSL2](https://img.shields.io/badge/WSL-2-0F80CC?style=for-the-badge\&logo=linux\&logoColor=white)
![n8n](https://img.shields.io/badge/n8n-Self--Hosted-EA4B71?style=for-the-badge\&logo=n8n\&logoColor=white)

</p>

---

## 📌 Daftar Isi

* [Tentang](#-tentang)
* [Arsitektur](#-arsitektur)
* [Persyaratan Sistem](#-persyaratan-sistem)
* [Instalasi](#️-instalasi)

  * [1. Install WSL 2](#1-install-wsl-2)
  * [2. Install Docker Desktop](#2-install-docker-desktop)
  * [3. Menjalankan n8n](#3-menjalankan-n8n)
* [Akses Dashboard](#-akses-dashboard)
* [Persistent Storage](#-persistent-storage)
* [Menjalankan Kembali](#-menjalankan-kembali)
* [Perintah Docker Berguna](#-perintah-docker-berguna)
* [Webhook & Akses Internet](#-webhook--akses-internet)
* [Troubleshooting](#-troubleshooting)
* [Catatan Keamanan](#️-catatan-keamanan)

---

## 🧠 Tentang

**n8n** adalah workflow automation platform yang memungkinkan kamu menghubungkan berbagai aplikasi, API, database, dan layanan menggunakan workflow visual.

Dengan setup ini, n8n berjalan secara:

* 🖥️ **Lokal** di komputer Windows
* 🐳 **Containerized** menggunakan Docker
* 💾 **Persistent** menggunakan Docker Volume
* 🔄 **Auto-restart** ketika Docker aktif kembali
* 🌐 Dapat diperluas untuk kebutuhan webhook dan integrasi eksternal

Setup ini cocok untuk:

```text
AI Automation
API Integration
Google Sheets Automation
Webhook
WhatsApp Automation
Database Workflow
Business Automation
Data Processing
```

---

# 🏗️ Arsitektur

Secara sederhana, environment yang digunakan:

```text
┌─────────────────────────────┐
│          Windows            │
│                             │
│  ┌───────────────────────┐  │
│  │        WSL 2          │  │
│  │                       │  │
│  │  ┌─────────────────┐  │  │
│  │  │ Docker Desktop  │  │  │
│  │  │                 │  │  │
│  │  │ ┌─────────────┐ │  │  │
│  │  │ │     n8n     │ │  │  │
│  │  │ │  Port 5678  │ │  │  │
│  │  │ └──────┬──────┘ │  │  │
│  │  └─────────┼────────┘  │  │
│  └────────────┼───────────┘  │
│               │              │
└───────────────┼──────────────┘
                │
         n8n_data Volume
                │
        Persistent Storage
```

### Alur akses

```text
Browser
   │
   ▼
localhost:5678
   │
   ▼
Docker
   │
   ▼
n8n Container
   │
   ▼
n8n Workflow
```

---

# 💻 Persyaratan Sistem

Sebelum instalasi, pastikan komputer memenuhi kebutuhan berikut:

| Komponen       |    Minimum | Rekomendasi |
| -------------- | ---------: | ----------: |
| OS             | Windows 10 |  Windows 11 |
| CPU            |     2 Core |     4 Core+ |
| RAM            |       4 GB |       8 GB+ |
| Storage        |     1–2 GB |       5 GB+ |
| Virtualization |    Enabled |     Enabled |
| Internet       |   Required |      Stable |

> **Note:** Workflow yang kompleks, AI automation, database, atau banyak container akan membutuhkan resource lebih besar.

---

# 🛠️ Instalasi

## 1. Install WSL 2

Docker Desktop pada Windows dapat menggunakan **WSL 2** sebagai backend Linux.

### Buka PowerShell sebagai Administrator

Kemudian jalankan:

```powershell
wsl --install
```

Tunggu sampai proses instalasi selesai.

Setelah itu:

```text
Restart komputer
        ↓
Windows boot kembali
        ↓
WSL 2 siap digunakan
```

### Cek instalasi WSL

Setelah restart, jalankan:

```powershell
wsl --status
```

Untuk melihat versi distro:

```powershell
wsl -l -v
```

Pastikan environment menggunakan **WSL 2**.

---

# 🐳 2. Install Docker Desktop

Download Docker Desktop dari website resmi:

**Docker Desktop for Windows**

https://www.docker.com/products/docker-desktop/

### Saat instalasi

Pastikan opsi berikut digunakan:

```text
☑ Use WSL 2 instead of Hyper-V
```

Setelah instalasi selesai:

1. Buka **Docker Desktop**
2. Accept terms jika diminta
3. Tunggu Docker selesai melakukan startup
4. Pastikan Docker sudah dalam kondisi **Running**

### Cek Docker

Buka CMD atau PowerShell:

```powershell
docker --version
```

Kemudian:

```powershell
docker info
```

Jika Docker berjalan normal, informasi Docker akan ditampilkan.

---

# ⚙️ 3. Menjalankan n8n

Setelah Docker Desktop aktif, buka **CMD** atau **PowerShell**.

## Step 1 — Buat Docker Volume

Volume digunakan untuk menyimpan data n8n secara persistent.

```bash
docker volume create n8n_data
```

Jika berhasil, Docker akan mengembalikan nama:

```text
n8n_data
```

---

## Step 2 — Jalankan Container n8n

Gunakan command berikut:

```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  --restart always \
  n8nio/n8n
```

> **Windows CMD:** Jika command multiline di atas tidak bekerja, gunakan versi satu baris di bawah.

```bash
docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n --restart always n8nio/n8n
```

### Penjelasan command

| Parameter                     | Fungsi                               |
| ----------------------------- | ------------------------------------ |
| `-d`                          | Menjalankan container di background  |
| `--name n8n`                  | Memberikan nama container            |
| `-p 5678:5678`                | Membuka port n8n                     |
| `-v n8n_data:/home/node/.n8n` | Menyimpan data n8n secara persistent |
| `--restart always`            | Restart container secara otomatis    |
| `n8nio/n8n`                   | Image resmi n8n                      |

---

# 🌐 Akses Dashboard

Setelah container berjalan, buka browser:

```text
http://localhost:5678
```

Jika semuanya berhasil, halaman setup n8n akan muncul.

```text
Browser
   │
   ▼
localhost:5678
   │
   ▼
n8n
   │
   ▼
Create Account
   │
   ▼
Workflow Editor
```

Setelah membuat akun lokal, kamu sudah bisa mulai membuat workflow.

---

# 💾 Persistent Storage

Salah satu bagian paling penting dari setup ini adalah:

```bash
-v n8n_data:/home/node/.n8n
```

Artinya data n8n disimpan di Docker Volume bernama:

```text
n8n_data
```

Sehingga ketika container dihentikan:

```bash
docker stop n8n
```

data workflow **tidak otomatis hilang**.

Bahkan ketika container dihapus:

```bash
docker rm n8n
```

volume `n8n_data` tetap ada selama tidak dihapus secara manual.

### Jangan menjalankan ini sembarangan

```bash
docker volume rm n8n_data
```

Karena command tersebut dapat menghapus storage yang berisi data n8n.

---

# 🔄 Menjalankan Kembali

Setelah komputer direstart, kamu **tidak perlu membuat container baru**.

Pastikan:

```text
Windows
   ↓
Docker Desktop
   ↓
n8n Container
   ↓
Browser
```

Karena menggunakan:

```bash
--restart always
```

Docker akan mencoba menjalankan kembali container n8n ketika Docker Engine aktif.

Kemudian buka:

```text
http://localhost:5678
```

### Jika n8n belum berjalan

Cek container:

```bash
docker ps
```

Jika container berhenti, jalankan:

```bash
docker start n8n
```

---

# 🔧 Perintah Docker Berguna

## Melihat container aktif

```bash
docker ps
```

## Melihat semua container

```bash
docker ps -a
```

## Melihat log n8n

```bash
docker logs n8n
```

Untuk melihat log secara realtime:

```bash
docker logs -f n8n
```

Tekan:

```text
CTRL + C
```

untuk keluar dari tampilan log.

---

## Stop n8n

```bash
docker stop n8n
```

## Start n8n

```bash
docker start n8n
```

## Restart n8n

```bash
docker restart n8n
```

## Menghapus container

```bash
docker rm n8n
```

> Menghapus **container** tidak sama dengan menghapus **volume**.

---

# 🌍 Webhook & Akses Internet

Secara default, setup ini hanya dapat diakses dari komputer lokal:

```text
http://localhost:5678
```

Artinya layanan eksternal tidak bisa langsung mengakses webhook n8n kamu.

Contohnya:

```text
WhatsApp
   │
   │ Webhook
   ▼
Internet
   X
localhost:5678
```

Untuk menerima request dari internet, kamu membutuhkan public endpoint.

Contoh arsitektur:

```text
External Service
      │
      ▼
 Public URL
      │
      ▼
 Tunnel / Reverse Proxy
      │
      ▼
 localhost:5678
      │
      ▼
     n8n
```

Untuk development/testing, kamu bisa menggunakan tunneling seperti **ngrok** atau **Cloudflare Tunnel**.

Untuk production, lebih baik mempertimbangkan **VPS + reverse proxy + HTTPS**.

---

# 🧪 Troubleshooting

## ❌ `docker` is not recognized

Kemungkinan Docker Desktop belum terinstall atau Docker belum berjalan.

Coba:

```bash
docker --version
```

Kemudian pastikan Docker Desktop sedang aktif.

---

## ❌ Port 5678 sudah digunakan

Cek container:

```bash
docker ps
```

Atau gunakan port lain, misalnya:

```bash
docker run -d --name n8n -p 8080:5678 -v n8n_data:/home/node/.n8n --restart always n8nio/n8n
```

Kemudian akses:

```text
http://localhost:8080
```

---

## ❌ Container tidak berjalan

Cek:

```bash
docker ps -a
```

Kemudian lihat log:

```bash
docker logs n8n
```

Jika perlu restart:

```bash
docker restart n8n
```

---

## ❌ n8n tidak bisa dibuka

Pastikan:

```bash
docker ps
```

menampilkan container `n8n` dalam kondisi running.

Kemudian coba:

```text
http://127.0.0.1:5678
```

Jika masih gagal, periksa log:

```bash
docker logs n8n
```

---

# 🔐 Catatan Keamanan

Setup ini ditujukan terutama untuk **local development dan learning environment**.

Jangan langsung mengekspos:

```text
localhost:5678
```

ke internet tanpa konfigurasi keamanan yang tepat.

Jika nantinya n8n digunakan untuk production:

* Gunakan HTTPS
* Gunakan authentication yang kuat
* Jangan membocorkan API key
* Gunakan environment variables untuk secret
* Gunakan reverse proxy
* Batasi akses jaringan
* Lakukan backup workflow dan database
* Update image n8n secara berkala

---

# 📦 Struktur Environment

Setelah setup selesai, environment kamu kurang lebih seperti ini:

```text
Windows 10/11
│
├── WSL 2
│
├── Docker Desktop
│   │
│   └── n8n Container
│       │
│       ├── Port: 5678
│       │
│       └── /home/node/.n8n
│               │
│               ▼
│           n8n_data
│
└── Browser
        │
        ▼
   localhost:5678
```

---

# 🚀 Quick Start

Kalau semua dependency sudah terinstall, cukup jalankan:

```bash
docker volume create n8n_data
```

Kemudian:

```bash
docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n --restart always n8nio/n8n
```

Lalu buka:

```text
http://localhost:5678
```

**Done. n8n sudah berjalan secara lokal di Windows.**

---

## 📚 Resources

* **n8n:** https://n8n.io/
* **n8n Documentation:** https://docs.n8n.io/
* **Docker Desktop:** https://www.docker.com/products/docker-desktop/
* **WSL Documentation:** https://learn.microsoft.com/windows/wsl/

---

<p align="center">

### ⚡ Built for Automation & Experimentation

**n8n + Docker + WSL 2**

</p>
