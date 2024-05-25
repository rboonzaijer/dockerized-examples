FROM usethis/php-nginx:8.3

# Switch temporarily to root to get installation permissions
USER root

# Copy the symfony nginx config
COPY ./_docker/conf.d/ /etc/nginx/conf.d/

# Copy all files
COPY . /var/www/html/

# Add build tools
RUN apk add --no-cache --update \
    composer

RUN composer install --no-dev --no-interaction --prefer-dist --optimize-autoloader

# Remove build tools again
RUN apk del \
    composer

# Set permissions of folders/files
RUN chown -R nobody:nobody /var/www/html
RUN find /var/www/html -type d -exec chmod 700 {} \;
RUN find /var/www/html -type f -exec chmod 600 {} \;

# Switch to rootless user
USER nobody

# Start supervisord
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
