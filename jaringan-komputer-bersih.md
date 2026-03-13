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

## 3. IPv4 PRIVATE vs PUBLIC

### A. Range IP Private (Wajib Hafal!)
| Kelas | Range IP | Subnet Default | Jumlah Host | Penggunaan |
|-------|----------|----------------|-------------|-------------|
| A | `10.0.0.0 – 10.255.255.255` | `255.0.0.0 (/8)` | 16.777.214 | Data center, enterprise besar |
| B | `172.16.0.0 – 172.31.255.255` | `255.255.0.0 (/16)` | 1.048.574 | Perusahaan menengah |
| C | `192.168.0.0 – 192.168.255.255` | `255.255.255.0 (/24)` | 65.534 | Rumah, kantor kecil |

### B. IP Khusus Lainnya
| Range / IP | Nama | Keterangan |
|------------|------|-------------|
| `127.0.0.1` | Loopback / Localhost | Untuk testing di komputer sendiri |
| `127.0.0.0/8` | Loopback Network | Seluruh range loopback |
| `169.254.0.0/16` | APIPA (Link-Local) | Otomatis jika DHCP gagal (Windows) |
| `0.0.0.0` | Unspecified / Any | Mewakili semua interface / default route |
| `255.255.255.255` | Broadcast Global | Broadcast ke semua host di network |
| `224.0.0.0 – 239.x.x.x` | Multicast | OSPF, IGMP, video streaming |

### C. Public IP vs Private IP
| Public IP | Private IP |
|-----------|------------|
| • Unik di seluruh internet | • Hanya unik di jaringan lokal |
| • Bisa diakses dari mana saja | • Tidak bisa diakses langsung dari internet |
| • Ditetapkan ISP | • Butuh NAT untuk ke internet |
| • Contoh: `8.8.8.8`, `1.1.1.1` | • Contoh: `192.168.1.1` |

---

*Sisa konten akan dilanjutkan sesuai kebutuhan*

© Modul Jaringan Komputer Lengkap | Persiapan SKB TIK & Ujian Jaringan