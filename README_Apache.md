Using PHP-FPM Through Socket using SetHandler




systemctl stop apache2



a2dismod php7.2

a2dismod mpm_prefork

apt install php-fpm

apt install libapache2-mod-fcgid

a2enconf php7.2-fpm
a2enmod proxy
a2enmod proxy_fcgi

systemctl status php7.4-fpm

a2enmod actions alias proxy_fcgi setenvif

vim /etc/apache2/sites-available/000-default.conf

cd 

Before </Virtualhost>

DirectoryIndex info.php
        <Directory /var/www/wordpress/>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Order allow,deny
        allow from all
     </Directory>
<FilesMatch \.php$>
        # 2.4.10+ can proxy to unix socket
         SetHandler "proxy:unix:/run/php/php7.4-fpm.sock|fcgi://localhost"
	  #fcgi://localhost or Domain Name"
        </FilesMatch>

systemctl restart apache2
https://dev.to/joetancy/php-fpm-with-apache2-2mk0


Using PHP-FPM Through Port using SetHandler




https://dev.to/joetancy/php-fpm-with-apache2-2mk
<VirtualHost *:80>
ServerAdmin webmaster@localhost
DocumentRoot /var/www/wordpress
 Redirect / https://harish.com/
<FilesMatch \.(php|phar)$>
SetHandler "proxy:fcgi://127.0.0.1:9000"
</FilesMatch>
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Initially change the FilesMatch and SetHandler as mentioned above .

<IfModule mod_ssl.c>
<VirtualHost _default_:443>
ServerAdmin webmaster@localhost

DocumentRoot /var/www/wordpress
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
SSLCertificateFile    /etc/ssl/certs/wordpress_selfsigned.key.crt
SSLCertificateKeyFile /etc/ssl/private/wordpress_selfsigned.key
<FilesMatch "\.(cgi|shtml|phtml|php)$">
 SSLOptions +StdEnvVars
 </FilesMatch>
 <Directory /usr/lib/cgi-bin>
SSLOptions +StdEnvVars
</Directory>
</VirtualHost>
</IfModule>
Change the ssl conf file as the same and restart the apache2.services .




Using PHP-FPM Through Port using Proxy Pass


apt install php-fpm

<VirtualHost *:80>
ServerName harish.com
ServerAdmin webmaster@localhost
DocumentRoot /var/www/wordpress
ProxyPassMatch ^/(.*\.php(/.)?)$fcgi://127.0.0.1:9000/var/www/wordpress/$1
Redirect / https://harish.com/
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

In VirtualHost 80 enter the Proxy pass as mentioned along with the document root.

<IfModule mod_ssl.c>
<VirtualHost _default_:443>
ServerAdmin webmaster@localhost
DocumentRoot /var/www/wordpress
ProxyPassMatch ^/(.*\.php(/.)?)$fcgi://127.0.0.1:9000/var/www/wordpress/$1
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
SSLEngine on
SSLCertificateFile      /etc/ssl/certs/wordpress_selfsigned.key.crt
SSLCertificateKeyFile /etc/ssl/private/wordpress_selfsigned.key
<FilesMatch "\.(cgi|shtml|phtml|php)$">
 SSLOptions +StdEnvVars
</FilesMatch>
<Directory /usr/lib/cgi-bin>
 SSLOptions +StdEnvVars
</Directory>
</VirtualHost>
</IfModule>
In VirtualHost 443 enter the Proxy pass as mentioned along with the document root.
















