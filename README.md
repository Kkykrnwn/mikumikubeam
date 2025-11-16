# Miku Miku Beam 💥⚡ (Network Stresser)

Server stress testing yang seru dan menarik dengan tampilan **bertema Miku**, di mana kamu bisa mengatur dan menjalankan attack sambil menikmati lagu keren di latar belakang! 🎤✨

![Screenshot](docs/screenshot.png)

## Fitur 🎉

* 🐳 **Siap Docker**: MMB siap dibangun dan dijalankan di dalam container Docker.
* 🌐 **Visualisasi Attack Secara Real-time**: Lihat progres dan statistik attack secara langsung saat berjalan. 🔥
* 🎶 **UI Bertema Miku**: Desain yang imut dan berwarna cerah dengan nuansa Miku, membuat prosesnya lebih menyenangkan. Termasuk lagu keren biar kamu makin semangat! 🎧
* 🧑‍💻 **Parameter Attack yang Bisa Dikonfigurasi**: Atur metode attack, ukuran paket, durasi, dan jeda paket dengan mudah melalui antarmuka frontend.
* 🛠️ **Penanganan Attack Berbasis Worker**: Server memproses attack di worker terpisah untuk performa dan skalabilitas yang optimal.
* 📊 **Statistik Langsung**: Pantau keberhasilan dan kegagalan tiap attack secara real-time. Lihat berapa banyak paket yang dikirim dan apakah berhasil atau gagal.
* 🖼️ **Desain Estetis**: Antarmuka yang imut dan enak dilihat agar pengalamanmu menyenangkan. 🌸
* 📡 **Metode Attack:**

  * `HTTP Flood` - Mengirim permintaan HTTP acak
  * `HTTP Bypass` - Mengirim permintaan HTTP yang meniru request nyata (redirect, cookies, headers, resources, dll)
  * `HTTP Slowloris` - Mengirim request HTTP dan mempertahankan koneksi tetap terbuka
  * `Minecraft Ping` - Mengirim ping/motd request Minecraft
  * `TCP Flood` - Mengirim paket TCP acak

## Setup 🛠️

### Prasyarat 📦

Pastikan kamu sudah menginstal:

* Node.js (v14 atau lebih tinggi) 🌱
* npm (Node Package Manager) 📦

### Mode Development 🔧

1. Clone repositori ini:

   ```bash
   git clone https://github.com/sammwyy/mikumikubeam.git
   cd mikumikubeam
   ```

2. Instal semua dependensi yang dibutuhkan:

   ```bash
   npm install
   ```

3. Buat file yang diperlukan:

   * `data/proxies.txt` - Daftar proxy.
   * `data/uas.txt` - Daftar user agent.

4. Jalankan server dalam mode pengembangan:

   ```bash
   npm run dev
   ```

   * **Frontend** berjalan di `http://localhost:5173`.
   * **Backend** berjalan di `http://localhost:3000`.

---

### Mode Produksi 💥

1. Clone repositori dan masuk ke direktori project:

   ```bash
   git clone https://github.com/sammwyy/mikumikubeam.git
   cd mikumikubeam
   ```

2. Instal dependensi:

   ```bash
   npm install
   ```

3. Build project:

   ```bash
   npm run build
   ```

4. Jalankan server dalam mode produksi:

   ```bash
   npm run start
   ```

   Dalam mode produksi, **frontend** dan **backend** akan dijalankan pada port yang sama (`http://localhost:3000`).

> Jangan lupa menambahkan file yang diperlukan `data/proxies.txt` dan `data/uas.txt`.

## Penggunaan ⚙️

Setelah server aktif dan berjalan, kamu bisa mengaksesnya lewat frontend:

1. **Mulai Attack**:

   * Atur parameter attack: target URL, metode attack (HTTP Flood, Slowloris, TCP, dll), ukuran paket, durasi, dan delay.
   * Tekan tombol "Start Attack" untuk memulai stress test.

2. **Hentikan Attack**:

   * Tekan tombol "Stop Attack" untuk menghentikan attack yang sedang berjalan.

### Contoh Request

```json
{
  "target": "http://example.com",
  "attackMethod": "http_flood",
  "packetSize": 512,
  "duration": 60,
  "packetDelay": 500
}
```

## Menambahkan Proxy dan User-Agent

Akses ke file `data/proxies.txt` dan `data/uas.txt` kini bisa dilakukan langsung lewat frontend. Klik tombol teks di sebelah kanan tombol beam untuk membuka editornya.

![AnnotatedImage](docs/annotated-button.png)

## Penanganan Attack Berbasis Worker 🔧💡

Setiap jenis attack ditangani dalam worker thread terpisah, memastikan server utama tetap responsif. Worker attack dimuat secara dinamis sesuai metode yang dipilih (HTTP, dll).

## To-Do 📝

* Tambahkan lebih banyak metode attack:

  * UDP 🌐
  * DNS 📡
  * Dan lainnya! 🔥

* Tingkatkan statistik dan laporan attack untuk pemantauan real-time yang lebih baik. 📊

## Kontribusi 💖

Silakan fork repo ini dan buat pull request untuk menambahkan protokol attack baru, memperbaiki bug, atau meningkatkan fitur. Kalau kamu punya ide fitur baru, jangan ragu untuk berbagi! 😄

### Menambahkan Metode Attack Baru ⚡

Untuk menambahkan metode attack baru (misalnya Minecraft, TCP, UDP, DNS), kamu bisa membuat file worker baru dan menambahkannya ke konfigurasi server.

Contohnya:

* Tambahkan metode attack baru di pengaturan frontend.
* Buat file worker yang sesuai (misalnya `minecraftAttack.js`).
* Perbarui konfigurasi handler attack agar mencakup metode baru tersebut.

```javascript
const attackHandlers = {
  http_flood: "./workers/httpFloodAttack.js",
  http_slowloris: "./workers/httpSlowlorisAttack.js",
  tcp_flood: "./workers/tcpFloodAttack.js",
  minecraft_ping: "./workers/minecraftPingAttack.js",

  // Tambahkan protokol lain jika diperlukan!
  your_protocol: "./workers/yourProtocolAttack.js"
};
```

---

### FAQ ❓

**1. Sistem operasi apa yang didukung MMB?**

> **Windows**, **Linux**, **Mac**, dan **Android (belum diuji)**

**2. Crash saat startup dengan error "concurrently"**

> Coba jalankan dua terminal terpisah.
> Terminal pertama: `npm run dev:client`
> Terminal kedua: `npm run dev:server`
> (Masalah ini sering terjadi pada beberapa pengguna Windows 11)

**3. Saya buka "[http://localhost:3000](http://localhost:3000)" tapi tidak muncul apa-apa.**

> Port `3000` adalah port server, untuk melihat UI kamu harus menggunakan port `5173` ([http://localhost:5173](http://localhost:5173)).

**4. Request gagal dikirim ke server target (Read timeout dan sejenisnya)**

> Kamu harus menambahkan proxy yang sesuai di file `data/proxies.txt`.
> Setiap baris berisi satu proxy yang digunakan untuk melakukan attack.
> Formatnya harus seperti ini:
>
> * `protocol://user:password@host:port` (Proxy dengan autentikasi)
> * `protocol://host:port`
> * `host:port` (secara default menggunakan http)
> * `host` (secara default menggunakan port 8080)

---

## Lisensi 📝

Project ini dilisensikan di bawah MIT License - lihat file [LICENSE](LICENSE) untuk detailnya.

---

## Disclaimer 🚨

Perlu diketahui bahwa project ini hanya untuk tujuan edukasi dan **tidak boleh digunakan untuk tujuan jahat**.

---

### (｡♥‿♥｡) Selamat Menjalankan Hacking 💖🎶