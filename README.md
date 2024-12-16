# FINAL PROJEK OS SERVER SISTEM ADMIN 23.83.0977

## **Ringkasan Proyek**
Proyek ini bertujuan untuk membuat server web E-Commerce pada **Ubuntu 22.04 LTS**. Server ini mencakup layanan Apache2, MySQL Database, Redis, Mail Server (Postfix), dan PHPMyAdmin untuk keperluan manajemen database.

---

## **Spesifikasi dan Layanan yang Digunakan**
- **Sistem Operasi**: Ubuntu 22.04 LTS
- **Web Server**: Apache2
- **Database**: MySQL
- **Cache**: Redis
- **Mail Server**: Postfix
- **Manajemen Database**: PHPMyAdmin

---

## **Langkah Instalasi**
Ikuti langkah-langkah di bawah ini untuk mengatur setiap layanan pada server Ubuntu 22.04:

### **1. Update Sistem**
Selalu perbarui sistem sebelum melakukan instalasi:
```bash
sudo apt update && sudo apt upgrade -y
```

### **2. Instalasi Apache2 (Web Server)**
Instal Apache2 dan periksa statusnya:
```bash
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```

**Pengujian**:
- Buka browser dan akses `http://localhost` atau alamat IP server Anda.

---

### **3. Instalasi MySQL (Database Server)**
1. Instal MySQL:
   ```bash
   sudo apt install mysql-server -y
   ```
2. Amankan instalasi MySQL:
   ```bash
   sudo mysql_secure_installation
   ```
   Ikuti perintah untuk mengatur kata sandi root dan meningkatkan keamanan.

3. Periksa status MySQL:
   ```bash
   sudo systemctl status mysql
   ```

4. Masuk ke MySQL:
   ```bash
   sudo mysql -u root -p
   ```

---

### **4. Instalasi Redis (Caching Server)**
1. Instal Redis:
   ```bash
   sudo apt install redis-server -y
   ```

2. Konfigurasi Redis:
   - Buka file konfigurasi:
     ```bash
     sudo nano /etc/redis/redis.conf
     ```
   - Temukan dan ubah `supervised no` menjadi:
     ```text
     supervised systemd
     ```
   - Simpan perubahan dan restart Redis:
     ```bash
     sudo systemctl restart redis-server
     sudo systemctl enable redis-server
     ```

3. Verifikasi Redis:
   ```bash
   redis-cli ping
   ```
   Output yang diharapkan: `PONG`

---

### **5. Instalasi Postfix (Mail Server)**
1. Instal Postfix dan mailutils:
   ```bash
   sudo apt install postfix mailutils -y
   ```
2. Saat instalasi, pilih **Internet Site** dan atur **system mail name** (contoh: `awikwok.com`).

3. Verifikasi layanan Postfix:
   ```bash
   sudo systemctl status postfix
   ```

4. Kirim email uji:
   ```bash
   echo "Ini adalah email uji" | mail -s "Test Mail" user@example.com
   ```

---

### **6. Instalasi PHP dan PHPMyAdmin**
1. Instal PHP dan ekstensi yang diperlukan:
   ```bash
   sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc -y
   ```
2. Instal PHPMyAdmin:
   ```bash
   sudo apt install phpmyadmin -y
   ```
   - Saat instalasi:
     - Pilih Apache2.
     - Konfigurasi database untuk PHPMyAdmin menggunakan dbconfig-common.
     - Atur kata sandi untuk user `phpmyadmin`.

3. Hubungkan PHPMyAdmin ke Apache:
   ```bash
   sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
   ```

4. Restart Apache:
   ```bash
   sudo systemctl restart apache2
   ```

**Akses PHPMyAdmin**:
- Buka `http://localhost/phpmyadmin` atau `http://IP-server/phpmyadmin`.

---

## **Struktur Direktori**
Struktur direktori proyek Saya:
```text
/var/www/html/
    │   ├── index.html
    │   ├── style.css
    │   └── ...
    └── phpmyadmin/        # Direktori PHPMyAdmin yang ditautkan
```

---

## **Pengujian Web Server**
1. Salin file proyek ke direktori Apache:
   ```bash
   sudo cp -r /path/to/proyek-website /var/www/html/
   ```
2. Atur izin yang benar:
   ```bash
   sudo chown -R www-data:www-data /var/www/html/proyek-website
   sudo chmod -R 755 /var/www/html/proyek-website
   ```
3. Restart Apache2:
   ```bash
   sudo systemctl restart apache2
   ```

4. Akses website:
   ```text
   http://localhost/proyek-website
   ```

---

## **Kesimpulan**
Dengan konfigurasi ini, Saya memiliki lingkungan server E-Commerce yang berfungsi penuh di Ubuntu 22.04 LTS menggunakan Apache2, MySQL, Redis, Postfix, dan PHPMyAdmin.


---

