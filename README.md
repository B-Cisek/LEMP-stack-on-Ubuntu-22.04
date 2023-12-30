
# LEMP stack on Ubuntu 22.04

### Update Ubuntu

```bash
sudo apt update && sudo apt upgrade 
```

### Installing Nginx

```bash
sudo apt install nginx
```

- If you have the ufw firewall enabled

```bash
sudo ufw allow 'Nginx HTTP'
```

### Installing MySQL

```bash
sudo apt install mysql-server
```

- Ensure that the server is running using the systemctl start command:

```bash
sudo systemctl start mysql.service
```

- Configuring MySQL

```bash
sudo mysql
```

- Setup password

```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

### Installing PHP
- Install a few required dependencies
```bash
sudo apt install software-properties-common ca-certificates lsb-release apt-transport-https 
```

- Add the Ondrej PPA to your system, which contains all versions of PHP packages for Ubuntu systems.
```bash
LC_ALL=C.UTF-8 sudo add-apt-repository ppa:ondrej/php 
```

- Now, update the Apt package manager cache.
```bash
sudo apt update 
```

- Install php 8.3 version
 ```bash
sudo apt install php8.3-fpm
```

- Install extensions
```bash
sudo apt install php8.3-curl php8.3-dev php8.3-gd php8.3-mbstring php8.3-zip php8.3-xml php8.3-fpm php8.3-imagick php8.3-recode php8.3-xmlrpc php8.3-intl
```

### Installing Composer
```bash
curl -sS https://getcomposer.org/installer -o composer-setup.php
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

### Update Node
- Installing NVM Using curl
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
source ~/.bashrc
```

- Install specify version 
```bash
nvm install 20.10.0
```

### Configuring Nginx to Use the PHP Processor
```bash
sudo mkdir /var/www/your_domain
```
```bash
sudo chown -R $USER:$USER /var/www/your_domain
```
```bash
sudo nano /etc/nginx/sites-available/your_domain
```
```bash
server {
    listen 80;
    server_name your_domain www.your_domain;
    root /var/www/your_domain;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```

```bash
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

```bash
sudo nginx -t
sudo systemctl reload nginx
```

