---
tags:
- Nginx
date: 2023-08-09
title: nginx password protection
categories:
lastMod: 2025-10-12
--- 
[How to password protect a directory with Nginx authentication - nixCraft](https://www.cyberciti.biz/faq/nginx-password-protect-directory-with-nginx-htpasswd-authentication/)

To generate passwd file, we need to use `apache2-utils` (on Debian/Ubuntu).

```shell
> sudo apt install apache2-utils
> htpasswd -c /path/to/passwd-file <username>
```

Then htpasswd will prompt for password. A passwd file can contain several pairs of username-password. When using an existing passwd file, omit `-c`.

Put the auth-related code into the code block you want to protect, for example, the following protects `/app/` directory when accessing `foo.domain.tld`:

```nginx
location /app/ {
    server_name    "foo.domain.tld"
    auth_basic          "Restricted Content";
    auth_basic_user_file /path/to/passwd-file;
}
```
 