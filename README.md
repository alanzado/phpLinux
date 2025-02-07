# phpLinux

## Comece baixando o repositorio ondrej 
```
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update

sudo apt install php5.6 php5.6-fpm php7.0 php7.0-fpm php7.4 php7.4-fpm -y

sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php5.6-fpm php7.0-fpm php7.4-fpm
sudo systemctl restart apache2

sudo echo "<VirtualHost *:8056>
    ServerName localhost
    DocumentRoot /var/www/php56

    <Directory /var/www/php56>
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php5.6-fpm.sock|fcgi://localhost/"
    </FilesMatch>

    ErrorLog ${APACHE_LOG_DIR}/php56-error.log
    CustomLog ${APACHE_LOG_DIR}/php56-access.log combined
</VirtualHost>" > /etc/apache2/sites-available/php56.conf

sudo echo "<VirtualHost *:8070>
    ServerName localhost
    DocumentRoot /var/www/php70

    <Directory /var/www/php70>
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php7.0-fpm.sock|fcgi://localhost/"
    </FilesMatch>

    ErrorLog ${APACHE_LOG_DIR}/php70-error.log
    CustomLog ${APACHE_LOG_DIR}/php70-access.log combined
</VirtualHost>" > /etc/apache2/sites-available/php70.conf

sudo echo "<VirtualHost *:8074>
    ServerName localhost
    DocumentRoot /var/www/php74

    <Directory /var/www/php74>
        AllowOverride All
        Require all granted
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php7.4-fpm.sock|fcgi://localhost/"
    </FilesMatch>

    ErrorLog ${APACHE_LOG_DIR}/php74-error.log
    CustomLog ${APACHE_LOG_DIR}/php74-access.log combined
</VirtualHost>" > /etc/apache2/sites-available/php74.conf

sudo a2ensite php56.conf
sudo a2ensite php70.conf
sudo a2ensite php74.conf

sudo chmod 777 -R /var/www/

mkdir -p /var/www/php56
echo "<?php phpinfo(); ?>" > /var/www/php56/index.php

mkdir -p /var/www/php70
echo "<?php phpinfo(); ?>" > /var/www/php70/index.php

mkdir -p /var/www/php74
echo "<?php phpinfo(); ?>" > /var/www/php74/index.php


sudo echo "Listen 8056
Listen 8070
Listen 8074" >> /etc/apache2/ports.conf

sudo systemctl restart apache2

```

PHP 5.6 → http://localhost:8056/
PHP 7.0 → http://localhost:8070/
PHP 7.4 → http://localhost:8074/