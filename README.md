# E-Commerce Web Server Project

## **Overview**
This project sets up an E-Commerce web server on **Ubuntu 22.04 LTS**. The server includes Apache2, MySQL Database, Redis, Mail Server (Postfix), and PHPMyAdmin for management purposes.

---

## **Tech Stack**
- **Operating System**: Ubuntu 22.04 LTS
- **Web Server**: Apache2
- **Database**: MySQL
- **Cache**: Redis
- **Mail Server**: Postfix
- **Database Management**: PHPMyAdmin

---

## **Installation Steps**
Follow the steps below to set up each service on your Ubuntu 22.04 server:

### **1. Update the System**
Always update the system before installation:
```bash
sudo apt update && sudo apt upgrade -y
```

### **2. Install Apache2 (Web Server)**
Install Apache2 and verify its status:
```bash
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```

**Test**:
- Open a browser and go to `http://localhost` or server IP.

---

### **3. Install MySQL (Database Server)**
1. Install MySQL:
   ```bash
   sudo apt install mysql-server -y
   ```
2. Secure MySQL installation:
   ```bash
   sudo mysql_secure_installation
   ```
   Follow the prompts to set a root password and improve security.

3. Check MySQL status:
   ```bash
   sudo systemctl status mysql
   ```

4. Log in to MySQL:
   ```bash
   sudo mysql -u root -p
   ```

---

### **4. Install Redis (Caching Server)**
1. Install Redis:
   ```bash
   sudo apt install redis-server -y
   ```

2. Configure Redis for production:
   - Open the configuration file:
     ```bash
     sudo nano /etc/redis/redis.conf
     ```
   - Find and change `supervised no` to:
     ```text
     supervised systemd
     ```
   - Save and exit, then restart Redis:
     ```bash
     sudo systemctl restart redis-server
     sudo systemctl enable redis-server
     ```

3. Verify Redis:
   ```bash
   redis-cli ping
   ```
   Expected output: `PONG`

---

### **5. Install Postfix (Mail Server)**
1. Install Postfix and mailutils:
   ```bash
   sudo apt install postfix mailutils -y
   ```
2. During installation, select **Internet Site** and set the system mail name (e.g., `example.com`).

3. Verify Postfix service:
   ```bash
   sudo systemctl status postfix
   ```

4. Send a test email:
   ```bash
   echo "This is a test email" | mail -s "Test Mail" user@example.com
   ```

---

### **6. Install PHP and PHPMyAdmin**
1. Install PHP and necessary extensions:
   ```bash
   sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc -y
   ```
2. Install PHPMyAdmin:
   ```bash
   sudo apt install phpmyadmin -y
   ```
   - During installation:
     - Choose Apache2.
     - Configure the database for PHPMyAdmin using dbconfig-common.
     - Set a password for the `phpmyadmin` user.

3. Link PHPMyAdmin to Apache:
   ```bash
   sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
   ```

4. Restart Apache:
   ```bash
   sudo systemctl restart apache2
   ```

**Access PHPMyAdmin**:
- Open `http://localhost/phpmyadmin` or `http://your-server-ip/phpmyadmin`.

---

## **Directory Structure**
Your project directory should look like this:
```text
/var/www/html/
    ├── your-website-folder/
    │   ├── index.html
    │   ├── style.css
    │   └── ...
    └── phpmyadmin/        # Linked PHPMyAdmin directory
```

---

## **Testing the Web Server**
1. Copy your project files to the Apache directory:
   ```bash
   sudo cp -r /path/to/your-website /var/www/html/
   ```
2. Ensure correct permissions:
   ```bash
   sudo chown -R www-data:www-data /var/www/html/your-website
   sudo chmod -R 755 /var/www/html/your-website
   ```
3. Restart Apache2:
   ```bash
   sudo systemctl restart apache2
   ```

4. Access the website:
   ```text
   http://localhost/your-website
   ```

---

## **Conclusion**
This setup will give you a fully functional E-Commerce web server environment on Ubuntu 22.04 LTS, using Apache2, MySQL, Redis, Postfix, and PHPMyAdmin.

Feel free to modify or extend this configuration for production environments!

---

