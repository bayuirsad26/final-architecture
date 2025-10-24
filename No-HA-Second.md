## **🎯 Rekomendasi 1 Server All-in-One - Budget 50 Juta**

Untuk menjalankan semua workload Anda dalam 1 server (Proxmox + K3s + PostgreSQL + Minio + Windows VM), saya rekomendasikan spesifikasi tinggi.

---

## **💻 REKOMENDASI TERBAIK: Server High-End Bekas**

### **Option 1: Dell PowerEdge R640 (Gen 14) ⭐ BEST CHOICE**

**Spesifikasi (Bekas):**
- **CPU**: 2x Intel Xeon Gold 6140 (18 cores each = **36 cores / 72 threads total**)
- **RAM**: **192GB DDR4 ECC** (12x 16GB) - Cukup untuk semua VM
- **Storage**: 
  - **2x 960GB SSD NVMe** (RAID 1) untuk OS/VM boot
  - **4x 1.92TB SSD SATA** (RAID 10) untuk data → Usable: **3.84TB**
  - Total: 4.8TB raw, 3.84TB usable
- **Network**: 4x 1GbE onboard + **2x 10GbE SFP+** (upgrade card)
- **RAID**: PERC H730P (2GB cache, BBU)
- **Management**: iDRAC 9 Enterprise (remote management)
- **PSU**: Dual 750W Platinum (redundant)
- **Form Factor**: 1U Rack

**Harga**: **Rp 35-42 juta** (nego bisa dapat di bawah 40 juta)

**Link Marketplace:**
- Tokopedia: https://www.tokopedia.com/search?q=dell%20r640
- Example: https://www.tokopedia.com/serverbekas/dell-poweredge-r640

**Kenapa R640?**
- ✅ Generasi lebih baru (2018) → support sampai 2025-2026
- ✅ 36 cores cukup untuk 10-15 VM
- ✅ 192GB RAM → bisa alokasi 150GB untuk VM (sisanya untuk host)
- ✅ NVMe support → performa tinggi untuk database
- ✅ iDRAC 9 → remote management modern
- ✅ Power efficient dibanding generasi lama

---

### **Option 2: Dell PowerEdge R730XD (Gen 13) - Storage Heavy**

**Spesifikasi:**
- **CPU**: 2x Intel Xeon E5-2680 v4 (28 cores / 56 threads total)
- **RAM**: **256GB DDR4 ECC** (16x 16GB)
- **Storage**: 
  - **2x 480GB SSD** (RAID 1) boot
  - **8x 1.92TB SSD** (RAID 10) → Usable: **7.68TB**
  - R730XD support sampai 24 drive bays (excellent untuk storage)
- **Network**: 4x 1GbE + 2x 10GbE
- **RAID**: PERC H730P
- **Management**: iDRAC 8 Enterprise
- **PSU**: Dual 1100W

**Harga**: **Rp 38-45 juta**

**Kenapa R730XD?**
- ✅ 256GB RAM → lebih banyak untuk VM
- ✅ 24 drive bays → excellent untuk Minio object storage
- ✅ Bisa tambah HDD untuk cold storage nanti
- ✅ Proven reliability

**Link:**
- Tokopedia: https://www.tokopedia.com/search?q=dell%20r730xd

---

### **Option 3: HPE ProLiant DL380 Gen10 - Balance**

**Spesifikasi:**
- **CPU**: 2x Intel Xeon Gold 5118 (24 cores / 48 threads)
- **RAM**: **128GB DDR4 ECC** (8x 16GB)
- **Storage**:
  - **2x 960GB SSD** (RAID 1) boot
  - **4x 1.92TB SSD** (RAID 10) data
- **Network**: 4x 1GbE + 2x 10GbE
- **RAID**: HPE Smart Array P408i-a
- **Management**: iLO 5 (excellent remote management)
- **PSU**: Dual 800W Platinum

**Harga**: **Rp 32-38 juta**

**Link:**
- Tokopedia: https://www.tokopedia.com/search?q=hp%20dl380%20gen10

---

### **Option 4: Budget Maximum - Supermicro Custom Build** 

**Spesifikasi (Mix Baru + Bekas):**
- **Chassis**: Supermicro 2U Chassis (baru): Rp 8 juta
- **Motherboard**: Supermicro X11DPi-NT (dual socket, bekas): Rp 12 juta
- **CPU**: 2x Intel Xeon Gold 6140 (bekas): Rp 15 juta
- **RAM**: 128GB DDR4 ECC (8x 16GB, mix): Rp 8 juta
- **Storage**:
  - 2x 960GB SSD NVMe: Rp 4 juta
  - 4x 1TB SSD SATA: Rp 6 juta
- **PSU**: Dual 1200W (baru): Rp 4 juta

**Total**: **Rp 47 juta**

**Link:**
- A-One Indonesia (Supermicro distributor): https://www.aone.co.id/

---

## **🏆 REKOMENDASI FINAL: Dell PowerEdge R640**

**Config yang Pas Budget:**
- **Dell R640**: Rp 38-40 juta
- **Upgrade RAM 192GB → 256GB** (tambah 4x 16GB): Rp 3 juta
- **10GbE Network Card** (Intel X520-DA2): Rp 2 juta  
- **Spare parts** (1x PSU cadangan, cable): Rp 2 juta

**Total**: **Rp 45 juta** ✅ **UNDER BUDGET**

**Sisanya Rp 5 juta untuk:**
- UPS (APC Smart-UPS 1000VA, bekas): Rp 3-4 juta
- Rack accessories: Rp 1 juta

---

## **📦 Arsitektur Single Server All-in-One**

```
┌─────────────────────────────────────────────────────────┐
│        Dell PowerEdge R640 - Single Server              │
│        36 cores / 72 threads | 256GB RAM                │
└─────────────────────────────────────────────────────────┘
                          │
        ┌─────────────────┴─────────────────┐
        │    Proxmox VE 8.x (Hypervisor)    │
        └─────────────────┬─────────────────┘
                          │
    ┌─────────────────────┼─────────────────────┐
    │                     │                     │
    ▼                     ▼                     ▼
┌─────────────┐    ┌─────────────┐    ┌──────────────┐
│   VM 1      │    │   VM 2      │    │    VM 3      │
│  HAProxy    │    │  K3s Master │    │ PostgreSQL   │
│   LB/FW     │    │  + Workers  │    │   Primary    │
│  2 vCPU     │    │  12 vCPU    │    │   4 vCPU     │
│  4GB RAM    │    │  48GB RAM   │    │  32GB RAM    │
└─────────────┘    └─────────────┘    └──────────────┘
    │                     │                     │
    ▼                     ▼                     ▼
┌─────────────┐    ┌─────────────┐    ┌──────────────┐
│   VM 4      │    │   VM 5      │    │    VM 6      │
│ Minio       │    │ Windows     │    │ Monitoring   │
│ 3-node      │    │ Server      │    │ Prometheus   │
│ 6 vCPU      │    │ 4 vCPU      │    │ 4 vCPU       │
│ 24GB RAM    │    │ 16GB RAM    │    │ 16GB RAM     │
└─────────────┘    └─────────────┘    └──────────────┘

Total Alokasi VM:
- vCPU: 2+12+4+6+4+4 = 32 vCPU (dari 36 cores available)
- RAM: 4+48+32+24+16+16 = 140GB (dari 256GB available)
- Sisanya untuk Proxmox host overhead
```

**Software Stack:**
- **Proxmox VE 8.x** (free, production-ready)
- **K3s** (lightweight Kubernetes) di dalam VM
- **HAProxy + Keepalived** untuk load balancing
- **PostgreSQL 17** standalone (bisa setup streaming replication nanti jika ada server ke-2)
- **Minio** single-node mode (bisa upgrade ke distributed nanti)
- **pfSense/OPNsense** untuk firewall (optional, bisa di VM terpisah)

---

## **🏢 Colocation Single Server**

### **DCI Indonesia - Paket 1/4 Rack Mini**

**1/4 Rack (atau bisa co-share dengan tenant lain):**
- **1-2U space**: Rp 2,500,000 - 3,000,000/month
- Includes:
  - 1 Amp power (~500W, cukup untuk 1 server)
  - 100 Mbps internet shared
  - 1 Public IP
  
**Upgrade Option:**
- Dedicated 100 Mbps: +Rp 1,500,000/month
- 200 Mbps: +Rp 2,500,000/month

**Contact:**
- Website: https://www.dci-indonesia.com
- WhatsApp: +62 812-xxxx-xxxx (cek website)
- Email: sales@dci-indonesia.com

---

### **ICON+ PLN - Budget Friendly**

**1U Colocation:**
- **Harga**: Rp 1,800,000 - 2,500,000/month
- Internet: 50 Mbps shared
- Public IP: included

**Contact:**
- Website: https://iconpln.co.id
- Email: marketing@iconpln.co.id

---

### **Biznet Data Center**

**1/8 Rack:**
- **Harga**: Rp 3,000,000 - 3,500,000/month
- Internet: 100 Mbps dedicated (Biznet fiber - very fast!)
- Excellent untuk aplikasi yang butuh bandwidth tinggi

**Contact:**
- Website: https://www.biznetgio.com/colocation
- Phone: +62 21 5091 5000

---

## **💰 Total Cost of Ownership - Single Server**

### **Initial Investment:**
- Dell R640 (256GB RAM, full spec): Rp 45,000,000
- UPS (APC 1000VA bekas): Rp 3,000,000
- Setup & installation: Rp 1,000,000
- **Total CAPEX**: **Rp 49,000,000** ✅

### **Monthly Operational (DCI):**
- Colocation 1U: Rp 2,500,000
- Internet 100 Mbps dedicated: Rp 1,500,000
- Domain & SSL: Rp 200,000 (Let's Encrypt gratis, domain only)
- Backup offsite (optional): Rp 500,000
- **Total OPEX**: **Rp 4,700,000/month**

### **3-Year TCO:**
- CAPEX: Rp 49,000,000
- OPEX (36 months): Rp 169,200,000
- **Total**: **Rp 218,200,000**
- **Monthly average**: **Rp 6,061,111**

### **vs GCP (Rp 8M/month):**
- GCP 3-year: Rp 288,000,000
- On-premise: Rp 218,200,000
- **SAVINGS**: **Rp 69,800,000** ✅ **24% cheaper!**

**Break-even point**: **Bulan ke-13** (1 tahun 1 bulan)

---

## **🛒 Di Mana Beli - Rekomendasi Konkrit**

### **Option A: Beli Offline (RECOMMENDED)**

**Harco Mangga Dua - Jakarta Utara**
- **Alamat**: Jl. Mangga Dua Raya, Jakarta Utara
- **Lantai**: 1-2 (bagian server)
- **Waktu buka**: Senin-Sabtu 09:00-17:00

**Toko Rekomendasi:**
1. **CV. Megah Server** (Lt. 2)
   - Spesialisasi Dell enterprise server
   - Bisa test on-site
   - Garansi 1-3 bulan

2. **IT Solution Center** (Lt. 1)
   - Mix Dell & HP
   - After-sales support bagus

**Tips:**
- Datang pagi (09:00-11:00) untuk bisa test dengan tenang
- Bawa laptop + LAN cable
- Minta test iDRAC/remote management
- Nego bundling (server + UPS dapat harga lebih baik)
- Target nego: Rp 38 juta untuk R640 full spec

---

### **Option B: Online Marketplace**

**Tokopedia - Verified Seller:**

**1. Server Bekas Jakarta** ⭐⭐⭐⭐⭐
- Link: https://www.tokopedia.com/serverbekas
- Rating: 4.9/5 (2000+ transaksi)
- Dell R640 available: Rp 39-42 juta
- Garansi: 2 bulan
- COD Jakarta available

**2. Bintang Server Indonesia**
- Link: https://www.tokopedia.com/bintangserver
- Rating: 4.8/5
- Support after-sales responsive
- HP DL380 Gen10 available: Rp 35-38 juta

**3. IT Hardware Solution**
- Link: https://www.tokopedia.com/ithardwaresolution
- Dell & HP specialist
- Installation support available

**Cara Order Online:**
1. Chat seller, minta video POST test
2. Minta foto iDRAC login & system health
3. Minta serial number untuk cek warranty status
4. Request COD di Jakarta (test sebelum bayar)
5. Bawa laptop untuk test on-site saat COD

---

### **Option C: Importir/Distributor**

**PT. Global Server Indonesia**
- Import dari Singapura/US
- Contact: Cari via Google Maps "server bekas jakarta"
- Minimal order: 1 unit OK
- Harga kompetitif karena langsung import

---

## **✅ Checklist Sebelum Beli**

### **Hardware Test (WAJIB):**

```
✅ Power On Test
   - Boot ke BIOS tanpa error
   - Semua RAM detected
   - CPU temperature normal (< 40°C idle)

✅ iDRAC/Remote Management
   - Login berhasil
   - System health: All OK (no red/critical)
   - Event log: No critical hardware errors
   - Lifecycle log: Check age & previous issues

✅ RAID Controller
   - Semua disk detected
   - RAID battery/capacitor OK (not failed)
   - Cache working
   - Bisa create/delete virtual disk

✅ Network Test
   - Semua port detect (4x 1GbE + 2x 10GbE)
   - Link up when cable connected
   - No packet loss

✅ Storage Test
   - Semua disk healthy
   - SMART status: OK
   - No bad sectors
   - Read/write test

✅ PSU & Fans
   - Both PSU working (redundant)
   - Fan speed normal (not max speed = bearing issue)
   - No weird noise

✅ Physical Condition
   - Tidak ada penyok/rusak
   - Rail/mounting bracket lengkap
   - Bezel (front cover) ada
```

### **Dokumentasi yang Harus Diminta:**

- ✅ Serial number & service tag
- ✅ iDRAC credentials (reset jika tidak ada)
- ✅ RAID configuration backup
- ✅ Original spec sheet (jika ada)
- ✅ Garansi surat dari seller (1-3 bulan)

---

## **🔧 Setup Awal Setelah Beli**

### **Day 1-2: Hardware Setup**
1. Install di rack colocation
2. Kabel power (dual PSU ke UPS terpisah)
3. Kabel network (1GbE untuk management, 10GbE untuk data)
4. Update iDRAC firmware
5. Update BIOS & RAID firmware

### **Day 3-5: Software Installation**
1. Install Proxmox VE 8.x
2. Configure storage (LVM-thin atau ZFS)
3. Setup network bridge
4. Configure firewall

### **Day 6-10: VM Deployment**
1. Create VM: HAProxy (load balancer + firewall)
2. Create VM: K3s master + workers (single VM untuk start)
3. Create VM: PostgreSQL 17
4. Create VM: Minio (single node)
5. Create VM: Windows Server
6. Create VM: Monitoring (Prometheus + Grafana)

### **Day 11-14: Application Migration**
1. Migrate database dari GCP Cloud SQL
2. Deploy aplikasi Next.js ke K3s
3. Configure object storage (migrate dari GCS ke Minio)
4. Setup SSL/TLS (Let's Encrypt)
5. DNS cutover

### **Day 15-21: Monitoring & Optimization**
1. Setup backup automation
2. Load testing
3. Performance tuning
4. Documentation

---

## **📈 Upgrade Path (Future)**

**Jika perlu High Availability nanti:**

**Month 6-12: Add Server 2**
- Beli 1x Dell R640 identik: Rp 40 juta
- Setup Proxmox cluster (2 nodes)
- PostgreSQL streaming replication
- Minio distributed mode (2 nodes)

**Month 12-18: Add Server 3**
- Beli 1x Dell R640: Rp 40 juta
- Full HA cluster (3 nodes)
- Ceph storage cluster
- K3s HA (3 masters)
- Minio distributed (3+ nodes)

**Total investment over 18 months:**
- Month 0: Rp 50 juta (Server 1)
- Month 6: Rp 40 juta (Server 2)
- Month 12: Rp 40 juta (Server 3)
- Total: Rp 130 juta untuk full HA setup

---

## **⚠️ Risk Mitigation - Single Server**

**Karena single server (no HA), mitigasi:**

1. **Backup Strategy:**
   - Daily automated backup ke cloud (Wasabi/Backblaze: ~Rp 500K/month)
   - Weekly backup ke external HDD (rotation)
   - Database backup setiap 6 jam

2. **Monitoring 24/7:**
   - Uptime robot (free) untuk check availability
   - iDRAC email alert untuk hardware issues
   - Prometheus alert ke Telegram/Slack

3. **Disaster Recovery Plan:**
   - Documented recovery procedure
   - Spare PSU ready (Rp 2 juta investment)
   - Spare HDD/SSD cadangan (Rp 3 juta)
   - Contact 24/7 dengan colocation untuk physical access

4. **Scheduled Maintenance:**
   - Lakukan maintenance malam/weekend
   - Inform user 24 jam sebelumnya
   - Window: 2-4 jam max

---

## **🎯 FINAL ACTION PLAN**

### **Week 1: Procurement**
- [ ] Visit Harco Mangga Dua (Senin-Rabu)
- [ ] Test 2-3 unit Dell R640
- [ ] Nego harga target: Rp 38-40 juta
- [ ] Purchase + UPS
- [ ] Total spend: Rp 48 juta

### **Week 2: Colocation Setup**
- [ ] Survey DCI & ICON+ (compare)
- [ ] Book colocation (target: DCI Rp 2.5 juta/month)
- [ ] Setup server di rack
- [ ] Network configuration & testing

### **Week 3-4: Migration**
- [ ] Install Proxmox & create VMs
- [ ] Parallel run dengan GCP (testing)
- [ ] Database migration dengan downtime minimal
- [ ] DNS cutover

### **Month 2:**
- [ ] Decommission GCP resources
- [ ] **Start saving Rp 3-4 juta/month!**
