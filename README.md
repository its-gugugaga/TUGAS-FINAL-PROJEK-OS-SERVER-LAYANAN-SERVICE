# **FINAL PROJEK OS SERVER SISTEM ADMIN 23.83.0977**

## **Ringkasan Proyek**
Proyek ini bertujuan untuk membuat server web E-Commerce pada **Ubuntu 22.04 LTS**. Server ini mencakup layanan **Apache2**, MySQL Database, Redis, Mail Server (Postfix), PHPMyAdmin untuk keperluan manajemen database, dan menggunakan framework Flask untuk routing serta Gunicorn sebagai aplikasi server.

---

## **Spesifikasi dan Layanan yang Digunakan**
- **Sistem Operasi**: Ubuntu 22.04 LTS  
- **RAM**: 3 GB  
- **Penyimpanan**: Harddisk 80 GB  
- **Web Server**: Apache2, Flask dengan Gunicorn  
- **Database**: MySQL  
- **Cache**: Redis  
- **Mail Server**: Postfix  
- **Manajemen Database**: PHPMyAdmin  
- **Framework**: Flask  
- **Keamanan dan Optimalisasi**: Cloudflare  
- **Domain**: [tokopedidi.shop](https://tokopedidi.shop)  

---

## **Langkah Instalasi**
Ikuti langkah-langkah di bawah ini untuk mengatur setiap layanan pada server Ubuntu 22.04:

### **1. Update Sistem**
Selalu perbarui sistem sebelum melakukan instalasi:
```bash
sudo apt update && sudo apt upgrade -y
```

---

### **2. Instalasi Apache2 (Web Server)**
1. Instal Apache2:
   ```bash
   sudo apt install apache2 -y
   ```
2. Periksa status Apache2:
   ```bash
   sudo systemctl status apache2
   ```
3. Aktifkan Apache2 saat booting:
   ```bash
   sudo systemctl enable apache2
   ```
4. Konfigurasi Firewall:
   ```bash
   sudo ufw allow 'Apache Full'
   sudo ufw reload
   ```
5. Uji server dengan membuka browser dan mengakses `http://IP-server` atau `http://tokopedidi.shop`. Anda akan melihat halaman default Apache2.

---

### **3. Integrasi Flask dengan Apache2 melalui WSGI**
1. Instal modul WSGI untuk Apache2:
   ```bash
   sudo apt install libapache2-mod-wsgi-py3 -y
   ```
2. Buat file konfigurasi Apache untuk aplikasi Flask:
   ```bash
   sudo nano /etc/apache2/sites-available/flaskapp.conf
   ```
   Isi file:
   ```text
   <VirtualHost *:80>
       ServerName tokopedidi.shop
       ServerAlias www.tokopedidi.shop
       DocumentRoot /var/www/html
       
       WSGIDaemonProcess flaskapp threads=5
       WSGIScriptAlias / /var/www/html/flaskapp.wsgi

       <Directory /var/www/html>
           Require all granted
       </Directory>

       ErrorLog ${APACHE_LOG_DIR}/flaskapp_error.log
       CustomLog ${APACHE_LOG_DIR}/flaskapp_access.log combined
   </VirtualHost>
   ```
3. Buat file WSGI untuk aplikasi Flask:
   ```bash
   sudo nano /var/www/html/flaskapp.wsgi
   ```
   Isi file:
   ```python
   import sys
   sys.path.insert(0, "/var/www/html")

   from app import app as application
   ```
4. Aktifkan konfigurasi dan restart Apache2:
   ```bash
   sudo a2ensite flaskapp
   sudo systemctl reload apache2
   ```

---

### **4. Instalasi MySQL (Database Server)**
1. Instal MySQL:
   ```bash
   sudo apt install mysql-server -y
   ```
2. Amankan instalasi MySQL:
   ```bash
   sudo mysql_secure_installation
   ```
3. Periksa status MySQL:
   ```bash
   sudo systemctl status mysql
   ```
4. Masuk ke MySQL:
   ```bash
   sudo mysql -u root -p
   ```

---

### **5. Instalasi Redis (Caching Server)**
1. Instal Redis:
   ```bash
   sudo apt install redis-server -y
   ```
2. Konfigurasi Redis:
   - Buka file konfigurasi:
     ```bash
     sudo nano /etc/redis/redis.conf
     ```
   - Ubah `supervised no` menjadi:
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

### **6. Instalasi Postfix (Mail Server)**
1. Instal Postfix dan mailutils:
   ```bash
   sudo apt install postfix mailutils -y
   ```
2. Saat instalasi, pilih **Internet Site** dan atur **system mail name** (contoh: `tokopedidi.shop`).
3. Verifikasi layanan Postfix:
   ```bash
   sudo systemctl status postfix
   ```
4. Kirim email uji:
   ```bash
   echo "Ini adalah email uji" | mail -s "Test Mail" user@example.com
   ```

---

### **7. Instalasi PHP dan PHPMyAdmin**
1. Instal PHP dan ekstensi yang diperlukan:
   ```bash
   sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc -y
   ```
2. Instal PHPMyAdmin:
   ```bash
   sudo apt install phpmyadmin -y
   ```
3. Hubungkan PHPMyAdmin:
   ```bash
   sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
   ```
   **Akses PHPMyAdmin**:
   - Buka `http://tokopedidi.shop/phpmyadmin` atau `http://IP-server/phpmyadmin`.

---

### **8. Instalasi Flask dengan Gunicorn**
1. Instal Python dan pip:
   ```bash
   sudo apt install python3 python3-pip -y
   ```
2. Instal Flask dan Gunicorn:
   ```bash
   pip3 install flask gunicorn
   ```
3. Buat file aplikasi Flask sederhana:
   ```bash
   nano /icikiwir/app.py
   ```
   Contoh isi file:
   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route('/')
   def home():
       return "Welcome to Flask with Gunicorn!"

   if __name__ == '__main__':
       app.run()
   ```
4. Jalankan aplikasi Flask menggunakan Gunicorn:
   ```bash
   gunicorn -b 127.0.0.1:5000 app:app
   ```

---

## **Struktur Direktori**
Struktur direktori proyek:
```text
/icikiwir
    ├── app.py          # Aplikasi Flask
    ├── flaskapp.wsgi   # File WSGI untuk Flask
    └── templates/      # Direktori untuk file HTML
        └── index.html
    └── phpmyadmin/     # Direktori PHPMyAdmin yang ditautkan
```

---

## **Pengujian Web Server**
1. Jalankan server Apache2:
   ```bash
   sudo systemctl start apache2
   ```
2. Jalankan aplikasi Flask menggunakan Gunicorn:
   ```bash
   gunicorn -b 127.0.0.1:5000 app:app
   ```
3. Akses website melalui domain Anda:
   ```text
   http://tokopedidi.shop
   

---

## **Kesimpulan**
Dengan konfigurasi ini, Saya memiliki lingkungan server E-Commerce yang berfungsi penuh di Ubuntu 22.04 LTS menggunakan Apache2, MySQL, Redis, Postfix, PHPMyAdmin, Flask untuk routing dengan Gunicorn sebagai aplikasi server, serta keamanan tambahan melalui Cloudflare, yang diakses melalui domain [tokopedidi.shop](https://tokopedidi.shop).


