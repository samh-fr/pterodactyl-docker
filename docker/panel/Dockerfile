FROM ubuntu/nginx:1.18-22.04_beta

RUN apt-get update \
    && apt-get -y install \
    software-properties-common curl apt-transport-https ca-certificates gnupg \
    && DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get -y install php8.1 php8.1-cli php8.1-gd php8.1-mysql php8.1-pdo php8.1-mbstring php8.1-tokenizer php8.1-bcmath php8.1-xml php8.1-fpm php8.1-curl php8.1-zip tar unzip git

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN mkdir -p /var/www/pterodactyl \
    && cd /var/www/pterodactyl \
    && curl -Lo panel.tar.gz https://github.com/pterodactyl/panel/releases/latest/download/panel.tar.gz \
    && tar -xzvf panel.tar.gz \
    && chmod -R 755 storage/* bootstrap/cache/

WORKDIR /var/www/pterodactyl

RUN cp .env.example .env \
    && composer install --no-dev --optimize-autoloader \
    && php artisan key:generate --force

COPY nginx-pterodactyl.conf /etc/nginx/conf.d/pterodactyl.conf
