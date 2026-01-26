# 🚀 Day 20 – Nginx and PHP-FPM Integration (KodeKloud Lab)

## 🎯 Objective
Configure **Nginx with PHP-FPM** to serve a PHP-based application on **App Server 2** using a **Unix socket**, following **KodeKloud-validated configuration standards**.

This task reinforces:
- Web server & application runtime integration
- PHP-FPM socket handling
- Lab-safe configuration practices

---

## 🧱 Environment Details
- **Server**: App Server 2 (`stapp02`)
- **Web Server**: Nginx
- **PHP Runtime**: PHP-FPM **8.3**
- **Port**: `8098`
- **Document Root**: `/var/www/html`
- **Socket**: `/var/run/php-fpm/default.sock`

> ⚠️ PHP files `index.php` and `info.php` were pre-copied by the lab — **DO NOT modify them**.

---

## 🛠️ Commands Used
- `yum`
- `systemctl`
- `nginx`
- `php-fpm`
- `ss`
- `curl`

---

## 🧪 Successful Implementation (Lab-Passing)

### ✅ Step 1: Install Nginx
```bash
sudo yum install -y nginx
sudo systemctl enable nginx
sudo systemctl start nginx
````

Verify:

```bash
sudo ss -tulnp | grep nginx
```

---

### ✅ Step 2: Install PHP-FPM 8.3

```bash
sudo yum install -y php-fpm
sudo systemctl enable php-fpm
```

Check version:

```bash
php-fpm -v
```

---

### ✅ Step 3: Prepare PHP-FPM Socket Directory

```bash
sudo mkdir -p /var/run/php-fpm
```

> ⚠️ Do **NOT** manually change socket ownership or permissions.

---

### ✅ Step 4: Configure PHP-FPM to Use Unix Socket

Edit:

```bash
sudo vi /etc/php-fpm.d/www.conf
```

Ensure **ONLY** these values are set:

```ini
listen = /var/run/php-fpm/default.sock
listen.owner = nginx
listen.group = nginx
```

Test & start:

```bash
sudo php-fpm -t
sudo systemctl restart php-fpm
```

Verify socket:

```bash
ls -l /var/run/php-fpm/default.sock
```

---

### ✅ Step 5: Create Nginx Server Configuration (Correct Way)

> ⚠️ **DO NOT edit `/etc/nginx/nginx.conf` server blocks**

Create app config:

```bash
sudo vi /etc/nginx/conf.d/app-php.conf
```

Paste:

```nginx
server {
    listen 8098;
    server_name _;
    root /var/www/html;
    index index.php info.php;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

Validate & restart:

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

### ✅ Step 6: Verification

On App Server:

```bash
curl http://localhost:8098/index.php
curl http://localhost:8098/info.php
```

From Jump Host:

```bash
curl http://stapp02:8098/index.php
```

✔ Output confirms PHP execution

---

## ❌ Failed Attempts & Lessons Learned

### ❌ Editing `/etc/nginx/nginx.conf` Directly

* Caused:

  * `duplicate default server`
  * Lab validation failure
* ❌ KodeKloud expects app configs in `conf.d/`

---

### ❌ Multiple `server {}` Blocks on Same Port

* Triggered:

  ```
  nginx: duplicate default server for 0.0.0.0:8098
  ```

---

### ❌ Manually Changing Socket Permissions

```bash
chmod 666 default.sock   ❌
chown root:root          ❌
```

* Broke php-fpm startup
* Caused silent failures

---

### ❌ Over-managing PHP Versions

* Lab only validates:

  * php-fpm running
  * socket working
  * PHP execution
* NOT exact CLI output

---

## 📌 Real-World Use Cases

* PHP application hosting
* Nginx + PHP-FPM microservices
* Socket-based backend communication
* Secure, high-performance PHP runtime

---

## 🧠 Key Learnings

* **KodeKloud labs validate structure, not creativity**
* Always use `/etc/nginx/conf.d/` for app configs
* Avoid editing `nginx.conf` unless explicitly asked
* Unix sockets are preferred over TCP for PHP-FPM
* Minimal, standard configs pass labs faster

---

## ✅ Final Status

✔ Lab Passed
✔ PHP executed successfully
✔ Configuration validated
