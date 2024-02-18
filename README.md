# NGINX-Serever-Blocks-Host-Multiple-Websites-On-One-Server-With-Single-IP-Address

```bash
sudo mkdir -p /var/www/example.com/public/
sudo mkdir -p /var/www/test.com/public/

sudo chown -R volodymyr:volodymyr /var/www/example.com/public
sudo chown -R volodymyr:volodymyr /var/www/test.com/public

sudo chmod 775 /var/www/example.com/public
sudo chmod 775 /var/www/test.com/public

Create index.php files in both folders /var/www/example.com/public 
and /var/www/test.com/public

In foledr /etc/nginx/sites-available/  create files example.com and test.com

example.com

	server {
       listen *:80 default_server;
       listen [::]:80 default_server;

       server_name example.test;

       root /var/www/example.com/public/;

       index index.html index.htm index.php;

       location / {
           try_files $uri $uri/ /index.php$is_args$args;
       }

       location ~ \.php$ {
           fastcgi_split_path_info ^(.+\.php)(/.+)$;
           fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
           fastcgi_index index.php;
           include fastcgi.conf;
      }
}
----------------

test.com

	server {
    listen *:80;
    listen [::]:80;

    server_name test.test;

    root /var/www/test.com/public/;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_index index.php;
        include fastcgi.conf;
    }
}

----------------

cd to folder `/etc/nginx/sites-available`

sudo ln -s /etc/nginx/sites-available/example.com ../sites-enabled/example.com
sudo ln -s /etc/nginx/sites-available/test.com ../sites-enabled/test.com

In file /etc/hosts:
	127.0.0.1       example.test
	127.0.0.1       test.test
	
sudo systemctl restart nginx
```
