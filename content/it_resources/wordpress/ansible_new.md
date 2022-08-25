---
title: "Ansible: Setting up WordPress"
weight: 11
date: "2022-08-25"
author: ""
description: ""
categories: [wordpress]
tags: [wordpress, install, mysql, wp-cli]
---

08/25/2022

## 1. Create droplet in DigitalOcean
- Select an Ubuntu 20.04LTS droplet
    - Go with the cheapest rate to get started and upgrade later
    - Datacenter - SF3
- Add SSH keys
- Enable Monitoring
- **Set an accurate hostname** - ie: casat.org, or dev.casat.org. Do not use names like 'Tamale' or 'WordPress'. The hostname set here will be referenced by Ansible later and setting to something other than the Domain name can cause issues later on.
- Make sure droplet is created into the 'Development Sites' group

## 2. Configure DNS
- In the 'Networking' section of DigitalOcean, under 'Domains' find the relevant domain for the website you are working on and then select it
- You will need to update the A Records for the domain to point to the new droplet, or create new ones if they do not already exist
    - There should be two A Records one for domain.com and another for www.domain.com, both should be pointing to the same location
    - If the A Records do not already exist you need to create them, one for '@' and another for 'www'
    - **Pay attention** to the TTL times set, the time it takes for these changes to propagate is equal to the TTL time. Once this changes are made you need wait until the TTL time has renewed for the changes to take effect. If they are set to a high time, this could be hours.
        - In same cases it is worthwhile to update the DNS A Record TTL times to a shorter time (I use 300) prior to starting this so you don't have to wait

## 3. Configure server and install WordPress
### With Ansible
- Use SSH to connect to the Ansible server - bastion.casat.org
- Add the new droplet's FQDN to the dev.yml host file
- Run the wp_new_server.yml playbook
    - ```ansible-playbook -i dev.yml playbooks/wordpress/wp_new_server.yml --limit $DOMAIN```

### Without Ansible
- Create user accounts on the server and update SSH keys for each
- Run server updates and restart
- Update server settings, apply STIG security settings, and enable firewall
- Install PHP and the necessary add-ons for WordPress
- Install MySQL, complete initial setup
    - Create database and user for WordPress
- Install Apache
    - Create Apache virtual host file
    - update firewall to allow Apache connections
- Install WordPress
    - Download latest version of WordPress
    - Create necessary folders and set permissions
    - Update and configure wp-config.php and .htaccess files

*Find more information on the [New WordPress server with Bash](../bash_new) wiki page*

## 4. Complete WordPress Install
- Navigate to the URL for the new website and complete the WordPress installation
    - This must be done prior to moving on to other tasks like using the wp_restore.yml playbook