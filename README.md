# NetworkDefenseProject

Repositori ini merupakan implementasi lengkap Tugas Semester mata kuliah **Network Defense**, yang berfokus pada pembangunan sistem pertahanan jaringan menggunakan *honeypot*, SIEM (Wazuh), IDS/IPS (Suricata/Snort), serta analisis traffic jaringan (Wireshark & PCAP).

Seluruh skenario melibatkan simulasi serangan nyata (Scanning, Brute Force, DDoS) dari mesin attacker (Kali Linux) menuju target honeypot, kemudian dimonitor melalui WatchDog SIEM stack (Wazuh Indexer, Manager, Dashboard).

Dokumentasi penuh uji serangan, instalasi komponen, rules, dan topologi dapat diakses pada halaman Notion Network Defense.

---

## 1. Tujuan Tugas

Berdasarkan dokumen **Tugas Semester**  
:contentReference[oaicite:2]{index=2}:

- Membangun sistem deteksi serangan siber real-world.
- Menggunakan **honeypot** sebagai target penyerangan.
- Menggunakan **Wazuh SIEM** untuk monitoring dan alerting.
- Menggunakan **Wireshark** untuk analisis traffic jaringan.
- Melakukan simulasi 3 jenis serangan umum:
  1. **Scanning (Reconnaissance)**
  2. **Brute Force Attack (SSH)**
  3. **DDoS Attack (ICMP/SYN Flood)**

---

## 2. Arsitektur Sistem

Mengacu pada halaman **Memahami Topologi Tugas**  
:contentReference[oaicite:3]{index=3}:

### Komponen Utama

| Komponen | Fungsi |
|---------|--------|
| **Kali Linux** | Mesin attacker (Nmap, Hydra, hping3). |
| **Honeypot (Ubuntu)** | Menjalankan Cowrie & Suricata sebagai target serangan. |
| **WatchDog Server (Wazuh SIEM)** | Menyimpan, menganalisis, dan menampilkan alert. |
| **Wireshark** | Analisis traffic (PCAP). |

### Alur Kerja

1. Attacker menjalankan scanning / brute force / flood.
2. Honeypot (Cowrie + Suricata) merekam aktivitas.
3. Wazuh Agent mengirim log ke Wazuh Manager.
4. Wazuh Dashboard menampilkan alert.
5. PCAP dikumpulkan untuk analisis forensik.

---

## 3. Instalasi & Implementasi

Seluruh langkah implementasi disusun dari halaman **Implementasi**  
:contentReference[oaicite:4]{index=4}.

### 3.1 Instalasi VMware & VM Environment
Dokumen:
- VMware Fusion (macOS):contentReference[oaicite:5]{index=5}
- Instalasi Kali Linux:contentReference[oaicite:6]{index=6}
- Instalasi Ubuntu Server/Ubuntu Desktop:contentReference[oaicite:7]{index=7}:contentReference[oaicite:8]{index=8}

VM yang dibuat:
- **Kali** attacker
- **Ubuntu Honeypot** (192.168.1.x)
- **WatchDog Server** (Wazuh stack)

### 3.2 Instalasi Honeypot
Dokumen:
- **Cowrie Honeypot**:contentReference[oaicite:9]{index=9}
- **Dionaea Honeypot**:contentReference[oaicite:10]{index=10}

Cowrie merekam:
- Login attempt (success/failed)
- Command execution
- File download attempts

Dionaea memerangkap malware.

### 3.3 Instalasi IDS/IPS
Dokumen:
- Instalasi Suricata:contentReference[oaicite:11]{index=11}
- Instalasi Snort:contentReference[oaicite:12]{index=12}

Suricata dipilih sebagai IDS utama karena log **JSON → kompatibel dengan Wazuh**.

### 3.4 Instalasi Wazuh SIEM
Dokumen: Instalasi Wazuh:contentReference[oaicite:13]{index=13}

Komponen:
- Wazuh Indexer (Elasticsearch fork)
- Wazuh Manager
- Wazuh Dashboard
- Wazuh Agent (pada honeypot)

### 3.5 Custom Ruleset
Dokumen: **Rules**  
:contentReference[oaicite:14]{index=14}

Di dalamnya terdapat:
- Cowrie login failed/success
- Suricata alerts mapping (ET SCAN, ET DOS, ET POLICY)
- Snort compatibility mappings

---

## 4. Skenario Serangan

Disusun dari dokumen:
- **Preparasi Skenario Serangan**:contentReference[oaicite:15]{index=15}
- **Uji Skenario Serangan** (dua versi)  
  :contentReference[oaicite:16]{index=16}  
  :contentReference[oaicite:17]{index=17}

### 4.1 Port Scanning (Reconnaissance)
Tools: **Nmap**

Perintah:
```bash
nmap -sS -p- <target>
nmap -A <target>
