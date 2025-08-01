# Panduan Instalasi dan Setup

## 📱 LTech Mobile

### Android
1. **Download APK**
   - Unduh dari [Google Play Store](https://play.google.com/store/apps/details?id=com.luckyjaya.ltechmobile) 
   - Atau unduh APK dari website resmi

2. **Instalasi**
   - Buka file APK yang sudah didownload
   - Izinkan instalasi dari "Unknown Sources" jika diminta
   - Ikuti wizard instalasi

3. **Setup Awal**
   - Buka aplikasi LTech Mobile
   - Masukkan Server URL yang diberikan admin
   - Login dengan username/password
   - Tunggu proses sinkronisasi data awal

### iOS
1. **Download dari App Store**
   - Cari "LTech Mobile" di App Store
   - Tap "Get" untuk install

2. **Setup Awal**
   - Sama seperti Android di atas

## 🌾 AsriJaya Mobile

### Persyaratan Sistem
- Android 6.0+ atau iOS 12.0+
- RAM minimal 2GB
- Storage kosong minimal 500MB
- Koneksi internet stabil

### Instalasi
1. Download dari Play Store/App Store
2. Login dengan akun yang diberikan distributor
3. Pilih wilayah operasi
4. Sinkronisasi data master

## 🖥️ LuckyJaya Desktop

### Persyaratan Sistem
- Windows 10/11 (64-bit)
- RAM minimal 4GB (8GB direkomendasikan)
- Storage kosong minimal 2GB
- .NET Framework 4.8+
- SQL Server 2016+ atau MySQL 8.0+

### Instalasi
1. **Download Installer**
   - Unduh setup file dari portal customer
   - Pastikan antivirus tidak memblokir

2. **Proses Instalasi**
   ```
   1. Jalankan setup.exe sebagai Administrator
   2. Pilih lokasi instalasi
   3. Pilih komponen yang akan diinstall:
      - Core Application (wajib)
      - Crystal Reports (untuk laporan)
      - Database Tools (jika database lokal)
   4. Tunggu proses instalasi selesai
   ```

3. **Konfigurasi Database**
   - Buka aplikasi pertama kali
   - Masuk ke menu "Konfigurasi Database"
   - Masukkan connection string database
   - Test koneksi dan simpan

### Update Aplikasi
- Aplikasi akan otomatis cek update saat startup
- Atau manual melalui menu "Help > Check for Updates"
- Backup data sebelum update major version

## 💰 ArisanManfaat

### Instalasi Standalone
1. Extract file ArisanManfaat.zip
2. Jalankan ArisanManfaat.exe
3. Setup database (SQLite lokal atau MySQL server)

### Instalasi Multi-User
1. Setup database MySQL di server
2. Install aplikasi di setiap workstation
3. Konfigurasi koneksi ke database server
4. Setup user dan permission

## 🗄️ Konfigurasi Database

### MySQL Setup
```sql
-- Buat database
CREATE DATABASE luckyjaya_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Buat user
CREATE USER 'luckyjaya_user'@'%' IDENTIFIED BY 'strong_password';
GRANT ALL PRIVILEGES ON luckyjaya_db.* TO 'luckyjaya_user'@'%';
FLUSH PRIVILEGES;
```

### SQL Server Setup
```sql
-- Buat database
CREATE DATABASE luckyjaya_db
ON (NAME = 'luckyjaya_db_data', FILENAME = 'C:\Database\luckyjaya_db.mdf')
LOG ON (NAME = 'luckyjaya_db_log', FILENAME = 'C:\Database\luckyjaya_db.ldf');

-- Buat login dan user
CREATE LOGIN luckyjaya_user WITH PASSWORD = 'StrongPassword123!';
USE luckyjaya_db;
CREATE USER luckyjaya_user FOR LOGIN luckyjaya_user;
ALTER ROLE db_owner ADD MEMBER luckyjaya_user;
```

## 🔧 Troubleshooting Instalasi

### Error "Missing .NET Framework"
- Download dan install .NET Framework 4.8 dari Microsoft
- Restart komputer setelah instalasi

### Error "Database Connection Failed"
- Pastikan database server running
- Cek firewall tidak memblokir port database
- Verifikasi username/password database
- Test koneksi menggunakan tools seperti MySQL Workbench

### Error "Insufficient Privileges"
- Jalankan aplikasi sebagai Administrator
- Pastikan user database memiliki permission yang cukup

### Aplikasi Lambat
- Cek spesifikasi hardware memenuhi minimum requirement
- Tutup aplikasi lain yang tidak perlu
- Defrag hard disk jika menggunakan HDD
- Pastikan koneksi internet stabil untuk cloud database

## 📞 Dukungan Instalasi

Jika mengalami kesulitan instalasi:
1. Cek [FAQ](faq.md) untuk masalah umum
2. Cari di [Issues](https://github.com/zahrasiska/luckyjayagroup-community/issues)
3. Buat issue baru dengan tag "installation"
4. Email support: support@luckyjayagroup.com

## 🔄 Uninstall

### Windows
- Control Panel > Programs > Uninstall
- Atau gunakan uninstaller di folder instalasi

### Android/iOS  
- Uninstall seperti aplikasi lainnya
- Data lokal akan terhapus, data server tetap aman

---

**Catatan**: Selalu backup data sebelum instalasi, update, atau uninstall aplikasi! 💾
