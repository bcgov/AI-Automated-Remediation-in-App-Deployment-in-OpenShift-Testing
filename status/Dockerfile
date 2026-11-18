# Internal status service (PHP). Pinned to an older patch release on purpose:
# the AI remediation workflow is expected to bump this tag.
FROM php:8.4.27-apache

# Listen on an unprivileged port; OpenShift runs the container with a random UID in GID 0
RUN sed -i 's/Listen 80$/Listen 8080/' /etc/apache2/ports.conf && \
    sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8080>/' /etc/apache2/sites-available/000-default.conf && \
    echo 'ServerName localhost' > /etc/apache2/conf-available/servername.conf && \
    a2enconf servername && \
    chgrp -R 0 /var/www/html && chmod -R g=u /var/www/html

COPY src/ /var/www/html/

EXPOSE 8080
USER 1001

HEALTHCHECK --interval=30s --timeout=3s CMD curl -fsS http://127.0.0.1:8080/healthz.php || exit 1
