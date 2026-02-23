# 🚀 NTP Server Auto Installer (LXC Proxmox)

Script ini digunakan untuk melakukan instalasi dan konfigurasi otomatis **NTP Server (Chrony)** pada container **LXC Proxmox**.

Didukung untuk sistem operasi berikut:

- ✅ Rocky Linux 9  
- ✅ Rocky Linux 10  
- ✅ Ubuntu 24.04  
- ❌ OS lain akan otomatis ditolak  

Script ini dirancang untuk kebutuhan production maupun lab environment.

---

## 🔧 Fitur Utama

Script ini akan secara otomatis melakukan:

- 🔎 Deteksi sistem operasi (Rocky 9/10 & Ubuntu 24.04 only)
- ⬇️ Install `chrony` sesuai package manager OS
- 🌏 Set timezone ke **Asia/Jakarta**
- 🌐 Konfigurasi upstream NTP default:
  ```
  id.pool.ntp.org
  ```
- 🔓 Mengizinkan akses dari semua IP:
  ```
  allow 0.0.0.0/0
  ```
- 🛜 Auto-detect koneksi internet:
  - Jika internet tersedia → normal sync
  - Jika tidak tersedia → otomatis aktifkan `local stratum 10`
- 🔥 Konfigurasi firewall otomatis:
  - `firewalld` (Rocky)
  - `ufw` (Ubuntu)
  - Jika tidak ada firewall → skip
- 🖥️ Menambahkan login banner dinamis berisi:
  - IP Address server
  - Status service NTP
  - Upstream pool
  - Active source
  - Tracking status
- 📄 Membuat file dokumentasi:
  ```
  /root/README-NTP-SERVER.txt
  ```

---

## 📦 Default Konfigurasi

| Parameter | Nilai |
|------------|--------|
| Upstream Pool | id.pool.ntp.org |
| Timezone | Asia/Jakarta |
| Akses Client | All IP (0.0.0.0/0) |
| Config File | /etc/chrony.conf |

---

## 📥 Cara Penggunaan

### 1️⃣ Pastikan Container Fresh

Untuk Ubuntu:
```bash
apt update
```

Untuk Rocky:
```bash
dnf update -y
```

---

### 2️⃣ Jalankan Script (Direct Execution)

```bash
curl -sSL https://raw.githubusercontent.com/sigitapriyan/ntp-server/main/install-ntp-server.sh | bash
```

---

### 3️⃣ Atau Clone Repository

```bash
git clone https://github.com/USERNAME/REPO.git
cd REPO
chmod +x install-ntp-server.sh
sudo ./install-ntp-server.sh
```

---

## 🧪 Cara Cek Status NTP

### Rocky Linux
```bash
systemctl status chronyd
```

### Ubuntu
```bash
systemctl status chrony
```

### Monitoring
```bash
chronyc sources
chronyc tracking
```

---

## 🔄 Cara Mengubah Upstream Server

Edit file konfigurasi:

```bash
nano /etc/chrony.conf
```

Ubah bagian:

```
server 0.id.pool.ntp.org iburst
```

Lalu restart service:

### Rocky
```bash
systemctl restart chronyd
```

### Ubuntu
```bash
systemctl restart chrony
```

---

## ⚠️ Troubleshooting LXC Proxmox

Jika muncul error seperti:

```
Operation not permitted
Cannot adjust system clock
```

Kemungkinan container dalam mode **unprivileged**.

### Solusi:

- Ubah container menjadi **privileged**
- Atau tambahkan capability `CAP_SYS_TIME`

---

## 📁 File yang Dibuat Script

| File | Fungsi |
|-------|--------|
| /etc/chrony.conf | Konfigurasi utama NTP |
| /etc/profile.d/ntp-info.sh | Banner login dinamis |
| /root/README-NTP-SERVER.txt | Dokumentasi lokal server |

---

## 🔐 Catatan Keamanan

Konfigurasi default mengizinkan semua IP mengakses NTP server:

```
allow 0.0.0.0/0
```

Jika server terhubung langsung ke internet, sangat disarankan membatasi akses hanya ke network internal.

Contoh:

```
allow 192.168.0.0/16
```

---

## 👨‍💻 Author

**@kangsigi.id**  

---

## 📄 License

MIT License — bebas digunakan, dimodifikasi, dan dikembangkan sesuai kebutuhan.

---

⭐ Jika project ini membantu, jangan lupa beri star di GitHub.
