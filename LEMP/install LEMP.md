## Nội dung thực hành tại HostVn 
#### Bài 1 : Cài đặt Wordpress trên LEMP 

* Bước 1 : Update hệ thống và cài đặt Nginx 
~~~

apt update
apt install nginx

~~~
*Note : Kiểm tra tường lửa xem có đang chặn nginx hay không bằng câu lệnh ` ufw app list` .Nếu đang bị chặn thì chúng ta allow bằng câu lệnh `uffw allow 'nginx HTTP`*



* Bước 2 :  Cài đặ mysql 

Cài đặt mysql : 

~~~

apt install mysql-server

~~~
Cài đặt mật khẩu cho mysql bằng dòng lệnh : 

~~~

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '2@NhqJKXF!5k';

~~~

* Bước 4 : Cài đặt PHP 

Cài đặt cho PHP

~~~

sudo apt install php-fpm php-mysql

~~~

* Bước 5 : Định cấu hình cho nginx để sử dụng PHP 

Tạo thư mục chứa mã nguồn của nginx 

~~~

mkdir /var/www/hieutn.com

~~~

Chown quyền cho thư mục : 

~~~

Chown -R $USER:$USER /var/www/hieutn.com

~~~

Tạo thư mục chưa file config của nginx

~~~

nano /etc /nginx/site-available/hieutn.com

~~~

Nội dung file : 
~~~

server {
    listen 80;
    server_name hieutn.com www.hieutn.com;
    root /var/www/hieutn.com;

    index index.html index.htm index.php;

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
}

~~~
* Bước 6 : Tạo liên kết với file cấu hình mới và hủy bỏ file cũ 

Liên kết với file mới của nginx 

~~~

ln -s /etc/nginx/sites-available/hieutn.com /etc/nginx/sites-enabled/

~~~
Hủy bỏ liên kết với file default 

~~~

unlink /etc/nginx/sites-enabled/default

~~~

Kiểm tra cú pháp nginx 

~~~

nginx -t

~~~

*Note : nếu ok chúng ta tiến hành reload lại dịch vụ bằng câu lệnh `systemctl reload nginx`*

* Bước 7 : Cài đặt mã nguồn wordpress vào thư mục hieutn.com.
* bước 8 : Chỉnh sửa file wp-config.php các mục db , user, password. 
* Bước 9 : cài đặt cấu hình wordpress trên giao diện truy cập theo đường link http://ip_server. 





