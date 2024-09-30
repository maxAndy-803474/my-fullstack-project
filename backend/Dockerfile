FROM php:8.2-fpm

# Встановлення необхідних розширень
RUN docker-php-ext-install pdo pdo_mysql

# Встановлення wget для завантаження Composer
RUN apt-get update && apt-get install -y wget

# Встановлення Composer
RUN wget https://getcomposer.org/installer -O composer-setup.php && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    rm composer-setup.php

# Копіювання файлів
COPY . /var/www/html

# Встановлення прав доступу
RUN chown -R www-data:www-data /var/www/html

# Встановлення пакету `supervisor`, щоб управляти `php artisan serve`
RUN apt-get update && apt-get install -y supervisor

# Копіюємо конфігурацію supervisor
COPY supervisor.conf /etc/supervisor/conf.d/supervisor.conf

# Порт, на якому слухатиме Laravel
EXPOSE 9000

# Запускаємо supervisor
CMD ["/usr/bin/supervisord"]
