Using PHP-FPM Through Socket

Download and extract the wordpress and place it in /var/www/wordpress.
Create a Database in Mysql
Create a user in the database
Grant privileges of the databases to the user
Inside the WordPress Directory, copy the wp-config-saple.php to wp-config, and enter the Database Name,User and Password.
Inside the var/www/wordpress change the Ownership and Permissions of the file using the following command.
chown -R www-data:www-data *
chmod -R 755 *
Install nginx.
Install php7.2 php7.2-cli php7.2-fpm php7.2-mysql php7.2-json php7.2-opcache php7.2-mbstring php7.2-xml php7.2-gd php7.
In /etc/nginx/sites-available/Wordpress.conf  enter the following configuration.
server {
        listen 80 default_server;
        listen [::]:80 default_server;
root /var/www/wordpress;
index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
try_files $uri $uri/ =404;
        }

location ~ \.php$ {
                include snippets/fastcgi-php.conf;
fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
}
location ~ /\.ht {
                deny all;
        }
        location = /favicon.ico {
                         log_not_found off;
                         access_log off;
        }
         location = /robots.txt {
                         allow all;
                         log_not_found off;
                         access_log off;
           }

        location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
                         expires max;
                         log_not_found off;
           }
}

Then restart the Nginx Service.


Using PHP-FPM Through Port


Download and extract the wordpress and place it in /var/www/wordpress.
Create a Database in Mysql
Create a user in the database
Grant privileges of the databases to the user
Inside the WordPress Directory, copy the wp-config-saple.php to wp-config, and enter the Database Name,User and Password.
Inside the var/www/wordpress change the Ownership and Permissions of the file using the following command.
chown -R www-data:www-data *
chmod -R 755 *
Install nginx.
Install php7.2 php7.2-cli php7.2-fpm php7.2-mysql php7.2-json php7.2-opcache php7.2-mbstring php7.2-xml php7.2-gd php7.
In /etc/nginx/sites-available/Wordpress.conf  enter the following configuration.
In /etc/php/7.4/fpm/pool.d/www.conf comment the socket (;listen = /run/php/php7.4-fpm.sock) and change it to (listen=127.0.0.1:9000) the default port is “9000”.
And restart the Php-fpm service.
In /etc/nginx/sites-available/Wordpress.conf  enter the following configuration.
server {
        listen 80 default_server;
        listen [::]:80 default_server;
root /var/www/wordpress;
index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
try_files $uri $uri/ =404;
        }

location ~ \.php$ {
                include snippets/fastcgi-php.conf;
fastcgi_pass 127.0.0.1:9000; 
}
location ~ /\.ht {
                deny all;
        }
        location = /favicon.ico {
                         log_not_found off;
                         access_log off;
        }
         location = /robots.txt {
                         allow all;
                         log_not_found off;
                         access_log off;
           }

        location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
                         expires max;
                         log_not_found off;
           }
}

Then restart the Nginx Service.



