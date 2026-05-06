<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

<p align="center">
<a href="https://github.com/laravel/framework/actions"><img src="https://github.com/laravel/framework/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework. You can also check out [Laravel Learn](https://laravel.com/learn), where you will be guided through building a modern Laravel application.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains thousands of video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the [Laravel Partners program](https://partners.laravel.com).

### Premium Partners

- **[Vehikl](https://vehikl.com)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Curotec](https://www.curotec.com/services/technologies/laravel)**
- **[DevSquad](https://devsquad.com/hire-laravel-developers)**
- **[Redberry](https://redberry.international/laravel-development)**
- **[Active Logic](https://activelogic.com)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).



DOM=liezlkaye.me
APPD=/var/www/laravel
export DEBIAN_FRONTEND=noninteractive
apt update && apt upgrade -y
apt install -y php8.3-{fpm,mysql,mbstring,bcmath,intl,xml,curl,zip} unzip mysql-server nginx python3-certbot-nginx
mysql -e "CREATE DATABASE laravel;"
mysql -e "CREATE USER 'lzl'@'localhost' identified by 'lzl';"
mysql -e "grant all privileges on *.* to 'lzl'@'localhost';"
mysql -e "flush privileges;"

cat > /etc/nginx/sites-enabled/default <<EOF
server{
	listen 80;
	root $APPD/public;
	index index.php index.html;
	server_name $DOM www.$DOM;
	location / {
	try_files \$uri \$uri/ /index.php?\$query_string;
	}
	location ~ \.php$ {
	include snippets/fastcgi-php.conf;
	fastcgi_pass unix:/run/php/php8.3-fpm.sock;
	}
	location ~ /\.(?!well-known).*{
	deny all;
	}
}
EOF
systemctl reload nginx
curl -sS https://getcomposer.org/installer -o composer-setup.php 
php composer-setup.php --install-dir=/usr/local/bin --filename=composer
cp .env.example .env
sed -i "s/DB_CONNECTION=sqlite/DB_CONNECTION=mysql/" .env
sed -i 's/# DB_HOST=127.0.0.1/DB_HOST=127.0.0.1/' .env
sed -i 's/# DB_PORT=3306/DB_PORT=3306/' .env
sed -i 's/# DB_DATABASE=laravel/DB_DATABASE=laravel/' .env
sed -i 's/# DB_USERNAME=root/DB_USERNAME=lzl/' .env
sed -i 's/# DB_PASSWORD=/DB_PASSWORD=lzl/' .env
composer install --no-dev --optimize-autoloader --no-interaction
php artisan key:generate
php artisan migrate --force
chmod -R 775 storage bootstrap/cache
chown -R www-data:www-data $APPD
certbot --nginx -d $DOM -d www.$DOM --non-interactive --agree-tos --email lk@gmail.com


on:
  push:
    branches: [hehe]
jobs:
  d:
    runs-on: ubuntu-latest
    steps:
      - uses: appleboy/ssh-action@v1
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SERVER_SSH_KEY }}
          port: ${{ secrets.SERVER_PORT }}
          script: |
            git config --global --add safe.directory /var/www/laravel
            cd /var/www/laravel
            git pull origin hehe
            php artisan optimize:clear

            ssh-keygen -t rsa -b 4096 -C "github-actions"
            cat ~/.ssh/id_rsa.pub
            STEP 1 — Generate SSH key (on your LOCAL machine)
            STEP 2 — Add key to your SERVER
            STEP 3 — Copy PRIVATE key to GitHub
            STEP 4 — Add GitHub Secrets
            STEP 5 — Make sure repo exists on server
            STEP 6 — First-time Laravel setup (ONE TIME)
