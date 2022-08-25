---
title: "Ansible: Migrating WordPress"
weight: 13
date: "2022-08-25"
author: "John Marks"
---
08/25/2022

Here are the steps for copying one WordPress website to another using Ansible. Prior to working through these steps you should have already created a 'blank' WordPress website using either the Ansible method or the Bash method and have a snapshot of the droplet.

## 1. Create a backup of the website you wish to copy/migrate
- Run the wp_backup.yml playbook
    - ```ansible-playbook -i production.yml playbooks/wordpress/wp_backup.yml --limit $DOMAIN```
    - wp_playbook.yml is okay to run on all servers, but should be run with the ```--limit``` tag limit it just the target we need to copy
    - backups created with the playbook will be saved on the bastion.casat.org server to /var/wp_backups/...
    - *Note that running this playbook will remove any saved backups older than 30 days for the same server, so you may need to move any copies you wish to save prior to running*

## 2. Copy the backup data to the new server
- Update the wp_clone.yml variables file saved in /etc/ansible/playbooks/secrets/wp_clone.yml
    - Make sure the server_name and backup_file variables match with files on the bastion server
    - Pay attention to the URL variables, particularly that the old URL is likely (but not always) HTTPS and the new URL is likely (but not always) HTTP
- Pay attention to where you are running this playbook, the prior command was run on the production host file this command will likely need to be run on the dev host file

## 3. Verify and save
- Go to the URL you specified and check that the website is available
    - If the website fails to load:
        - Check your URL variables in the wp_clone.yml variables file
        - Connect to the new server and verify the URL in the MySQL database with wp-cli
        - Check that the server has enough memory to support the website (may need to 'resize' the droplet in DigitalOcean)
- If the website checks out (errors due to no HTTPS are okay for now) create a snapshot in DigitalOcean to save your work