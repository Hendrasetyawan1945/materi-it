# MODUL LENGKAP JARINGAN KOMPUTER
Subnetting • NAT • DHCP • DNS • TCP/UDP • VLAN • Routing • Port
Referensi Ujian SKB TIK | Persiapan CPNS & Ujian Jaringan

---

## Daftar Topik & Prioritas Ujian
| No | Topik | Prioritas Ujian |
|----|-------|-----------------|
| 1 | Subnetting & CIDR | ⭐⭐⭐⭐⭐ SANGAT TINGGI |
| 2 | NAT (Static / Dynamic / PAT) | ⭐⭐⭐⭐⭐ SANGAT TINGGI |
| 3 | TCP vs UDP | ⭐⭐⭐⭐ TINGGI |
| 4 | Port Service Penting | ⭐⭐⭐⭐⭐ SANGAT TINGGI |
| 5 | DHCP & Proses DORA | ⭐⭐⭐⭐ TINGGI |
| 6 | DNS & Record Type | ⭐⭐⭐⭐ TINGGI |
| 7 | VLAN | ⭐⭐⭐ SEDANG |
| 8 | Routing (Static & Dynamic) | ⭐⭐⭐ SEDANG |
| 9 | IPv4 Private vs Public | ⭐⭐⭐⭐ TINGGI |
| 10 | Jebakan Soal Ujian | ⭐⭐⭐⭐⭐ WAJIB |

---

## 1. SUBNETTING & CIDR
Subnetting = membagi satu network besar menjadi sub-network kecil. Topik ini PALING SERING keluar di ujian!

### A. Rumus Inti Subnetting
| Rumus | Formula | Keterangan |
|-------|---------|------------|
| Host Bit | `32 − CIDR` | Jumlah bit untuk host |
| Total IP | `2^(Host Bit)` | Semua IP dalam subnet |
| Host Valid | `Total IP − 2` | IP yang bisa dipakai (min. 2 dibuang: network & broadcast) |
| Block Size | `256 − Subnet Mask oktet terakhir` | Jarak antar subnet |
| Network Address | `IP pertama subnet` | Tidak bisa dipakai untuk host |
| Broadcast Address | `Network + Block Size − 1` | IP terakhir subnet, tidak bisa dipakai untuk host |
| Host Range | `Network+1  s/d  Broadcast−1` | IP yang bisa diassign ke host |

### B. Tabel CIDR Lengkap (Wajib Hafal)
| CIDR | Subnet Mask | Block Size | Total IP | Host Valid | Jumlah Subnet* |
|------|-------------|------------|----------|------------|----------------|
| /24 | `255.255.255.0` | 256 | 256 | 254 | 1 |
| /25 | `255.255.255.128` | 128 | 128 | 126 | 2 |
| /26 | `255.255.255.192` | 64 | 64 | 62 | 4 |
| /27 | `255.255.255.224` | 32 | 32 | 30 | 8 |
| /28 | `255.255.255.240` | 16 | 16 | 14 | 16 |
| /29 | `255.255.255.248` | 8 | 8 | 6 | 32 |
| /30 | `255.255.255.252` | 4 | 4 | 2 | 64 |
| /23 | `255.255.254.0` | 512 | 512 | 510 | 1/2 dari /24 |
| /22 | `255.255.252.0` | 1024 | 1024 | 1022 | 1/4 dari /24 |
| /16 | `255.255.0.0` | 65536 | 65536 | 65534 | Kelas B |
| /8 | `255.0.0.0` | 16777216 | 16777216 | 16777214 | Kelas A |

*Dihitung dari /24 sebagai referensi

### C. Contoh Soal Lengkap
**Soal**: Berikan informasi lengkap untuk IP `192.168.10.65/27`

| Langkah | Perhitungan | Hasil |
|---------|-------------|-------|
| Host Bit | `32 − 27` | 5 |
| Total IP | `2⁵` | 32 |
| Host Valid | `32 − 2` | 30 |
| Block Size | `256 − 224 (mask /27)` | 32 |
| Network ke-1 | | `192.168.10.0` (blok 0–31) |
| Network ke-2 | | `192.168.10.32` (blok 32–63) |
| Network ke-3 ✓ | | `192.168.10.64` (IP .65 ada di blok ini) |
| Broadcast | `192.168.10.64 + 32 − 1` | `192.168.10.95` |
| Host Range | | `192.168.10.65 → 192.168.10.94` |
| Subnet Mask | | `255.255.255.224` (/27) |

### D. Contoh Soal 2 – VLSM (Variable Length Subnet Mask)
**Soal**: Bagi `192.168.1.0/24` untuk 3 departemen: IT(50 host), HR(25 host), Finance(10 host)

| Departemen | Kebutuhan Host | CIDR | Subnet Mask | Network | Broadcast | Range Host |
|------------|----------------|------|-------------|---------|-----------|------------|
| IT | 50 host → /26 (62 host) | `/26` | `255.255.255.192` | `192.168.1.0` | `192.168.1.63` | `192.168.1.1 – .62` |
| HR | 25 host → /27 (30 host) | `/27` | `255.255.255.224` | `192.168.1.64` | `192.168.1.95` | `192.168.1.65 – .94` |
| Finance | 10 host → /28 (14 host) | `/28` | `255.255.255.240` | `192.168.1.96` | `192.168.1.111` | `192.168.1.97 – .110` |

### E. Jebakan Subnetting di Soal
| Jebakan | Penjelasan | Contoh Benar |
|---------|-------------|--------------|
| Network address dianggap host | Network IP (pertama) TIDAK bisa dipakai | `192.168.1.0` bukan host valid |
| Broadcast dianggap host | Broadcast (terakhir) TIDAK bisa dipakai | `192.168.1.255` bukan host valid |
| Salah hitung block size | `/26 → 256−192 = 64, bukan 192` | Block size /26 = 64 |
| Lupa dikurangi 2 | Host valid = Total IP − 2 (bukan Total IP) | `32 − 2 = 30 host` |
| Salah identifikasi subnet | `/27 punya 8 subnet dalam /24` | `0,32,64,96,128,160,192,224` |

---

## 2. NAT – Network Address Translation
NAT = mengubah IP private menjadi IP public agar bisa berkomunikasi dengan internet. Bekerja di ROUTER (Layer 3).

### A. Jenis-Jenis NAT
| Jenis NAT | Cara Kerja | Penggunaan | Contoh |
|-----------|------------|--------------|--------|
| Static NAT | 1 IP private ↔ 1 IP public (tetap) | Server, Web Server, Mail Server | `192.168.1.10 → 203.1.1.10` (selalu) |
| Dynamic NAT | Pool IP private dipetakan ke pool IP public | Perusahaan besar | `192.168.1.x → 203.1.1.x` (bergilir) |
| PAT / NAT Overload | Banyak IP private → 1 IP public (beda port) | Rumah, kantor (PALING UMUM) | `192.168.1.10:5000 → 8.8.8.8:30000` |

### B. Cara Kerja PAT (Port Address Translation)
PAT = NAT Overload. Satu IP public melayani BANYAK client dengan membedakan port number.

| Client (Inside) | Port Sumber → IP Public | Port NAT | Tujuan |
|----------------|--------------------------|----------|--------|
| `192.168.1.10` | `5000 → 8.8.8.8` | `30000` | Server internet |
| `192.168.1.11` | `5001 → 8.8.8.8` | `30001` | Server internet |
| `192.168.1.12` | `5002 → 8.8.8.8` | `30002` | Server internet |

### C. Terminologi NAT (Cisco)
| Istilah | Arti | Keterangan |
|---------|------|-------------|
| Inside Local | IP private client | `192.168.1.10` – IP asli di jaringan lokal |
| Inside Global | IP public mewakili client | `203.1.1.5` – Setelah ditranslasi NAT |
| Outside Local | IP tujuan dari sisi lokal | Biasanya sama dengan Outside Global |
| Outside Global | IP tujuan sebenarnya | `8.8.8.8` – IP server di internet |

### D. Manfaat & Kelemahan NAT
| ✅ Manfaat NAT | ❌ Kelemahan NAT |
|---------------|-----------------|
| • Menghemat IPv4 public | • Menambah latency |
| • Keamanan (IP private tersembunyi) | • Tidak mendukung beberapa protokol (FTP, VoIP murni) |
| • Memungkinkan jaringan besar dengan 1 IP public | • Menyulitkan tracing & logging |

---

## 3. TCP vs UDP

### A. Perbandingan TCP vs UDP
| Aspek | TCP | UDP |
|-------|-----|-----|
| Tipe koneksi | Connection-Oriented | Connectionless |
| Handshake | 3-Way Handshake (SYN→SYN-ACK→ACK) | Tidak ada handshake |
| Keandalan | Reliable – ada ACK & retransmisi | Unreliable – tidak ada jaminan |
| Urutan data | Dijamin berurutan (sequencing) | Tidak dijamin urutannya |
| Flow Control | Ada (windowing) | Tidak ada |
| Error Control | Ada (checksum + retransmit) | Checksum saja, tidak retransmit |
| Kecepatan | Lebih lambat (overhead tinggi) | Lebih cepat (overhead rendah) |
| Penggunaan | HTTP, HTTPS, FTP, SSH, SMTP, Telnet | DNS, VoIP, Video Streaming, DHCP, SNMP |
| Port Layer | Layer 4 – Transport | Layer 4 – Transport |
| Header size | 20 bytes minimum | 8 bytes saja |

### B. Three-Way Handshake TCP
🤝 Three-Way Handshake = proses membangun koneksi TCP sebelum data dikirim.

| Langkah | Dari | Ke | Flag | Keterangan |
|---------|------|----|------|------------|
| 1 | Client | Server | SYN | Client minta buka koneksi (Synchronize) |
| 2 | Server | Client | SYN-ACK | Server setuju & balas (Synchronize-Acknowledge) |
| 3 | Client | Server | ACK | Client konfirmasi, koneksi terbentuk |

🔚 Four-Way Termination: `FIN → ACK → FIN → ACK` (menutup koneksi TCP)

### C. Kapan Pakai TCP, Kapan UDP?
| Gunakan TCP jika... | Gunakan UDP jika... |
|-----------------------|---------------------|
| • Data WAJIB sampai (transaksi, file) | • Kecepatan lebih penting dari keandalan |
| • Urutan data penting | • Toleran terhadap packet loss |
| • Error tidak boleh terjadi | • Real-time communication |
| Contoh: Transfer file, web, email | Contoh: Video call, live stream, gaming |

---

## 4. PORT SERVICE PENTING
🔥 Port = pintu komunikasi di Layer 4. Range: 0-1023 (Well Known), 1024-49151 (Registered), 49152-65535 (Dynamic). WAJIB HAFAL!

### A. Tabel Port Lengkap (Wajib Hafal)
| Port | Service | Protokol | Keterangan |
|------|---------|----------|------------|
| 20 | FTP Data | TCP | Transfer data file (FTP aktif) |
| 21 | FTP Control | TCP | Kontrol koneksi FTP |
| 22 | SSH | TCP | Remote login terenkripsi |
| 23 | Telnet | TCP | Remote login TIDAK terenkripsi (usang) |
| 25 | SMTP | TCP | Kirim email (Send) |
| 53 | DNS | UDP/TCP | Resolusi domain ke IP |
| 67 | DHCP Server | UDP | Server memberikan IP ke client |
| 68 | DHCP Client | UDP | Client menerima IP dari server |
| 69 | TFTP | UDP | Transfer file sederhana (tanpa autentikasi) |
| 80 | HTTP | TCP | Web tidak terenkripsi |
| 110 | POP3 | TCP | Terima email (download & hapus dari server) |
| 119 | NNTP | TCP | Network News Transfer Protocol |
| 123 | NTP | UDP | Sinkronisasi waktu (Network Time Protocol) |
| 143 | IMAP | TCP | Terima email (sinkron, tidak hapus di server) |
| 161 | SNMP | UDP | Monitoring jaringan (Simple Network Management) |
| 443 | HTTPS | TCP | Web terenkripsi (SSL/TLS) |
| 445 | SMB | TCP | File sharing Windows |
| 3389 | RDP | TCP | Remote Desktop Protocol (Windows) |
| 3306 | MySQL | TCP | Database MySQL |
| 5432 | PostgreSQL | TCP | Database PostgreSQL |

### B. Kelompok Port Berdasarkan Fungsi
| 📧 Email | 🌐 Web & Remote | 🔧 Infrastruktur |
|---------|----------------|-------------------|
| SMTP = 25 (kirim) | HTTP = 80 | DNS = 53 |
| POP3 = 110 (terima-hapus) | HTTPS = 443 | DHCP = 67/68 |
| IMAP = 143 (terima-simpan) | SSH = 22 | NTP = 123 |
| | RDP = 3389 | SNMP = 161 |
| | Telnet = 23 | FTP = 20/21 |

### C. POP3 vs IMAP – Sering Keluar!
| Aspek | POP3 (Port 110) | IMAP (Port 143) |
|-------|-----------------|-----------------|
| Cara kerja | Download email, lalu HAPUS dari server | Sinkronisasi – email TETAP di server |
| Akses multi-device | ❌ Tidak ideal | ✅ Sangat baik |
| Butuh internet terus? | ❌ Tidak (setelah download) | ✅ Ya (sinkron online) |
| Cocok untuk | 1 perangkat, internet terbatas | Banyak perangkat, internet cepat |

---

## 5. DHCP – Dynamic Host Configuration Protocol
⚡ DHCP memberikan konfigurasi IP secara otomatis kepada client. Port: 67 (Server) & 68 (Client). Protokol: UDP.

### A. Proses DORA (Wajib Hafal!)
| Langkah | Nama | Arah | Penjelasan | Transport |
|---------|------|------|------------|----------|
| D | DISCOVER | Client → Broadcast | Client mencari DHCP server (belum punya IP) | UDP Broadcast |
| O | OFFER | Server → Client | Server menawarkan IP yang tersedia | UDP Unicast/Broadcast |
| R | REQUEST | Client → Broadcast | Client memilih & meminta IP yang ditawarkan | UDP Broadcast |
| A | ACK | Server → Client | Server mengkonfirmasi pemberian IP | UDP Unicast |

### B. Informasi yang Diberikan DHCP
| Parameter | Keterangan | Contoh |
|-----------|------------|--------|
| IP Address | Alamat IP yang diberikan ke client | `192.168.1.100` |
| Subnet Mask | Mask jaringan | `255.255.255.0` |
| Default Gateway | Alamat router untuk keluar jaringan | `192.168.1.1` |
| DNS Server | Alamat server untuk resolusi nama domain | `8.8.8.8` |
| Lease Time | Durasi pemakaian IP (setelah habis, perbarui) | `86400 detik (24 jam)` |
| DHCP Server IP | Alamat server DHCP | `192.168.1.1` |

### C. DHCP Relay Agent
📡 DHCP Relay Agent digunakan ketika client dan server DHCP berada di network yang BERBEDA. Router yang memforward pesan DISCOVER ke server.

Alur: `Client → [Discover] → Router (Relay Agent) → [Forward ke Server DHCP] → [Offer kembali ke Client]`

### D. APIPA – Automatic Private IP Addressing
Jika DHCP gagal, Windows otomatis assign IP dari range `169.254.0.0/16`. Ini menandakan masalah DHCP!

| Kondisi | IP yang Diassign | Artinya |
|---------|-----------------|---------|
| DHCP berhasil | `192.168.x.x / 10.x.x.x` (sesuai server) | Normal |
| DHCP gagal (Windows) | `169.254.x.x` (APIPA) | ❌ Cek koneksi ke DHCP server |
| IP manual (static) | Sesuai konfigurasi admin | Tidak bergantung DHCP |

---

## 6. DNS – Domain Name System
🔍 DNS = menerjemahkan domain ke IP address (dan sebaliknya). Port: 53. Protokol: UDP (query) & TCP (transfer zona).

### A. Jenis Record DNS (Wajib Hafal!)
| Record | Fungsi | Contoh |
|--------|--------|--------|
| A | Domain → IPv4 | `google.com → 142.250.190.14` |
| AAAA | Domain → IPv6 | `google.com → 2404:6800::` |
| CNAME | Alias → Domain lain (canonical name) | `www.google.com → google.com` |
| MX | Mail eXchanger – server email | `google.com → aspmx.l.google.com` |
| NS | Name Server – server DNS authoritative | `google.com → ns1.google.com` |
| PTR | Reverse lookup: IP → Domain | `142.250.190.14 → google.com` |
| TXT | Data teks (SPF, DKIM, verifikasi) | `v=spf1 include:_spf.google.com ~all` |
| SOA | Start of Authority – info zona DNS | `Serial, refresh, retry, expire` |

### B. Proses Resolusi DNS
| Langkah | Proses | Keterangan |
|---------|--------|------------|
| 1 | User ketik `google.com` di browser | Client ingin tahu IP google.com |
| 2 | Cek DNS Cache lokal | Apakah sudah pernah diquery sebelumnya? |
| 3 | Query ke Recursive Resolver (ISP/Google) | `8.8.8.8` atau DNS ISP |
| 4 | Recursive Resolver tanya Root Server | Root server tahu NS untuk `.com` |
| 5 | Tanya TLD Server (`.com`) | Arahkan ke authoritative NS google.com |
| 6 | Tanya Authoritative NS Google | Balas: `google.com = 142.250.190.14` |
| 7 | IP dikembalikan ke client & di-cache | Browser buka `142.250.190.14` |

### C. DNS Hierarchy
`Root DNS → TLD (.com, .id, .org) → Authoritative DNS → Client`

⚠️ DNS menggunakan UDP port 53 untuk query biasa. TCP port 53 untuk Zone Transfer (antar server DNS).

### D. Serangan terkait DNS
| Serangan | Penjelasan |
|----------|------------|
| DNS Spoofing / Cache Poisoning | Memasukkan record palsu ke cache DNS agar redirect ke IP jahat |
| DNS Hijacking | Mengubah setting DNS di router/komputer korban |
| DNS Amplification (DDoS) | Menggunakan DNS untuk memperbesar serangan DDoS |
| DNS Tunneling | Menyembunyikan data lain dalam query DNS untuk bypass firewall |

---

## 7. VLAN – Virtual Local Area Network
🔀 VLAN = memisahkan jaringan secara LOGIS dalam satu switch fisik. Tanpa VLAN, semua port dalam satu broadcast domain.

### A. Manfaat VLAN
| Manfaat | Penjelasan |
|---------|------------|
| Keamanan | Departemen berbeda tidak bisa saling akses (misal: Finance vs IT) |
| Kurangi Broadcast | Broadcast hanya ke VLAN yang sama, tidak ke semua port |
| Efisiensi | Satu switch fisik bisa jadi beberapa jaringan logis |
| Fleksibilitas | Pindah user ke VLAN berbeda tanpa ganti kabel fisik |
| QoS | Bisa prioritaskan traffic berdasarkan VLAN (VoIP dll) |

### B. Contoh Implementasi VLAN
| VLAN ID | Nama | Departemen | IP Network |
|---------|------|------------|--------------|
| 10 | Finance | Keuangan – akses terbatas | `192.168.10.0/24` |
| 20 | HR | HRD – akses data karyawan | `192.168.20.0/24` |
| 30 | IT | IT – akses penuh | `192.168.30.0/24` |
| 40 | Guest | Tamu – internet only | `192.168.40.0/24` |
| 99 | Management | Network admin only | `192.168.99.0/24` |

### C. Jenis Port VLAN
| Jenis Port | Keterangan | Digunakan Untuk |
|--------------|------------|-------------------|
| Access Port | Terhubung ke 1 VLAN saja. Tag VLAN dilepas saat keluar. | PC, printer, server biasa |
| Trunk Port | Membawa traffic BANYAK VLAN sekaligus. Menggunakan 802.1Q tagging. | Antar switch, switch ke router |
| Hybrid Port | Kombinasi access dan trunk (di beberapa vendor) | Khusus konfigurasi tertentu |

### D. VLAN Tagging – IEEE 802.1Q
802.1Q menambahkan 4 byte tag ke frame Ethernet untuk identifikasi VLAN:

| Field | Ukuran | Keterangan |
|-------|--------|------------|
| TPID (Tag Protocol ID) | 16 bit | Nilai: `0x8100` (identifikasi frame ber-tag) |
| PCP (Priority Code Point) | 3 bit | QoS priority (0-7) |
| DEI (Drop Eligible) | 1 bit | Tanda paket boleh dibuang saat congesti |
| VID (VLAN ID) | 12 bit | ID VLAN (range: 1–4094, jadi maks 4094 VLAN) |

⚠️ Native VLAN = VLAN yang tidak diberi tag di trunk port. Default native VLAN Cisco = VLAN 1. Sebaiknya diubah untuk keamanan!

### E. Inter-VLAN Routing
Untuk komunikasi ANTAR VLAN, dibutuhkan router atau Layer 3 switch (multilayer switch).

| Metode | Keterangan | Keuntungan |
|--------|------------|--------------|
| Router-on-a-Stick | 1 interface router dengan sub-interface per VLAN (trunk) | Murah, 1 kabel ke router |
| Layer 3 Switch | Switch bisa routing antar VLAN (SVI – Switched Virtual Interface) | Lebih cepat, scalable |
| Dedicated Router Interface | Setiap VLAN dapat interface fisik sendiri | Jarang dipakai, mahal |

---

## 8. ROUTING
🗺️ Routing = proses memilih jalur terbaik untuk mengirim paket antar network. Bekerja di Layer 3 (Network). Dilakukan oleh ROUTER.

### A. Jenis Routing
| Jenis | Cara Kerja | Keuntungan | Kelemahan | Cocok Untuk |
|-------|------------|----------|------------|--------------|
| Static Routing | Admin input route manual | Simpel, aman, tidak ada overhead | Tidak dinamis, mahal untuk jaringan besar | Jaringan kecil, stub network |
| Dynamic Routing | Router belajar otomatis via protokol | Adaptif, scalable, otomatis | Overhead CPU & bandwidth, lebih kompleks | Jaringan besar & kompleks |
| Default Route | Route `0.0.0.0/0` – ke mana saja yang tidak diketahui | Sederhana untuk akses internet | Tidak spesifik | Edge router / gateway ke ISP |

### B. Protokol Dynamic Routing
| Protokol | Tipe | Algoritma | AD* | Metric | Skala |
|----------|------|----------|-----|--------|-------|
| RIP v2 | Distance Vector | Bellman-Ford | 120 | Hop count (max 15) | Kecil |
| OSPF | Link State | Dijkstra (SPF) | 110 | Cost (bandwidth) | Besar |
| EIGRP | Hybrid (Cisco) | DUAL | 90 | Bandwidth + Delay | Sedang-Besar |
| BGP | Path Vector | Best Path | 20 (eBGP) | AS Path, policy | Internet (ISP) |
| IS-IS | Link State | Dijkstra | 115 | Cost | Besar (ISP) |

*AD = Administrative Distance – nilai kepercayaan route. Semakin KECIL = semakin DIPERCAYA.

### C. Administrative Distance (Tabel Lengkap)
| Sumber Route | AD | Artinya |
|---------------|----|---------|
| Connected (langsung terhubung) | 0 | Paling dipercaya |
| Static Route | 1 | Sangat dipercaya |
| EIGRP Summary | 5 | - |
| eBGP (external BGP) | 20 | - |
| EIGRP (internal) | 90 | - |
| IGRP | 100 | - |
| OSPF | 110 | - |
| IS-IS | 115 | - |
| RIP | 120 | - |
| iBGP (internal BGP) | 200 | Kurang dipercaya |
| Unknown / tidak terpercaya | 255 | Route diabaikan |

### D. Contoh Static Route (Cisco IOS)
Sintaks: `ip route [network tujuan] [subnet mask] [next-hop / exit interface]`

| Perintah | Keterangan |
|----------|------------|
| `ip route 192.168.2.0 255.255.255.0 192.168.1.1` | Rute ke network `192.168.2.0/24` via gateway `192.168.1.1` |
| `ip route 0.0.0.0 0.0.0.0 203.1.1.1` | Default route – semua traffic ke `203.1.1.1` (ISP) |
| `ip route 10.0.0.0 255.0.0.0 GigabitEthernet0/1` | Rute ke `10.0.0.0/8` via interface Gi0/1 |

---

## 9. JEBAKAN & RINGKASAN SOAL UJIAN

### A. Jebakan Paling Sering di Soal
| No | Jebakan | Yang Benar | Ingat! |
|----|---------|------------|--------|
| 1 | Host count = Total IP | Host valid = Total IP − 2 | Kurangi 2 (network & broadcast) |
| 2 | /28 punya banyak host | Host valid /28 = hanya 14 | `2⁴−2 = 14` |
| 3 | Broadcast bisa dipakai host | Broadcast = IP terakhir, TIDAK bisa dipakai | `192.168.1.255` bukan host |
| 4 | NAT = hanya Static NAT | NAT paling umum = PAT (overload) | PAT = banyak IP → 1 IP public |
| 5 | UDP lebih aman dari TCP | TCP lebih andal, UDP lebih cepat | Keamanan ≠ kecepatan |
| 6 | DNS pakai TCP saja | DNS pakai UDP (query) & TCP (zone transfer) | Port 53 dua-duanya |
| 7 | SMTP untuk terima email | SMTP untuk KIRIM, POP3/IMAP untuk TERIMA | `25=kirim, 110/143=terima` |
| 8 | Switch = Layer 3 | Switch (biasa) = Layer 2, L3 Switch = Layer 3 | Router yang Layer 3 |
| 9 | VLAN butuh banyak switch | VLAN bisa di 1 switch fisik | Logis, bukan fisik |
| 10 | Telnet aman untuk remote | Telnet TIDAK terenkripsi, pakai SSH | Port 22=SSH (aman), 23=Telnet |

### B. Kata Kunci → Layer OSI (Trik Cepat)
| Kata Kunci / Gejala | Layer | Alasan |
|-----------------------|-------|--------|
| Kabel putus, sinyal mati, NIC, hub | 1 – Physical | Fisik = layer 1 |
| MAC address, switch, ARP, VLAN, frame | 2 – Data Link | MAC = layer 2 |
| IP address, router, ping (ICMP), gateway, subnet | 3 – Network | IP routing = layer 3 |
| Port number, TCP, UDP, handshake, segment | 4 – Transport | Port = layer 4 |
| Login session, checkpoint, dialog control | 5 – Session | Session = layer 5 |
| Enkripsi, SSL/TLS, encoding, JPEG, MP3 | 6 – Presentation | Format data = layer 6 |
| HTTP, DNS, FTP, email, aplikasi error | 7 – Application | Aplikasi user = layer 7 |

### C. Ringkasan Port Wajib Hafal
| Port | Service | Proto | Port | Service | Proto |
|------|---------|-------|------|---------|-------|
| 20 | FTP Data | TCP | 110 | POP3 | TCP |
| 21 | FTP Control | TCP | 143 | IMAP | TCP |
| 22 | SSH | TCP | 161 | SNMP | UDP |
| 23 | Telnet | TCP | 443 | HTTPS | TCP |
| 25 | SMTP | TCP | 445 | SMB (File Sharing) | TCP |
| 53 | DNS | UDP/TCP | 3306 | MySQL | TCP |
| 67 | DHCP Server | UDP | 3389 | RDP | TCP |
| 68 | DHCP Client | UDP | 5432 | PostgreSQL | TCP |
| 80 | HTTP | TCP | 8080 | HTTP Alternatif | TCP |
| 123 | NTP | UDP | 8443 | HTTPS Alternatif | TCP |

### D. Soal Angka – Subnetting Cepat
⚡ Hafalkan host valid per CIDR: `/30=2, /29=6, /28=14, /27=30, /26=62, /25=126, /24=254`

| CIDR | /30 | /29 | /28 | /27 | /26 | /25 | /24 |
|------|-----|-----|-----|-----|-----|-----|-----|
| Host Valid | 2 | 6 |14 |30 |62 |126 |254 |
| Total IP |4 |8 |16 |32 |64 |128 |256 |
| Block Size |4 |8 |16 |32 |64 |128 |256 |

---

© Modul Jaringan Komputer Lengkap | Persiapan SKB TIK & Ujian Jaringan