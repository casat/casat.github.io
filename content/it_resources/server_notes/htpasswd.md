---
title: "Adding logins with Apache"
weight: 2
author: "JMarks"
categories: [apache]
tags: [apache, htpasswd, access, acl, basic auth, password]
---
---

Following these directions will add a username and password prompt to a website or specific webpage. Typically we use this to hide a 'Dev' website so it can still be accessible but not open to the public.

## Using htpasswd to generate users and passwords

1. Install Dependencies
Make sure you have Apache Extended Utilities installed
```
sudo apt update
sudo apt install apache2-utils
```

2. Create first user with htpasswd
```
sudo htpasswd -c /etc/apache2/htpasswd.users $USER_NAME
```
- Immediately after running this command you will be prompted to define the password for the user

{{< hint info >}}
**Additional Users**\
When creating additional users after the first for the same ACL (user list) you need to drop the -c flag from the command.\
ie: sudo htpasswd /etc/apache2/htpasswd.users $USER_NAME
{{< /hint >}}

## Applying the htpasswd ACL with .htaccess files (WordPress)

Follow these directions when working with a website that has an .htacces file (WordPress websites)

3. Add the following section to the end of your .htaccess file
```
## BEGIN passwd
AuthType Basic
AuthName "Authorized Users"
AuthUserFile /etc/apache2/htpasswd.users
Require valid-user
## END passwd
```

4. Reset the Apache service to get the changes to take affect
```
sudo systemctl restart apache2.service
```

## OR applying the htpasswd ACL with Apache VirtualHost files

You should follow these directions if the website doesn't have an .htaccess file.

3. Add the following section to your Apache VirtualHost file (.conf file), within the directory block
```
AuthType Basic
AuthName "Authorized Users"
AuthUserFile "/etc/apache2/htpasswd.users"
Require valid-user
```

4. Reset the Apache service to get the changes to take affect
```
sudo systemctl restart apache2.service
```

{{< hint warning >}}
**Not Seeing the login**\
If you have recently visited/viewed the website you are updating, you will only see the change after you clear your cache or use an 'incognito window'.
{{< /hint >}}
