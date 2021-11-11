# Deployment Instruction

```bash
https://blog.logrocket.com/setting-up-a-remote-postgres-database-server-on-ubuntu-18-04/
https://www.hostinger.co.id/tutorial/install-laravel-di-ubuntu/
https://tudip.com/blog-post/laravel-queue-and-running-it-with-supervisor/
```

```bash
sudo apt update
sudo apt install apache2

sudo add-apt-repository ppa:ondrej/php
sudo apt install php7.4
sudo a2enmod php7.4
sudo update-alternatives --set php /usr/bin/php7.4
sudo update-alternatives --config php
sudo update-alternatives --set phar /usr/bin/phar7.4
sudo systemctl restart apache2

sudo apt-get update
sudo apt-get install zip unzip
sudo apt-get install libapache2-mod-php7.4 php7.4 php7.4-tokenizer php7.4-json php7.4-bcmath php7.4-zip php7.4-xmlrpc php7.4-xml php7.4-gd php7.4-opcache php7.4-mbstring
sudo apt-get install php7.4-pgsql
sudo systemctl restart apache2
sudo nano /var/www/html/test.php
<?php
phpinfo();
?>

curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
sudo chmod +x /usr/local/bin/composer

sudo chgrp -R www-data /var/www/html/cv_petro/
sudo chmod -R 775 /var/www/html/cv_petro/storage

sudo nano riwayat_jabatan.conf
<VirtualHost *:80>
   ServerName rj.petrokimia-gresik.com
   ServerAdmin webmaster@localhost
   DocumentRoot /var/www/html/karir_petro/public

   <Directory /var/www/html/karir_petro>
       AllowOverride All
   </Directory>
   ErrorLog ${APACHE_LOG_DIR}/error.log
   CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
sudo a2dissite 000-default.conf
sudo a2ensite riwayat_jabatan
sudo a2enmod rewrite
sudo systemctl restart apache2

sudo apt install postgresql postgresql-contrib
createuser --interactive --pwprompt
createdb -O riwayat_jabatan riwayat_jabatan
sudo nano /etc/postgresql/10/main/postgresql.conf
listen_addresses = '*'
sudo nano /etc/postgresql/10/main/pg_hba.conf
host    all             all             0.0.0.0/0            md5
sudo ufw allow 5432/tcp
```

```bash
php artisan queue:work --tries=3 --timeout=300 >> storage/logs/jobs.log &

sudo apt install supervisor
service supervisor status
sudo nano /etc/supervisor/conf.d/queue.conf

[program:queue]

process_name=%(program_name)s_%(process_num)02d
command=php /var/www/html/karir_petro/artisan queue:work --tries=3 --sleep=3
user=root
autostart=true
autorestart=true
numprocs=3
redirect_stderr=true
stdout_logfile=/var/www/html/karir_petro/storage/logs/queue.log

sudo supervisorctl reread

sudo supervisorctl update

sudo supervisorctl start queue:*
```

```
sudo apt-get install redis-server
composer install predis/predis
REDIS_CLIENT=predis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379
```

```
upload_max_filesize=20M
post_max_size=40M
memory_limit=2048M
```
