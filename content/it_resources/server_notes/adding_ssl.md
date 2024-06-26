---
title: "Adding SSL certificate to Apache"
weight: 1
author: "JMarks + BAnderson"
categories: [apache]
tags: [apache, ssl, https, certbot]
---
## Obtain certificate from Ansible Role: Certbot (for Let's Encrypt)
- Run ssl_get.yml playbook to obtain certificate
    - ```ansible-playbook -i staging.yml playbooks/ssl/ssl_get.yml --limit $DOMAIN```
    - **Before** running, make sure to update the '- domains:' section of the ssl_info.yml variable file, found in /etc/ansible/playbooks/secrets/..
- Run wp_ssl_on.yml playbook **on production** to apply certificate
    - ```ansible-playbook -i production.yml playbooks/wordpress/wp_ssl_on.yml --limit $DOMAIN```
    - Make sure to run this command on the 'production' host file, and not on 'dev' or 'staging'. If the command is run on dev or staging it will result in the Apache virtual host file referencing the wrong URL (ie: dev.casat.org instead of casat.org). Use the ```--limit``` tag to restrict the playbook to a single target.
---
*Adapted from the [Certbot website](https://certbot.eff.org/lets-encrypt)*

## Obtain a certificate using Certbot
The Certbot app recommends using the 'snap' package manager for installation. First we'll make sure Snapd is installed and updated to handle the Certbot app installation. We then install the app, confirm that the command is correctly ['linked'](http://manpages.ubuntu.com/manpages/xenial/man1/ln.1.html) and then we'll request a certificate.

1. Update snapd
```
sudo snap install core; sudo snap refresh core
```

2. Install Certbot app
```
sudo snap install --classic certbot
```

3. Prepare the Certbot command
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

4. If Apache is currently running, disable it so Certbot can use the port
```
sudo systemctl stop apache2.service
```

5. Test Certbot
```
sudo certbot certonly --dry-run
```
- when prompted, select 'Spin up a temporary webserver'
- when entering the domain name, you can list multiple targets
    - ie: casat.org, www.casat.org, *.casat.org

6. If the test run passes without errors you should be good to run the 'live' command
```
sudo certbot certonly
```  
{{< hint info >}}
**Default certificate location**\
If you followed the above commands the certificates should be saved to the certbot default location:\
/etc/letsencrypt/live/$DOMAIN_NAME/
{{< /hint >}}


## Create new Apache Vhost file for SSL

For Apache to serve HTTPS content we must update the VirtualHost file with support for connections over port 443, and references to the locations of the SSL certificates. For this we will keep our existing VirtualHost file in tact and create a new .conf that supports SSL connections instead.

1. Create new Vhost .conf file
```
sudo vim /etc/apache2/sites-available/$DOMAIN_NAME-ssl.conf
```
So in practice that location should read something like: /etc/apache2/sites-available/casat-ssl.conf

2. Fill in Vhost information\
Make sure to fill in the variables with the correct information. You need to replace all instances of $FQDN with the correct domain name and then check that the 'DocumentRoot' points to the correct root folder for the website.
```
<VirtualHost *:80>
ServerName  $FQDN
ServerAlias www.$FQDN
Redirect permanent / https://$FQDN/
</VirtualHost>

<VirtualHost *:443>
ServerAdmin webmaster@casat.org
DocumentRoot /var/www/wordpress/
ServerName  $FQDN
ServerAlias www.$FQDN
SSLEngine on
SSLCertificateFile    /etc/letsencrypt/live/$FQDN/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/$FQDN/privkey.pem
    <Directory /var/www/wordpress/>
    Options FollowSymLinks
    AllowOverride All
    Require all granted
    </Directory>

ErrorLog  ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

3. Enable SSL support in Apache
```
sudo a2enmod ssl
```

4. Disable any current Vhost files and enable the new SSL file
```
sudo a2dissite $CURRENT_VHOST.conf
sudo a2ensite $NEW_VHOST-ssl.conf
```

5. Check that Apache doesn't have any problems with your syntax in the Vhost file
```
sudo apache2ctl configtest
```

6. Restart the Apache service to activate the changes
```
sudo systemctl restart apache2.service
```
