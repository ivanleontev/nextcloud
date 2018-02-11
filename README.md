```{r, engine='bash', code_block_name}```

# Установка NextCloud на Ubuntu 16.04
Ставим дополнительные пакеты:

`sudo apt-get install php-gd php-json php-mysql php-curl php-intl php-mcrypt php-imagick php-zip php-dom php7.0-xml php-mbstring wget unzip`

Редактируем файл "php.ini"

`sudo nano /etc/php/7.0/apache2/php.ini`

Меняем значения внутри на:

memory_limit = 512M

date.timezone = Asia/Kolkata

upload_max_filesize = 200M

post_max_size = 200M

Перезапускаем Apache:

sudo systemctl restart apache2
Теперь нужно создать базу данных в MySQL с именем "ncdb", а так же пользователя "ncuser" для управления этой базой данных:

mysql -u root -p
CREATE DATABASE ncdb;
GRANT ALL ON ncdb.* to 'ncuser'@'localhost' IDENTIFIED BY '_password_';
FLUSH PRIVILEGES;
GRANT ALL PRIVILEGES ON ncdb.* TO 'ncuser'@'localhost';
quit
Скачиваем и распаковываем NexCloud. Версия может отличатся. Используйте самую актуальную. Наберите в консоли:

wget https://download.nextcloud.com/server/releases/nextcloud-10.0.2.zip
unzip nextcloud-10.0.2.zip
Перемещаем папку с NextCloud в директорию "/var/www/html/":

sudo cp -r nextcloud/ /var/www/html/
sudo chown -R www-data:www-data /var/www/html/nextcloud/
Создаем файл "nextcloud.conf" в папке "/etc/apache2/sites-available/":

sudo nano /etc/apache2/sites-available/nextcloud.conf
Набираем:

Alias /nextcloud "/var/www/html/nextcloud/"

<Directory /var/www/html/nextcloud/>
  Options +FollowSymlinks
  AllowOverride All

 <IfModule mod_dav.c>
  Dav off
 </IfModule>

 SetEnv HOME /var/www/html/nextcloud
 SetEnv HTTP_HOME /var/www/html/nextcloud

</Directory>

Создаем ссылку на "/etc/apache2/sites-enabled/":

sudo ln -s /etc/apache2/sites-available/nextcloud.conf /etc/apache2/sites-enabled/nextcloud.conf
Включаем модули Apache:

sudo a2enmod rewrite
sudo a2enmod headers
sudo a2enmod env
sudo a2enmod dir
sudo a2enmod mime
И перезапускаем сервер:

sudo systemctl restart apache2
Теперь приступаем к завершающему этапу. Войдите в NextCloud через любой веб-браузер по адресу: "http://ip-адрес/nextcloud/"
