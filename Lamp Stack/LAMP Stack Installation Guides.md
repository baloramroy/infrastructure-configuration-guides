Target OS: **CentOS Stream 9**

---

# SOP: LAMP Stack Installation on CentOS

---

## Purpose

To install and configure a **LAMP Stack** (Linux, Apache, MariaDB, PHP) for hosting dynamic web applications on a Linux server.

---

## Prerequisites

Before starting, ensure:

1. Server is installed with CentOS
2. You have **root** or **sudo** access
3. Internet connectivity is available
4. System is updated

---

## System Update

- Update all packages

    ```bash
    sudo dnf update -y
    ```

- Reboot if kernel is updated

    ```bash
    sudo reboot
    ```

---

## Install Apache Web Server

- Install Apache package

    ```bash
    sudo dnf install httpd -y
    ```

- Start Apache service

    ```bash
    sudo systemctl start httpd
    ```

- Enable Apache at boot

    ```bash
    sudo systemctl enable httpd
    ```

- Verify Apache status

    ```bash
    sudo systemctl status httpd
    ```

- Expected result:\
  Service status should be **active (running)**.

---

## Configure Firewall for Apache

- Allow HTTP and HTTPS traffic

    ```bash
    sudo firewall-cmd --permanent --add-service=http
    sudo firewall-cmd --permanent --add-service=https
    ```

- Reload firewall rules

    ```bash
    sudo firewall-cmd --reload
    ```

---

## Test Apache in browser

- Open browser and access:

    ```text
    http://<server-ip>
    ```

- Expected result:\
  Apache default test page is displayed.

---

## Install MariaDB Database Server

- Install MariaDB server

    ```bash
    sudo dnf install mariadb-server -y
    ```

- Start MariaDB service

    ```bash
    sudo systemctl start mariadb
    ```

- Enable MariaDB at boot

    ```bash
    sudo systemctl enable mariadb
    ```

- Verify MariaDB status

    ```bash
    sudo systemctl status mariadb
    ```

---

## Secure MariaDB Installation

Run security script

```bash
sudo mysql_secure_installation
```

Recommended answers

1. Set root password → **Y**
2. Remove anonymous users → **Y**
3. Disallow remote root login → **Y**
4. Remove test database → **Y**
5. Reload privilege tables → **Y**

---

## Install PHP and Required Modules

- Enable PHP module (if required)

    ```bash
    sudo dnf module reset php -y
    sudo dnf module enable php:8.1 -y
    ```

- Install PHP and extensions

    ```bash
    sudo dnf install php php-mysqlnd php-cli php-common php-opcache php-gd php-curl php-zip -y
    ```

- Verify PHP installation

    ```bash
    php -v
    ```

- Expected result:\
  PHP version information is displayed.

---

## Restart Apache to Load PHP

```bash
sudo systemctl restart httpd
```

---

## Test PHP Integration

- Create PHP test file

    ```bash
    sudo vi /var/www/html/info.php
    ```

- Add the following content:

    ```php
    <?php
    phpinfo();
    ?>
    ```

- Save and exit.

---

### Access PHP test page

- Open browser:

    ```text
    http://<server-ip>/info.php
    ```

- Expected result:\
  PHP information page is displayed.

---

## Verify LAMP Stack Components

Check services

```bash
systemctl status httpd
systemctl status mariadb
```

### Confirm stack components

| Component | Status  |
| --------- | ------- |
| Linux     | Running |
| Apache    | Active  |
| MariaDB   | Active  |
| PHP       | Working |

---

## Cleanup

After testing, remove PHP info file:

```bash
sudo rm -f /var/www/html/info.php
```

---

## Conclusion

The LAMP stack has been successfully **installed and configured**.
The server is now ready to host **PHP-based web applications** such as WordPress or custom applications.

---

