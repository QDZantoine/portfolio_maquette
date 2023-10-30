# portfolio_maquette

#Deployment on ct-antoine:

antoine@ct-antoine:~$ sudo nano /etc/apache2/sites-enabled/ct-antoine.conf

<!-- MODIFY NANO -->

#

# /opt/git/antoine/antoine-project/cmd/sites-available.default_ct-antoine.conf

#

# git -C /opt/git/antoine/antoine-project pull origin master; sudo cp /opt/git/antoine/antoine-project/cmd/sites-available.default_ct-antoine.conf /etc/apach>

#

# sudo nano /etc/apache2/sites-available/ct-antoine.conf

#

<VirtualHost \*:80> # Haproxy Forward Client’s IP address to Backend
RemoteIPProxyProtocol On
RemoteIPHeader X-Forwarded-For
RemoteIPTrustedProxy 127.0.0.1

        # Use HTTP Strict Transport Security (HSTS) to force client to use secure connections only
        <IfModule mod_headers.c>
                Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
        </IfModule>

        <IfModule mod_expires.c>
                <FilesMatch "\.(ico|jpe?g|png|gif|svg|webp|js|css|otf|json|woff2)$">
                        ExpiresActive On
                        ExpiresDefault "access plus 1 month"
                </FilesMatch>
        </IfModule>

        <FilesMatch \.php$>
                SetHandler "proxy:unix:/run/php/php8.2-fpm.sock|fcgi://localhost"
        </FilesMatch>

        <IfModule mod_deflate.c>
                SetOutputFilter DEFLATE

</IfModule>

        ServerSignature Off
        AddDefaultCharset UTF-8
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        ServerName antoine.opale-concept.com
        ServerAdmin contact@opale-concept.com
        DirectoryIndex index.html

        DocumentRoot /opt/git/antoine/portfolio_maquette
        <Directory /opt/git/antoine/portfolio_maquette>
                Require all granted
                AllowOverride None
                FallbackResource /index.html
        </Directory>

</VirtualHost>
<!-- SAVE NANO AND EXIT -->
antoine@ct-antoine:~$ sudo systemctl restart apache2
<!-- LINK FOR ACCESS -->
https://antoine.opale-concept.com/
