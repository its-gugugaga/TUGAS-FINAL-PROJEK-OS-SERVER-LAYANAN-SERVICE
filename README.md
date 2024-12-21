# FINAL PROJEK OS SERVER SISTEM ADMIN 23.83.0977

## **Ringkasan Proyek**
Proyek ini bertujuan untuk membuat server web E-Commerce pada **Ubuntu 22.04 LTS**. Server ini mencakup layanan MySQL Database, Redis, Mail Server (Postfix), PHPMyAdmin untuk keperluan manajemen database, dan menggunakan framework Flask untuk routing serta Gunicorn sebagai aplikasi server.

---

## **Spesifikasi dan Layanan yang Digunakan**
- **Sistem Operasi**: Ubuntu 22.04 LTS
- **RAM**: 3 GB
- **Penyimpanan**: Harddisk 80 GB
- **Web Server**: Flask dengan Gunicorn
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

### **2. Instalasi MySQL (Database Server)**
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

### **3. Instalasi Redis (Caching Server)**
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

### **4. Instalasi Postfix (Mail Server)**
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

### **5. Instalasi PHP dan PHPMyAdmin**
1. Instal PHP dan ekstensi yang diperlukan:
   ```bash
   sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc -y
   ```
2. Instal PHPMyAdmin:
   ```bash
   sudo apt install phpmyadmin -y
   ```
   - Saat instalasi:
     - Konfigurasi database untuk PHPMyAdmin menggunakan dbconfig-common.
     - Atur kata sandi untuk user `phpmyadmin`.

3. Hubungkan PHPMyAdmin:
   ```bash
   sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
   ```

**Akses PHPMyAdmin**:
- Buka `http://tokopedidi.shop/phpmyadmin` atau `http://IP-server/phpmyadmin`.

---

### **6. Instalasi Flask dengan Gunicorn**
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
   mkdir /templates
   nano app.py
   ```
   Contoh isi file:
   ```python
   from flask import Flask, render_template

   app = Flask(__name__)

   @app.route('/')
   def home():
       return render_template('index.html')

   if __name__ == '__main__':
       app.run()
   ```

4. Tambahkan file HTML di direktori `templates`:
   ```bash
   nano /templates/index.html
   ```
   Contoh isi file:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Welcome</title>
   </head>
   <body>
       <h1>Welcome to Flask with Gunicorn</h1>
   </body>
   </html>
   ```

5. Jalankan aplikasi Flask menggunakan Gunicorn:
   ```bash
   gunicorn -b 127.0.0.0:5000 app:app
   ```

---

## **Struktur Direktori**
Struktur direktori proyek Saya:
```text
/var/www/html/
    │   ├── app.py          # Aplikasi Flask
    │   └── templates/      # Direktori untuk file HTML
    │       └── index.html
    └── phpmyadmin/         # Direktori PHPMyAdmin yang ditautkan
```

---

## **Pengujian Web Server**
1. Jalankan aplikasi Flask:
   ```bash
   cd /templates
   gunicorn -b 127.0.0.0:5000 app:app
   ```

2. Akses website melalui domain Anda:
   ```text
   https://tokopedidi.shop
   ```

---

## **Kesimpulan**
Dengan konfigurasi ini, Saya memiliki lingkungan server E-Commerce yang berfungsi penuh di Ubuntu 22.04 LTS menggunakan MySQL, Redis, Postfix, PHPMyAdmin, Flask untuk routing dengan Gunicorn sebagai aplikasi server, serta keamanan tambahan melalui Cloudflare, yang diakses melalui domain [tokopedidi.shop](http://tokopedidi.shop).

