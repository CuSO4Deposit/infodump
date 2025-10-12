---
tags:
- Linux
date: 2023-07-17
title: configure sftp on ubuntu
categories:
lastMod: 2025-10-12
--- 
Assume SSH is already set up, sftp is easy to configure. It uses the same port as SSH.

Modify `/etc/ssh/sshd_config`, uncomment this line:

```
Subsystem      sftp    /usr/lib/openssh/sftp-server
```

add the following under this line:

```
Match User <username>
        ChrootDirectory <dir>
        X11Forwarding no
        AllowTcpForwarding no
        ForceCommand internal-sftp
```

where \<username\> is the **fullname** (not username) of the user. \<dir\> is the most upper directory this user can access. Then restart service, on ubuntu:

```shell
> sudo systemctl restart ssh
```

When login on ftp clients, the login username is the **username** (not fullname) of the configured user.
 