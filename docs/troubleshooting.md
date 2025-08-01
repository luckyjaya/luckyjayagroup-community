# Panduan Troubleshooting

## 🚨 Masalah Umum

### 📱 Aplikasi Mobile Crash

#### Gejala:
- Aplikasi menutup sendiri tiba-tiba
- Force close saat membuka fitur tertentu
- Aplikasi tidak merespon (freeze)

#### Solusi:
1. **Force Stop dan Restart**
   ```
   Android: Settings > Apps > [Nama App] > Force Stop
   iOS: Double tap home button > swipe up pada app
   ```

2. **Clear Cache (Android)**
   ```
   Settings > Apps > [Nama App] > Storage > Clear Cache
   ```

3. **Update Aplikasi**
   - Cek Play Store/App Store untuk update
   - Install versi terbaru

4. **Restart Device**
   - Matikan device selama 30 detik
   - Nyalakan kembali

5. **Reinstall Aplikasi**
   - Backup data jika perlu
   - Uninstall dan install ulang

### 🖥️ Desktop Aplikasi Tidak Bisa Start

#### Gejala:
- Error message saat startup
- Aplikasi loading terus tanpa masuk
- Blue screen atau system crash

#### Solusi:
1. **Run as Administrator**
   - Klik kanan pada icon aplikasi
   - Pilih "Run as Administrator"

2. **Check Dependencies**
   ```
   - .NET Framework 4.8+
   - Visual C++ Redistributable
   - Crystal Reports Runtime (jika diperlukan)
   ```

3. **Database Connection Issues**
   - Cek koneksi database server
   - Verifikasi connection string
   - Test koneksi dengan tools eksternal

4. **Antivirus Blocking**
   - Add aplikasi ke whitelist antivirus
   - Temporarily disable real-time protection
   - Check quarantine folder

### 🗃️ Database Connection Problems

#### Gejala:
- "Cannot connect to database"
- "Connection timeout"
- "Access denied for user"

#### Solusi:
1. **Check Database Server Status**
   ```sql
   -- MySQL
   sudo systemctl status mysql
   
   -- SQL Server
   services.msc > SQL Server (instance)
   ```

2. **Test Connection**
   ```bash
   # MySQL
   mysql -h hostname -u username -p
   
   # SQL Server
   sqlcmd -S servername -U username
   ```

3. **Firewall Configuration**
   ```
   MySQL: Port 3306
   SQL Server: Port 1433
   PostgreSQL: Port 5432
   ```

4. **User Permissions**
   ```sql
   -- MySQL
   SHOW GRANTS FOR 'username'@'host';
   
   -- SQL Server  
   SELECT * FROM sys.database_permissions
   WHERE grantee_principal_id = USER_ID('username');
   ```

## 🔧 Specific Application Issues

### 📊 LTech Mobile

#### Sync Data Gagal
**Gejala**: Data tidak tersinkronisasi dengan server

**Solusi**:
1. Cek koneksi internet
2. Force sync: Menu > Settings > Force Sync
3. Logout dan login kembali
4. Clear app data (backup dulu data offline)

#### Inventory Tidak Update
**Gejala**: Stock tidak berubah setelah transaksi

**Solusi**:
1. Refresh halaman inventory
2. Cek apakah transaksi sudah di-approve
3. Restart aplikasi
4. Sync manual

### 🌾 AsriJaya Mobile

#### GPS Tidak Akurat
**Gejala**: Lokasi tidak tepat atau tidak terdeteksi

**Solusi**:
1. Enable High Accuracy GPS
2. Refresh location: Tap tombol GPS di app
3. Restart GPS service di device
4. Cek permission lokasi aplikasi

#### Foto Tidak Ter-upload
**Gejala**: Gambar tidak muncul di server

**Solusi**:
1. Cek koneksi internet
2. Kompres foto jika terlalu besar (>5MB)
3. Retry upload dari gallery
4. Clear cache dan coba lagi

### 💻 LuckyJaya Desktop

#### Report Tidak Muncul
**Gejala**: Crystal Reports error atau blank

**Solusi**:
1. Install Crystal Reports Runtime
2. Cek printer default
3. Verify database connection
4. Check report file path

#### Slow Performance
**Gejala**: Aplikasi lambat atau hanging

**Solusi**:
1. Close aplikasi lain yang tidak perlu
2. Increase virtual memory
3. Defrag hard disk
4. Check available RAM
5. Optimize database (rebuild indexes)

### 💰 ArisanManfaat

#### Perhitungan Salah
**Gejala**: Total arisan tidak sesuai

**Solusi**:
1. Recalculate: Menu > Tools > Recalculate
2. Check master data member
3. Verify interest rate settings
4. Backup dan reinstall jika perlu

## 📊 Performance Issues

### 🐌 Aplikasi Lambat

#### Kemungkinan Penyebab:
- Hardware tidak memenuhi minimum requirement
- Database terlalu besar tanpa maintenance
- Koneksi internet lambat
- Terlalu banyak data di local storage

#### Solusi:
1. **Hardware Upgrade**
   - RAM: minimal 4GB (8GB recommended)
   - Storage: SSD lebih baik dari HDD
   - Processor: minimal dual-core

2. **Database Maintenance**
   ```sql
   -- MySQL
   OPTIMIZE TABLE table_name;
   ANALYZE TABLE table_name;
   
   -- SQL Server
   ALTER INDEX ALL ON table_name REBUILD;
   UPDATE STATISTICS table_name;
   ```

3. **Clear Old Data**
   - Archive data lama
   - Delete temporary files
   - Clear browser cache (untuk web app)

4. **Network Optimization**
   - Gunakan koneksi kabel jika memungkinkan
   - Tutup aplikasi yang konsumsi bandwidth tinggi
   - Cek ping ke server

## 🔐 Security Issues

### 🚫 Access Denied

#### Gejala:
- "Permission denied"
- "Unauthorized access"
- "Invalid credentials"

#### Solusi:
1. **Check User Permissions**
   - Verify role assignment
   - Check module access rights
   - Contact administrator

2. **Reset Password**
   - Use forgot password feature
   - Contact admin untuk reset manual

3. **Clear Session**
   - Logout dari semua device
   - Clear cookies/app data
   - Login kembali

### 🔒 Account Locked

#### Gejala:
- "Account is locked"
- "Too many failed attempts"

#### Solusi:
1. Tunggu lock timeout (biasanya 15-30 menit)
2. Contact administrator untuk unlock
3. Reset password jika perlu

## 📱 Mobile-Specific Issues

### 📶 Offline Mode Problems

#### Sync Conflict
**Gejala**: Data berbeda antara offline dan online

**Solusi**:
1. Prioritas data server (default)
2. Manual merge dari conflict resolution screen
3. Export data offline sebagai backup

#### Storage Full
**Gejala**: "Insufficient storage space"

**Solusi**:
1. Clear app cache
2. Delete old photos/documents
3. Move media ke cloud storage
4. Uninstall unused apps

## 🆘 Emergency Procedures

### 💾 Data Recovery

#### Database Corruption
1. Stop application immediately
2. Check latest backup
3. Run database repair tools:
   ```sql
   -- MySQL
   mysqlcheck --repair database_name
   
   -- SQL Server
   DBCC CHECKDB('database_name', REPAIR_REBUILD);
   ```
4. Restore from backup jika repair gagal

#### Accidental Data Deletion
1. **Jangan panic dan jangan input data baru**
2. Check recycle bin/trash jika ada
3. Restore from latest backup
4. Contact support immediately

### 🔄 System Recovery

#### Complete System Failure
1. Document error messages dan circumstances
2. Collect log files
3. Contact emergency support: emergency@luckyjayagroup.com
4. Prepare for potential system restore

## 📋 Diagnostic Tools

### 📊 Built-in Diagnostics

#### LTech Mobile
- Menu > Settings > Diagnostic
- Export logs untuk support

#### Desktop Applications
- Help > System Information
- Tools > Database Connectivity Test
- Generate diagnostic report

### 🔍 External Tools

#### Database
- MySQL Workbench
- SQL Server Management Studio
- phpMyAdmin

#### Network
- ping, tracert, nslookup
- Wireshark untuk advanced debugging

#### System
- Task Manager / Activity Monitor
- Event Viewer (Windows)
- System logs (/var/log pada Linux)

## 📞 Escalation Path

### Level 1: Self-Service
1. Check FAQ dan documentation
2. Search GitHub Issues
3. Try basic troubleshooting

### Level 2: Community Support
1. Post di GitHub Discussions
2. Create GitHub Issue dengan template
3. Ask di community forum

### Level 3: Official Support
1. Email: support@luckyjayagroup.com
2. Include diagnostic information
3. Provide steps to reproduce

### Level 4: Emergency
1. Phone: +62-xxx-xxx-xxxx (business hours)
2. Emergency email: emergency@luckyjayagroup.com
3. For critical business-stopping issues

---

## 📝 Information to Collect Before Contacting Support

### 🔍 Basic Information
- Aplikasi name dan version
- Operating system dan version
- Error message (exact text atau screenshot)
- Steps to reproduce
- When the problem started

### 📊 Technical Information
- Hardware specifications
- Database version dan size
- Network configuration
- Log files (jika available)

### 💼 Business Impact
- How many users affected
- Business processes impacted
- Urgency level
- Workaround attempts

---

**Tips**: Selalu backup data secara regular dan document semua perubahan konfigurasi untuk mempermudah troubleshooting! 🛠️
