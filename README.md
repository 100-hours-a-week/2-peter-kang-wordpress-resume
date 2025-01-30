## 클라우드 1주차 과제 - LAMP 서버에 Wordpress 이력서 작성
### APM 설치
```Bash
# 저장소 업데이트
sudo apt update
# 아파치 설치
sudo apt install apache2
# mysql 설치 
sudo apt install mysql-server
# mysql 설정
sudo mysql_secure_installation
# php 및 php-mysql 라이브러리 설치
sudo apt install php php-mysql
```
### WordPress 설치
```Bash
# WordPress 설치 및 설정
sudo mkdir -p /srv/www
sudo chown www-data: /srv/www
curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www

# apache site를 wordpress를 생성한다.
sudo vim /etc/apache2/sites-available/wordpress.conf

<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>

# 사이트 허용
sudo a2ensite wordpress
# URL 재작성 허용
sudo a2enmod rewrite
# 'It Works' 페이지 삭제
sudo a2dissite 000-default
#변경사항 적용
sudo service apache2 reload
```
### mysql 설정
```Bash
# mysql 로그인
sudo mysql -u root
# db 생성
CREATE DATABASE wordpress;
# user 생성
CREATE USER wordpress@localhost IDENTIFIED BY 'wordpress123';
# user 권한 부여
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON wordpress.* TO wordpress@localhost;
# 권한 적용
FLUSH PRIVILEGES;
# mysql 로그아웃
quit
```
### wordpress mysql 연결
```Bash
# wordpress 설정 파일 복사
sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php
# mysql 설정 php 삽입
sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/password_here/wordpress123/' /srv/www/wordpress/wp-config.php
# wordpress 설정 파일 수정 (아래줄 삭제)
define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );
```
### 이력서 작성를 작성한다.
![gif](https://github.com/user-attachments/assets/f1f1f38c-8338-4ce9-976e-0556113ec030)