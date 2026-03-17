# 📚 MODUL LENGKAP DATABASE & SQL
## Persiapan Ujian SKB TIK, CPNS, dan Ujian Database

---

### 🎯 Daftar Isi
1. [Konsep Dasar Database & DBMS](#1-konsep-dasar-database--dbms)
2. [Model Data & ERD (Entity Relationship Diagram)](#2-model-data--erd-entity-relationship-diagram)
3. [Normalisasi Database](#3-normalisasi-database)
4. [DDL & DML](#4-ddl--dml)
5. [SELECT & Filtering Data](#5-select--filtering-data)
6. [JOIN antar Tabel](#6-join-antar-tabel)
7. [Agregasi & GROUP BY](#7-agregasi--group-by)
8. [Subquery & View](#8-subquery--view)
9. [Indexing](#9-indexing)
10. [Transaksi & ACID](#10-transaksi--acid)
11. [Ringkasan & Jebakan Soal Ujian](#11-ringkasan--jebakan-soal-ujian)

---

## 1. Konsep Dasar Database & DBMS

### 1.1 Istilah Penting
| Istilah | Definisi | Contoh |
|---------|----------|--------|
| **Database** | Kumpulan data yang terorganisir secara sistematis | Data pelanggan, produk, transaksi toko |
| **DBMS** | Software untuk mengelola database | MySQL, PostgreSQL, Oracle, SQL Server |
| **Tabel/Relasi** | Struktur penyimpanan data dalam baris dan kolom | Tabel pelanggan dengan kolom id, nama, email |
| **Primary Key (PK)** | Kolom unik sebagai identitas tiap baris | `id_pelanggan`, `id_produk` |
| **Foreign Key (FK)** | Kolom yang merujuk PK tabel lain | `id_pelanggan` di tabel pesanan → PK tabel pelanggan |
| **SQL** | Bahasa standar untuk RDBMS | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |

### 1.2 Jenis-Jenis Database
| Jenis | Model | Contoh DBMS | Keterangan |
|-------|-------|-------------|------------|
| **Relasional (RDBMS)** | Tabel dengan relasi | MySQL, PostgreSQL | PALING UMUM untuk ujian SKB |
| **Dokumen** | JSON/BSON | MongoDB, CouchDB | NoSQL fleksibel |
| **Key-Value** | Pasangan kunci-nilai | Redis, DynamoDB | Cache dan session |
| **Graf** | Node & Edge | Neo4j, Amazon Neptune | Jaringan sosial dan rekomendasi |

### 1.3 Arsitektur ANSI/SPARC
3 level arsitektur database:
1. **External Level**: Tampilan data untuk pengguna akhir
2. **Conceptual Level**: Struktur logis keseluruhan database
3. **Internal Level**: Cara data disimpan secara fisik di disk

---

## 2. Model Data & ERD

### 2.1 Komponen ERD
| Komponen | Simbol | Keterangan |
|----------|--------|------------|
| **Entity** | Persegi panjang | Objek dengan data |
| **Atribut PK** | Oval bergaris bawah | Kolom unik identitas entity |
| **Relasi** | Belah ketupat | Hubungan antar entity |

### 2.2 Kardinalitas Relasi
| Tipe | Notasi | Contoh Nyata |
|------|--------|--------------|
| **1:1** | 1 — 1 | 1 Pegawai → 1 KTP |
| **1:N** | 1 — N | 1 Pelanggan → Banyak Pesanan |
| **M:N** | M — N | Banyak Pesanan → Banyak Produk |

---

## 3. Normalisasi Database

### 3.1 Tujuan Normalisasi
Menghilangkan redundansi dan anomali data (insert, update, delete).

### 3.2 Normal Form
| NF | Syarat Utama | Eliminasi |
|----|---------------|-----------|
| **1NF** | Nilai atomic, baris unik | Multi-value dan repeating group |
| **2NF** | 1NF + tidak ada partial dependency | Partial dependency pada composite key |
| **3NF** | 2NF + tidak ada transitive dependency | Transitive dependency |
| **BCNF** | 3NF + setiap determinan adalah superkey | Anomali sisa di 3NF |

---

## 4. DDL & DML

### 4.1 Kategori Perintah SQL
| Kategori | Perintah | Fungsi |
|------------|-----------|--------|
| **DDL** | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` | Mendefinisikan struktur database |
| **DML** | `INSERT`, `UPDATE`, `DELETE`, `SELECT` | Memanipulasi data |
| **TCL** | `COMMIT`, `ROLLBACK`, `SAVEPOINT` | Mengelola transaksi |

### 4.2 Contoh Perintah DDL
```sql
-- Membuat tabel pelanggan
CREATE TABLE pelanggan (
    id_pelanggan INT PRIMARY KEY AUTO_INCREMENT,
    nama VARCHAR(150) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);
```

---

## 5. SELECT & Filtering Data

### 5.1 Anatomi Query SELECT
```sql
SELECT [DISTINCT] kolom1, kolom2
FROM nama_tabel
WHERE kondisi
GROUP BY kolom
HAVING kondisi_agregasi
ORDER BY kolom [ASC|DESC]
LIMIT n [OFFSET m];
```

### 5.2 Operator Filter
- `LIKE`: Pencarian pola (`%` untuk banyak karakter, `_` untuk satu karakter)
- `BETWEEN`: Rentang nilai
- `IN`: Cocok dengan salah satu nilai dalam daftar
- `IS NULL`: Pencarian nilai kosong

---

## 6. JOIN antar Tabel

### 6.1 Jenis-Jenis JOIN
| Jenis JOIN | Hasil |
|-----------|-------|
| **INNER JOIN** | Hanya baris yang cocok di kedua tabel |
| **LEFT JOIN** | Semua baris tabel kiri + cocok dari kanan |
| **RIGHT JOIN** | Semua baris tabel kanan + cocok dari kiri |

### 6.2 Contoh INNER JOIN
```sql
SELECT p.id_pesanan, pl.nama AS pelanggan
FROM pesanan p
INNER JOIN pelanggan pl ON p.id_pelanggan = pl.id_pelanggan;
```

---

## 7. Agregasi & GROUP BY

### 7.1 Fungsi Agregasi
`COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`

### 7.2 Klausa HAVING
Digunakan untuk memfilter hasil agregasi setelah `GROUP BY`:
```sql
SELECT id_kategori, COUNT(*) AS jumlah_produk
FROM produk
GROUP BY id_kategori
HAVING COUNT(*) > 5;
```

---

## 8. Subquery & View

### 8.1 Subquery
Query di dalam query, bisa digunakan di `WHERE`, `FROM`, atau `SELECT`:
```sql
-- Produk dengan harga di atas rata-rata
SELECT nama_produk, harga
FROM produk
WHERE harga > (SELECT AVG(harga) FROM produk);
```

### 8.2 View
Tabel virtual yang menyimpan query:
```sql
-- Membuat view laporan pesanan
CREATE VIEW v_laporan_pesanan AS
SELECT ps.id_pesanan, pl.nama AS pelanggan
FROM pesanan ps
INNER JOIN pelanggan pl ON ps.id_pelanggan = pl.id_pelanggan;
```

---

## 9. Indexing

### 9.1 Cara Kerja Index
Mempercepat pencarian seperti daftar isi buku, mengurangi full table scan.

### 9.2 Jenis Index
- **Primary Index**: Dari Primary Key
- **Unique Index**: Nilai tidak duplikat
- **Composite Index**: Index pada beberapa kolom

---

## 10. Transaksi & ACID

### 10.1 Sifat ACID
1. **Atomicity**: Semua operasi berhasil atau semua dibatalkan
2. **Consistency**: Database selalu dalam state valid
3. **Isolation**: Transaksi tidak saling mengganggu
4. **Durability**: Perubahan permanen setelah COMMIT

### 10.2 Perintah Transaksi
```sql
START TRANSACTION;
UPDATE rekening SET saldo = saldo - 500000 WHERE id_rek = 'A001';
UPDATE rekening SET saldo = saldo + 500000 WHERE id_rek = 'B001';
COMMIT;
```

---

## 11. Ringkasan & Jebakan Soal Ujian

### 11.1 Jebakan Umum
1. `COUNT(*)` vs `COUNT(kolom)`: `COUNT(*)` menghitung semua baris, `COUNT(kolom)` menghitung nilai non-NULL
2. `WHERE` vs `HAVING`: `WHERE` sebelum `GROUP BY`, `HAVING` setelah
3. `LEFT JOIN` menampilkan SEMUA baris tabel kiri, bukan hanya yang cocok

### 11.2 Cheat Sheet
Selalu periksa:
- Apakah foreign key sudah dibuat dengan benar
- Apakah host valid sudah ditambahkan ke database
- Apakah hak akses sudah diatur dengan benar

---

✅ Modul ini disusun berdasarkan materi ujian SKB TIK, CPNS, dan ujian database yang umum. Untuk latihan soal, silakan tambahkan pertanyaan sesuai kebutuhan! 😊