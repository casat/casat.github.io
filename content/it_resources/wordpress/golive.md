---
title: "Staging to live with Ansible"
weight: 10
date: "2022-08-24"
author: ""
description: ""
categories: [wordpress]
tags: [wordpress, migration, mysql, wp-cli]
---
08/24/22

Here are the steps for Taking a WordPress website live using Ansible. This process should be used when needing to move a WordPress website from *Staging* to *Production*.

## 1. Snapshots
- In the DigitalOcean interface create 'pre-migration' snapshots of both the current *'live'* server (soon to become the deprecated server) and the staging server

## 2.Update URL information on Staging
- Edit the Apache virtual host file (should be: /etc/apache2/sites-available/wordpress.conf) to reflect the actual URL in non-HTTPS (ie: **http**://casat.org)
- Update URL in the MySQL database
    - Run the wp_url.yml playbook
- Remove/disable Apache basic auth (if enabled)
    - Run the wp_password_off.yml playbook

## 3. Update DNS records
- Within DigitalOcean, in the *Networking* section, under *Domains*, find the relevant domain and update the **A records** that point to both the non-www URL and the www.URL and update them to point to the staging server
- At this time you should turn off the 'old' live site
    - Do this ***only*** by connecting to the server via its **IP address**, not via URL OR through the DigitalOcean interface. If you connect to a server via URL at this time it can lead you to either the old or the new server, so only connect via IP address.

## 4. Obtain an SSL certificate and apply it
- Run ssl_get.ynl playbook to obtain certificate
- Run wp_ssl_on.yml playbook **on production** to apply certificate
    - Make sure to run this command on the 'production' host file, and not on 'dev' or 'staging'. If the command is run on dev or staging it will result in the Apache virtual host file referencing the wrong URL (ie: dev.casat.org instead of casat.org)

## 4. Update the MySQL database URL references to HTTPS
- Run wp_url.yml playbook
- OR use the wp-cli tool on the server
    - ```wp search-replace $OLD_URL $NEW_URL```

## 5. Install monitoring agents on droplet
- Run the monitoring.yml playbook
- Add the staging server IP address to the panopticon server firewall 'allowed' list
    - ```sudo ufw allow from $IP proto tcp to any port 3100```
- Remove the old server IP address from the panopticon server firewall 'allowed' list

## 6. Update DigitalOcean
- Move deprecated droplet to the 'Retired Sites' section
- Rename the new live droplet and move it into the correct group
- Cleanup any unnecessary DNS entries