# Toko Microservices

Project tugas Sistem Terdistribusi dengan API Gateway dan empat microservice. Setiap service memakai Express dan data seed in-memory.

## Menjalankan
```bash
npm install
npm start
# atau seluruh service
docker compose up --build
```
Gateway: `http://localhost:3000`. Login seed: `admin@toko.test` / `admin123`.

## Endpoint utama
`POST /api/register`, `POST /api/login`, `GET /api/products`, `POST /api/orders`, `POST /api/payments`, `GET /api/profile`.
Endpoint terlindungi memakai `Authorization: Bearer <token>`.

## Menjalankan Service di Device Berbeda

Setiap anggota dapat menjalankan service sesuai job desk di laptop masing-masing. Pastikan semua laptop berada di jaringan Wi-Fi/LAN yang sama.

| Peserta | Service | Port |
|---|---|---:|
| 1 | `katalog-service` | 3001 |
| 2 | `pelanggan-service` | 3002 |
| 3 | `order-service` | 3003 |
| 4 | `pembayaran-service` | 3004 |
| 5 | `gateway` dan UI | 3000 |

Di setiap laptop, clone repository lalu install dependency:

```bash
git clone URL_REPO_KALIAN
cd microservices-tugaskelompok
npm install
```

Jalankan service masing-masing:

```bash
cd katalog-service
npm install
node server.js
```

Sesuaikan nama folder untuk peserta lainnya. Cari alamat IP laptop dengan:

```bash
ipconfig
```

Gunakan alamat `IPv4 Address`, misalnya `192.168.1.11`. Pada laptop Peserta 5, arahkan Gateway ke IP setiap service:

```powershell
$env:KATALOG_URL="http://192.168.1.11:3001"
$env:PELANGGAN_URL="http://192.168.1.12:3002"
$env:ORDER_URL="http://192.168.1.13:3003"
$env:PEMBAYARAN_URL="http://192.168.1.14:3004"
node gateway/server.js
```

Ganti IP sesuai alamat asli setiap laptop. Semua anggota kemudian membuka UI melalui IP laptop Peserta 5:

```text
http://192.168.1.15:3000
```

Jangan memakai `localhost` dari laptop lain, karena `localhost` menunjuk ke laptop yang sedang digunakan. Untuk menguji koneksi, buka endpoint berikut dari browser:

```text
http://IP_PESERTA_1:3001/health
http://IP_PESERTA_2:3002/health
http://IP_PESERTA_3:3003/health
http://IP_PESERTA_4:3004/health
```

Jika tidak bisa diakses, izinkan Node.js melalui Windows Firewall atau buka port service:

```powershell
New-NetFirewallRule -DisplayName "Toko Microservices" -Direction Inbound -Protocol TCP -LocalPort 3000,3001,3002,3003,3004 -Action Allow
```

## Pembagian push
- Peserta 1: `katalog-service/`
- Peserta 2: `pelanggan-service/`
- Peserta 3: `order-service/`
- Peserta 4: `pembayaran-service/`
- Peserta 5: `gateway/`, `tests/`, `docker-compose.yml`, `README.md`, `package.json`
