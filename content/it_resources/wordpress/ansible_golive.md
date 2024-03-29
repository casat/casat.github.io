---
title: "Ansible: Staging to live"
weight: 13
date: "2022-08-24"
author: "John Marks"
---
08/24/22

These are the steps for Taking a WordPress website from staging, or 'dev', to live using Ansible.

## 1. Snapshots
- In the DigitalOcean interface create 'pre-migration' snapshots of both the current *'live'* server (soon to become the deprecated server) and the staging server

## 2. Update URL information on Staging
- Edit the Apache virtual host file (should be: /etc/apache2/sites-available/wordpress.conf) to reflect the actual URL in non-HTTPS (ie: **http**://casat.org)
- Update URL in the MySQL database
    - Run the wp_url.yml playbook
        - ```ansible-playbook -i staging.yml playbooks/wordpress/wp_url.yml --limit $DOMAIN```
        - **Before** running, update the URL variables in the wp_clone.yml variables file, found in /etc/ansible/playbooks/secrets/..
    - OR connect to the server and use wp-cli
        - ```wp search-replace $OLD_URL $NEW_URL```
- Remove/disable Apache basic auth *(if enabled)*
    - Run the wp_password_off.yml playbook
       - ```ansible-playbook -i staging.yml playbooks/wordpress/wp_password_off.yml --limit $DOMAIN```

## 3. Update DNS records
- Within DigitalOcean, in the *Networking* section, under *Domains*, find the relevant domain and update the **A records** that point to both the non-www URL and the www.URL and update them to point to the staging server
- At this time you should turn off the 'old' live site
    - Do this **only** by connecting to the server via its **IP address**, not via URL OR through the DigitalOcean interface. If you connect to a server via URL at this time it can lead you to either the old or the new server, so only connect via IP address.

## 4. Obtain an SSL certificate and apply it
- Run ssl_get.yml playbook to obtain certificate
    - ```ansible-playbook -i staging.yml playbooks/ssl/ssl_get.yml --limit $DOMAIN```
    - **Before** running, make sure to update the '- domains:' section of the ssl_info.yml variable file, found in /etc/ansible/playbooks/secrets/..
- Run wp_ssl_on.yml playbook **on production** to apply certificate
    - ```ansible-playbook -i production.yml playbooks/wordpress/wp_ssl_on.yml --limit $DOMAIN```
    - Make sure to run this command on the 'production' host file, and not on 'dev' or 'staging'. If the command is run on dev or staging it will result in the Apache virtual host file referencing the wrong URL (ie: dev.casat.org instead of casat.org). Use the ```--limit``` tag to restrict the playbook to a single target.

## 4. Update the MySQL database URL references to HTTPS
- Run wp_url.yml playbook
    - ```ansible-playbook -i staging.yml playbooks/wordpress/wp_url.yml --limit $DOMAIN```
    - **Before** running, update the URL variables in the wp_clone.yml variables file, found in /etc/ansible/playbooks/secrets/..
- OR connect to the server and use wp-cli

## 5. Install monitoring agents on server
- Run the playbooks/monitoring/monitor.yml playbook
- Add the staging server IP address to the panopticon server firewall 'allowed' list
    - ```sudo ufw allow from $IP proto tcp to any port 3100```
- Remove the old server IP address from the panopticon server firewall 'allowed' list
    - ```sudo ufw status numbered```
    - ```sudo ufw delete #```

## 6. Update DigitalOcean
- Move deprecated server to the 'Retired Sites' section
    - add '-OLD' to the end of the server name to avoid any confusion
- Rename the new live server and move it into the correct group
- Cleanup any unnecessary DNS entries
- Create 'post-migration' snapshot of the new live server