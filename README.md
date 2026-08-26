# 🚀 n8n Self-Hosted on Windows — Docker

> **Menjalankan n8n Community Edition secara lokal di Windows menggunakan Docker Desktop + WSL 2.**

Setup ini dirancang untuk **development, learning, dan automation pribadi**. Data workflow disimpan menggunakan Docker Volume sehingga workflow tetap tersimpan meskipun komputer dimatikan atau container dihentikan.

<p align="center">

![Windows](https://img.shields.io/badge/Windows-10%2F11-0078D6?style=for-the-badge\&logo=windows\&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Desktop-2496ED?style=for-the-badge\&logo=docker\&logoColor=white)
![WSL2](https://img.shields.io/badge/WSL-2-0F80CC?style=for-the-badge\&logo=linux\&logoColor=white)
![n8n](https://img.shields.io/badge/n8n-Self--Hosted-EA4B71?style=for-the-badge\&logo=n8n\&logoColor=white)

</p>

---

## 📌 Daftar Isi

* [Tentang](#-tentang)
* [Fitur Setup](#-fitur-setup)
* [Arsitektur](#-arsitektur)
* [Persyaratan Sistem](#-persyaratan-sistem)
* [Instalasi](#️-instalasi)

  * [1. Install WSL 2](#1-install-wsl-2)
  * [2. Install Docker Desktop](#2-install-docker-desktop)
  * [3. Membuat Docker Volume](#3-membuat-docker-volume)
  * [4. Menjalankan n8n](#4-menjalankan-n8n)
* [Akses Dashboard](#-akses-dashboard)
* [Persistent Storage](#-persistent-storage)
* [Setelah Komputer Dimatikan](#-setelah-komputer-dimatikan)
* [Perintah Docker](#-perintah-docker)
* [Webhook & Akses Internet](#-webhook--akses-internet)
* [Troubleshooting](#-troubleshooting)
* [Catatan Keamanan](#️-catatan-keamanan)
* [Quick Start](#-quick-start)
* [Resources](#-resources)

---

# 🧠 Tentang

**n8n** adalah platform workflow automation yang memungkinkan kita menghubungkan berbagai aplikasi, API, database, dan layanan menggunakan workflow visual.

Dengan setup ini, n8n berjalan menggunakan:

```text
Windows
   │
   ├── WSL 2
   │
   └── Docker Desktop
          │
          └── n8n Container
                 │
                 └── Persistent Volume
```

Setup ini cocok untuk:

* AI Automation
* API Integration
* Google Sheets Automation
* Webhook
* WhatsApp Automation
* Database Workflow
* Business Automation
* Data Processing
* Internal Tools
* Learning & Experimentation

---

# ✨ Fitur Setup

| Fitur                  | Status                                             |
| ---------------------- | -------------------------------------------------- |
| 🖥️ Windows Support    | ✅                                                  |
| 🐳 Docker Container    | ✅                                                  |
| 🐧 WSL 2 Backend       | ✅                                                  |
| 💾 Persistent Storage  | ✅                                                  |
| 🔄 Auto Restart        | ✅                                                  |
| 🌐 Local Web Interface | ✅                                                  |
| 🔗 Webhook             | ⚠️ Perlu konfigurasi tambahan untuk akses internet |
| 🔐 Production Ready    | ⚠️ Perlu hardening tambahan                        |

---

# 🏗️ Arsitektur

Berikut gambaran sederhana environment yang digunakan:

```text
┌─────────────────────────────────────────┐
│                WINDOWS                  │
│                                         │
│  ┌───────────────────────────────────┐  │
│  │              WSL 2                │  │
│  │                                   │  │
│  │  ┌─────────────────────────────┐  │  │
│  │  │       Docker Desktop        │  │  │
│  │  │                             │  │  │
│  │  │   ┌─────────────────────┐   │  │  │
│  │  │   │        n8n          │   │  │  │
│  │  │   │      :5678          │   │  │  │
│  │  │   └──────────┬──────────┘   │  │  │
│  │  │              │              │  │  │
│  │  │              ▼              │  │  │
│  │  │         n8n_data            │  │  │
│  │  │      Persistent Volume      │  │  │
│  │  └─────────────────────────────┘  │  │
│  └───────────────────────────────────┘  │
│                                         │
└───────────────────┬─────────────────────┘
                    │
                    ▼
             localhost:5678
                    │
                    ▼
                 Browser
```

---

# 💻 Persyaratan Sistem

Sebelum memulai instalasi, pastikan komputer memenuhi kebutuhan berikut:

| Komponen         |    Minimum | Rekomendasi |
| ---------------- | ---------: | ----------: |
| Operating System | Windows 10 |  Windows 11 |
| CPU              |     2 Core |     4 Core+ |
| RAM              |       4 GB |       8 GB+ |
| Storage          |     1–2 GB |       5 GB+ |
| Virtualization   |    Enabled |     Enabled |
| Internet         |   Required |      Stable |

> **Note:** Jika workflow menggunakan AI, banyak node, database, atau beberapa container sekaligus, RAM dan CPU yang lebih besar akan memberikan performa yang lebih baik.

---

# 🛠️ Instalasi

## 1. Install WSL 2

Docker Desktop pada Windows dapat menggunakan **WSL 2** sebagai backend Linux.

### Buka PowerShell sebagai Administrator

Klik:

```text
Start
  ↓
PowerShell
  ↓
Run as Administrator
```

Kemudian jalankan:

```powershell
wsl --install
```

Tunggu hingga proses selesai.

Setelah selesai, **restart komputer**.

---

### Cek WSL

Setelah komputer menyala kembali, buka PowerShell:

```powershell
wsl --status
```

Kemudian:

```powershell
wsl -l -v
```

Pastikan distro yang digunakan berjalan menggunakan:

```text
VERSION 2
```

---

# 🐳 2. Install Docker Desktop

Download Docker Desktop melalui website resmi:

**Docker Desktop for Windows**

https://www.docker.com/products/docker-desktop/

Install Docker Desktop seperti biasa.

### Saat proses instalasi

Pastikan opsi:

```text
☑ Use WSL 2 instead of Hyper-V
```

dalam kondisi aktif.

Setelah instalasi selesai:

1. Buka **Docker Desktop**
2. Accept Terms jika diminta
3. Tunggu Docker Engine melakukan startup
4. Pastikan Docker sudah dalam kondisi **Running**

---

### Cek Docker

Buka CMD atau PowerShell:

```bash
docker --version
```

Kemudian:

```bash
docker info
```

Jika Docker berjalan normal, informasi mengenai Docker Engine akan ditampilkan.

---

# 💾 3. Membuat Docker Volume

Sebelum menjalankan n8n, buat volume untuk menyimpan data.

Jalankan:

```bash
docker volume create n8n_data
```

Jika berhasil, output akan terlihat seperti:

```text
n8n_data
```

Volume ini akan digunakan untuk menyimpan:

```text
Workflow
Credentials
Settings
User Data
n8n Configuration
```

---

# ⚙️ 4. Menjalankan n8n

Setelah Docker Desktop aktif, buka **CMD** atau **PowerShell**.

Jalankan command berikut:

```bash
docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n --restart always n8nio/n8n
```

Jika berhasil, Docker akan memberikan Container ID.

---

## 🔍 Penjelasan Command

| Parameter                     | Fungsi                                              |
| ----------------------------- | --------------------------------------------------- |
| `docker run`                  | Membuat dan menjalankan container                   |
| `-d`                          | Menjalankan container di background                 |
| `--name n8n`                  | Memberikan nama container `n8n`                     |
| `-p 5678:5678`                | Mapping port host ke port n8n                       |
| `-v n8n_data:/home/node/.n8n` | Menyimpan data secara persistent                    |
| `--restart always`            | Meminta Docker me-restart container secara otomatis |
| `n8nio/n8n`                   | Docker image n8n                                    |

---

# 🌐 Akses Dashboard

Setelah container berjalan, buka browser.

Masukkan:

```text
http://localhost:5678
```

Atau:

```text
http://127.0.0.1:5678
```

Jika berhasil, halaman n8n akan muncul.

Kemudian buat akun lokal sesuai instruksi yang diberikan n8n.

Setelah selesai:

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
Workflow Editor
```

Sekarang n8n siap digunakan.

---

# 💾 Persistent Storage

Setup ini menggunakan:

```bash
-v n8n_data:/home/node/.n8n
```

Artinya data n8n disimpan pada Docker Volume:

```text
n8n_data
```

Sehingga:

```text
Container Stop
      ↓
Data Tetap Ada
```

dan:

```text
Container Restart
      ↓
Data Tetap Ada
```

Bahkan jika container dihapus:

```bash
docker rm n8n
```

volume `n8n_data` tetap ada selama tidak dihapus secara manual.

---

## ⚠️ Jangan Hapus Volume Sembarangan

Jangan menjalankan:

```bash
docker volume rm n8n_data
```

kecuali memang ingin menghapus data persistent n8n.

Menghapus volume dapat menyebabkan data yang tersimpan di dalamnya hilang.

---

# 🔌 Setelah Komputer Dimatikan

Ini bagian penting.

Jika komputer dimatikan atau restart, **kamu tidak perlu menginstall n8n lagi**.

Kamu juga **tidak perlu menjalankan `docker run` lagi**.

Data workflow tetap tersimpan di:

```text
n8n_data
```

---

## 1. Nyalakan Komputer

Setelah Windows selesai boot:

```text
Windows
   ↓
Login
```

---

## 2. Buka Docker Desktop

Jalankan:

```text
Docker Desktop
```

Tunggu sampai Docker Engine aktif.

Pastikan Docker Desktop sudah dalam kondisi:

```text
Running
```

---

## 3. Cek Container n8n

Buka CMD atau PowerShell:

```bash
docker ps
```

Jika muncul:

```text
CONTAINER ID   IMAGE       STATUS        NAMES
xxxxxxxxxxxx   n8nio/n8n   Up ...        n8n
```

berarti n8n sudah berjalan.

### Jika sudah `Up`

Tidak perlu menjalankan command tambahan.

Langsung buka:

```text
http://localhost:5678
```

---

# 🔧 Jika n8n Tidak Otomatis Berjalan

Jika `docker ps` tidak menampilkan n8n, cek semua container:

```bash
docker ps -a
```

Jika muncul:

```text
n8n    Exited
```

jalankan:

```bash
docker start n8n
```

Kemudian cek:

```bash
docker ps
```

Jika sudah:

```text
n8n    Up ...
```

buka:

```text
http://localhost:5678
```

---

# 🧭 Alur Setelah Restart

```text
┌──────────────────────┐
│   Nyalakan Windows   │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│   Buka Docker        │
│      Desktop         │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│ Docker Engine        │
│      Running         │
└──────────┬───────────┘
           ↓
      docker ps
           │
      ┌────┴────┐
      │         │
      ▼         ▼
    n8n Up    n8n Exited
      │         │
      │         ▼
      │    docker start n8n
      │         │
      └────┬────┘
           ↓
┌──────────────────────┐
│ http://localhost:5678│
└──────────────────────┘
```

---

# ❌ Yang Tidak Perlu Dilakukan Setelah Restart

Jangan menjalankan ulang:

```bash
docker volume create n8n_data
```

dan jangan menjalankan ulang:

```bash
docker run -d --name n8n ...
```

Karena container dan volume sudah dibuat sebelumnya.

Jika menjalankan `docker run` lagi dengan nama container yang sama, kemungkinan muncul error:

```text
Conflict. The container name "/n8n" is already in use
```

### Cukup ingat:

```text
💻 Nyalakan PC
      ↓
🐳 Buka Docker Desktop
      ↓
🔍 docker ps
      ↓
🌐 localhost:5678
```

Jika belum running:

```bash
docker start n8n
```

---

# 🔧 Perintah Docker

Berikut beberapa command yang berguna untuk mengelola n8n.

### Melihat container aktif

```bash
docker ps
```

### Melihat semua container

```bash
docker ps -a
```

### Melihat log n8n

```bash
docker logs n8n
```

### Melihat log secara realtime

```bash
docker logs -f n8n
```

Tekan:

```text
CTRL + C
```

untuk keluar dari log.

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

> Menghapus container **tidak sama** dengan menghapus volume.

---

# 🌍 Webhook & Akses Internet

Secara default, n8n pada setup ini hanya dapat diakses secara lokal melalui:

```text
http://localhost:5678
```

External service di internet tidak bisa langsung mengakses `localhost`.

Contohnya:

```text
External Service
      │
      ▼
    Internet
      │
      X
      │
localhost:5678
```

Hal ini menjadi masalah jika kamu ingin menerima webhook dari:

* WhatsApp
* Telegram
* Google Forms
* Payment Gateway
* GitHub
* API eksternal
* Service lainnya

---

## 🔗 Solusi untuk Development

Untuk development/testing, kamu dapat menggunakan tunneling seperti:

```text
Internet
   │
   ▼
Public URL
   │
   ▼
Tunnel
   │
   ▼
localhost:5678
   │
   ▼
n8n
```

Contoh teknologi yang dapat digunakan:

* ngrok
* Cloudflare Tunnel

---

## 🏢 Untuk Production

Jika n8n nantinya digunakan sebagai sistem production, lebih baik gunakan:

```text
Internet
   │
   ▼
Domain
   │
   ▼
HTTPS
   │
   ▼
Reverse Proxy
   │
   ▼
n8n
   │
   ▼
Database / Storage
```

Untuk penggunaan serius, VPS atau server dedicated biasanya lebih cocok daripada laptop pribadi karena membutuhkan uptime yang lebih stabil.

---

# 🧪 Troubleshooting

## ❌ Docker tidak dikenali

Jika muncul:

```text
'docker' is not recognized...
```

pastikan:

1. Docker Desktop sudah terinstall
2. Docker Desktop sedang berjalan
3. Terminal sudah dibuka ulang

Kemudian cek:

```bash
docker --version
```

---

## ❌ Container n8n tidak berjalan

Cek:

```bash
docker ps -a
```

Kemudian:

```bash
docker logs n8n
```

Jika diperlukan:

```bash
docker restart n8n
```

---

## ❌ localhost:5678 tidak bisa dibuka

Cek:

```bash
docker ps
```

Pastikan ada:

```text
n8n    Up ...
```

Jika tidak ada:

```bash
docker start n8n
```

Kemudian coba:

```text
http://localhost:5678
```

atau:

```text
http://127.0.0.1:5678
```

---

## ❌ Port 5678 sudah digunakan

Jika port `5678` sudah digunakan aplikasi lain, gunakan port berbeda.

Contoh:

```bash
docker run -d --name n8n -p 8080:5678 -v n8n_data:/home/node/.n8n --restart always n8nio/n8n
```

Kemudian akses:

```text
http://localhost:8080
```

---

## ❌ Container name sudah digunakan

Jika muncul:

```text
Conflict. The container name "/n8n" is already in use
```

Jangan membuat container baru.

Cek:

```bash
docker ps -a
```

Jika container `n8n` sudah ada, cukup jalankan:

```bash
docker start n8n
```

---

# 🔐 Catatan Keamanan

Setup ini terutama ditujukan untuk:

* Learning
* Development
* Testing
* Personal Automation

Jangan langsung mengekspos n8n ke internet tanpa konfigurasi keamanan yang tepat.

Jika digunakan untuk production, pertimbangkan:

* HTTPS
* Strong authentication
* Secure credentials
* Environment variables
* Reverse proxy
* Firewall
* Network segmentation
* Regular backup
* Regular update
* Access control
* Monitoring

### Jangan commit secret ke Git

Jangan menyimpan:

```text
API Key
Password
Token
Webhook Secret
Database Credential
Private Key
```

langsung di repository.

Gunakan environment variables atau credential management yang sesuai.

---

# 📦 Struktur Environment

Setelah instalasi selesai, environment akan terlihat seperti:

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

Jika WSL 2 dan Docker Desktop sudah terinstall, cukup jalankan:

### 1. Buat volume

```bash
docker volume create n8n_data
```

### 2. Jalankan n8n

```bash
docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n --restart always n8nio/n8n
```

### 3. Buka browser

```text
http://localhost:5678
```

### 4. Setelah komputer restart

Tidak perlu install ulang.

Cukup:

```bash
docker ps
```

Jika `n8n` belum berjalan:

```bash
docker start n8n
```

Kemudian:

```text
http://localhost:5678
```

---

# 📋 Cheat Sheet

| Kebutuhan           | Command              |
| ------------------- | -------------------- |
| Cek container aktif | `docker ps`          |
| Cek semua container | `docker ps -a`       |
| Start n8n           | `docker start n8n`   |
| Stop n8n            | `docker stop n8n`    |
| Restart n8n         | `docker restart n8n` |
| Lihat log           | `docker logs n8n`    |
| Live log            | `docker logs -f n8n` |
| Cek Docker          | `docker info`        |
| Cek volume          | `docker volume ls`   |

### ⭐ Command yang paling sering dipakai

Setelah komputer restart:

```bash
docker ps
```

Kalau belum running:

```bash
docker start n8n
```

Lalu buka:

```text
http://localhost:5678
```

---

# 📚 Resources

* **n8n:** https://n8n.io/
* **n8n Documentation:** https://docs.n8n.io/
* **Docker Desktop:** https://www.docker.com/products/docker-desktop/
* **Docker Documentation:** https://docs.docker.com/
* **WSL Documentation:** https://learn.microsoft.com/windows/wsl/

---

<p align="center">

## ⚡ n8n + Docker + WSL 2

**Local Automation Environment**

Built for learning, experimentation, and automation.

</p>
