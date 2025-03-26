# Usa una imagen base de PHP con Apache
FROM php:8.2-apache

# Instala las extensiones necesarias para Laravel
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    && docker-php-ext-install pdo pdo_mysql

# Habilita el módulo de reescritura de Apache
RUN a2enmod rewrite

# Copia los archivos del proyecto al contenedor
COPY . /var/www/html

# Establece los permisos correctos para el directorio de almacenamiento
# Crea los directorios si no existen
RUN mkdir -p /var/www/html/storage /var/www/html/bootstrap/cache \
    && chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# Establece el directorio de trabajo
WORKDIR /var/www/html

# Expone el puerto 80
EXPOSE 80

# Instala Composer
COPY --from=composer:2.6 /usr/bin/composer /usr/bin/composer

# Ejecuta Composer para instalar las dependencias
RUN composer install --no-dev --optimize-autoloader

# Configura el archivo .env
RUN cp .env.example .env && php artisan key:generate

# Comando por defecto para iniciar el servidor
CMD ["apache2-foreground"]