# 📡 CHEAT SHEET JARINGAN KOMPUTER

Model OSI 7 Layer + TCP/IP  |  Referensi Cepat SKB / Ujian Jaringan

---

## 🔢 MODEL OSI – 7 LAYER

Model referensi yang dibuat oleh International Organization for Standardization (ISO). Terdiri dari 7 layer yang menjelaskan bagaimana data berpindah dari satu perangkat ke perangkat lain.

**Mnemonic (Bawah→Atas):** "Please Do Not Throw Sausage Pizza Away"

**Layer 1–7:** Physical → Data Link → Network → Transport → Session → Presentation → Application

| # | Nama Layer | Alias | Protokol / Fungsi | Unit Data |
| :---: | :---- | :---- | :---- | :---: |
| **7** | **Application** | Aplikasi | HTTP/HTTPS, FTP, SMTP, DNS | **Data** |
| **6** | **Presentation** | Presentasi | SSL/TLS, JPEG, MP3, Enkripsi | **Data** |
| **5** | **Session** | Sesi | Session Mgmt, Login, Checkpoint | **Data** |
| **4** | **Transport** | Transportasi | TCP, UDP, Port Number | **Segment** |
| **3** | **Network** | Jaringan | IP, ICMP, Routing, NAT | **Packet** |
| **2** | **Data Link** | Data Link | MAC, ARP, VLAN, Switching | **Frame** |
| **1** | **Physical** | Fisik | Kabel, Sinyal, Tegangan | **Bit** |

---

## 🖥 DEVICE MAPPING PER LAYER

| Perangkat | Layer | Keterangan |
| :---- | :---: | :---- |
| **Hub** | **1 – Physical** | Meneruskan semua sinyal (broadcast) |
| **Switch** | **2 – Data Link** | Meneruskan frame berdasarkan MAC address |
| **Bridge** | **2 – Data Link** | Menghubungkan 2 segmen jaringan |
| **Router** | **3 – Network** | Menentukan jalur terbaik via IP address |
| **Firewall (port-based)** | **4 – Transport** | Filter berdasarkan port number |
| **Next-Gen Firewall (DPI)** | **7 – Application** | Inspeksi paket mendalam (payload) |

---

## 🔁 ENKAPSULASI DATA

| Pengiriman (Layer 7 → 1) | Penerimaan (Layer 1 → 7) |
| :---- | :---- |
| Data → Segment → Packet → Frame → Bit | Bit → Frame → Packet → Segment → Data |

---

## 🔐 SERANGAN BERDASARKAN LAYER

| Serangan | Layer | Penjelasan |
| :---- | :---: | :---- |
| **ARP Spoofing** | **2 – Data Link** | Memalsukan ARP reply untuk mencuri traffic |
| **MAC Flooding** | **2 – Data Link** | Membanjiri switch table agar berperilaku seperti hub |
| **IP Spoofing** | **3 – Network** | Memalsukan source IP address |
| **TCP SYN Flood** | **4 – Transport** | DDoS dengan banjir paket SYN (setengah handshake) |
| **Port Scanning** | **4 – Transport** | Mendeteksi port yang terbuka di target |
| **SQL Injection** | **7 – Application** | Menyisipkan query SQL berbahaya via input form |

---

## 🌍 OSI vs TCP/IP

| # | Layer TCP/IP | Setara OSI | Protokol |
| :---: | :---- | :---- | :---- |
| **4** | **Application** | OSI Layer 5–7 | HTTP, FTP, DNS, SMTP, SSL/TLS |
| **3** | **Transport** | OSI Layer 4 | TCP, UDP, Port Number |
| **2** | **Internet** | OSI Layer 3 | IP, ICMP, ARP, Routing |
| **1** | **Network Interface** | OSI Layer 1–2 | Ethernet, Wi-Fi, MAC, Kabel |

---

## 🎯 TRIK CEPAT JAWAB SOAL SKB

**💡 Kalau soal menyebut kata kunci berikut, langsung identifikasi layernya:**

| Kata Kunci / Gejala | → Jawaban Layer |
| :---- | :---- |
| Kabel putus / sinyal mati | **Layer 1 – Physical** |
| MAC Address, ARP, Switch, VLAN | **Layer 2 – Data Link** |
| IP Address, Gateway salah, Router, Ping (ICMP) | **Layer 3 – Network** |
| Port Number, TCP, UDP, Three-way Handshake | **Layer 4 – Transport** |
| Enkripsi, SSL/TLS, Kompresi, Encoding | **Layer 6 – Presentation** |
| DNS, HTTP, FTP, SMTP, Aplikasi error | **Layer 7 – Application** |

---

## ⚡ PERTANYAAN YANG SERING MUNCUL

| Pertanyaan | ✓ Jawaban |
| :---- | :---- |
| Switch kerja di layer berapa? | **Layer 2 – Data Link** |
| Router kerja di layer berapa? | **Layer 3 – Network** |
| Unit data layer 3? | **Packet** |
| TCP ada di layer? | **Layer 4 – Transport** |
| UDP ada di layer? | **Layer 4 – Transport** |
| ARP ada di layer? | **Layer 2 – Data Link** |
| ICMP ada di layer? | **Layer 3 – Network** |
| DNS ada di layer? | **Layer 7 – Application** |
| Three-way Handshake milik protokol? | **TCP – Layer 4 – Transport** |
| SSL/TLS ada di layer? | **Layer 6 – Presentation** |
