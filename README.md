# Description
Install and configure vsftpd FTP server.

Features :

- local users
- chrooting
- unsecure / tls v1

Does not cover :

- SSL certificate and key creation


# Role installation

```
$ git clone https://github.com/Xat59/ansible-vsftpd
```

# Variables

* **vsftpd_enable_local_users** : Enable local users connection
  * required : No 
  * default value : true
  * choices : true or false
* **vsftpd_chroot_local_users** : Enable chrooting for local users <br /> **Note** : chrooting users needs the user to be chrooted in a valid (owner and mode) chroot path
  * required : No 
  * default value : false
  * choices : true or false


| vsftpd_ftp_banner | No | Private FTP server | | Greeting banner when a connection first comes in
| vsftpd_passive_min_port | No | | | Minimum port number for data connection <br /> **Note** : useful for firewall configuration
| vsftpd_passive_max_port | No | | | Maximum port number for data connection <br /> **Note** : useful for firewall configuration
| vsftpd_passive_address | No | | | IP address for connection
| vsftpd_ssl_enabled | No | false | true or false | Enable or disable SSL support
| vsftpd_ssl_privkey | No | | | Path to the SSL key certificate <br /> **Required** : if vsftpd_ssl_enabled is 'True'
| vsftpd_ssl_certificate | No | | | Path to the SSL certificate <br /> **Required** : if vsftpd_ssl_enabled is 'True'

# Usage

- Unsecure FTP

```
---
- hosts: host01
  gather_facts: yes
  become: yes
    - role: vsftpd
```

- Secure FTP with explicit TLS

```
---
- hosts: host01
  gather_facts: yes
  become: yes
    - role: vsftpd
    vsftpd_ssl_enabled: true
    vsftpd_ssl_privkey: /etc/vsftpd/ssl/vsftpd.key
    vsftpd_ssl_certificate: /etc/vsftpd/ssl/vsftpd.crt
```

# Contribute

## Roadmap

- virtual users creation
