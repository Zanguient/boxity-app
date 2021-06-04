web: heroku-php-apache2
scheduler: php -d memory_limit=512M artisan schedule:daemon
release: php artisan migrate --force && php artisan db:seed && php artisan optimize:clear && php artisan cache:clear
