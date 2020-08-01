# Wordpress installation

## Prerequisits
In case you are using Wordpress behind an Apache2 Webserver, you'll find a configuration file below

#### 1) apache2 configuration
You have to place it in "/etc/apache2/sites-available/wordpress.conf".

```bash
<VirtualHost *:80>
    ServerName www.wordpress.example.com
    ServerAlias wordpress.example.com

    ServerAdmin root@example.com

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Now you have to activate the apache2 site:

```bash
root@server ~ # a2ensite wordpress
root@server ~ # systemctl restart apache2
```

#### 2) Certbot installation
To use Letsencrypt with Certbot, you have to install it first:

```bash
root@server ~ # apt update && apt install certbot python-certbot-apache
root@server ~ # certbot --apache
```

Now you chose your Domainname (withour the "www").
After that, you have to change the VirtualHost file added by Certbot:

```bash
<IfModule mod_ssl.c>
<VirtualHost *:443>
    ServerName www.wordpress.example.com
    ServerAlias wordpress.example.com

    ServerAdmin root@example.com

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    SSLProxyEngine on
    SSLProxyVerify none
    SSLProxyCheckPeekCN off
    SSLProxyCheckPeerName off
    SSLProxyCheckPeerExpire off

    ProxyPass / https://127.0.0.1:1443/
    ProxyPassReverse / https://127.0.0.1:1443/

SSLCertificateFile /etc/letsencrypt/live/<yourdomain>/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/<yourdomain>/privkey.pem
Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
</IfModule>
```

## Docker-Compose
Now you can simply start and create the Container by typing the following command. Be careful: docker-compose searches for the docker-compose.yml file in the current Folder!

```bash
root@server ~ # cd docker/wordpress/
root@server ~ # docker-compose up --build
```

## Further information
After you build your container once, you can simply start it with the docker-compose start / stop command.
The only thing you have to note is, that docker-compose searches for the docker-compose.yml in the current folder.

Three useful commands:
```bash
root@server ~ # docker-compose start
root@server ~ # docker-compose stop
root@server ~ # docker-compose logs
```

You can find all volumes in "/var/lib/docker/volumes/".