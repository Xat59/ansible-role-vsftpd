# Description
Install and configure vsftpd FTP server.

Features :

- local users
- chrooting
- unsecure / tls v1
- several vsftpd instance

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
  
* **vsftpd_ftp_banner** : Greeting banner when a connection first comes in
  * required : No
  * default value : Private FTP server 
  
* **vsftpd_passive_min_port** : Minimum port number for data connection <br /> **Note** : useful for firewall configuration
  * required : No
  
* **vsftpd_passive_max_port** : Maximum port number for data connection <br /> **Note** : useful for firewall configuration
  * required : No
  
* **vsftpd_passive_address** : IP address for connection
  * required : No 
  
* **vsftpd_ssl_enabled** : Enable or disable SSL support
  * required : No
  * default value : false
  * choices : true or false
  
* **vsftpd_ssl_privkey** : Path to the SSL key certificate
  * required : if vsftpd_ssl_enabled is 'True'
  
* **vsftpd_ssl_certificate** : Path to the SSL certificate
  * required : if vsftpd_ssl_enabled is 'True'

* **vsftpd_ssl_implicit** : Enable or disable implicit TLS <br /> If **enabled**, an SSL handshake is the first thing expect on all connections (FTPs) <br /> If **disabled**, explicit TLS is enabled (FTPes)
  * required : No
  * default value : true
  * choices : true or false

* **vsftpd_systemd_service_name** : Name of the vsftpd instance <br /> **Note** : the vsftpd configuration file and systemd service file will be inherited from this name <br /> **Example** with vsftpd_systemd_service_name set to 'vsftpd-implicit', the configuration file will be /etc/vsftpd/vsftpd-implicit.conf and systemd service file will be vsftpd@vsftpd-implicit.service
  * required: No
  * default value : vsftpd


# Usage

- Unsecure FTP

```
---
- hosts: host01
  gather_facts: yes
  become: yes
    - role: vsftpd
```

- Secure FTP with explicit TLS (FTPes)

```
---
- hosts: host01
  gather_facts: yes
  become: yes
    - role: vsftpd
    vsftpd_ssl_enabled: true
    vsftpd_ssl_privkey: /etc/vsftpd/ssl/vsftpd.key
    vsftpd_ssl_certificate: /etc/vsftpd/ssl/vsftpd.crt
    vsftpd_ssl_implicit: false
```

- Secure FTP with implicit TLS (FTPs)

```
---
- hosts: host01
  gather_facts: yes
  become: yes
    - role: vsftpd
    vsftpd_ssl_enabled: true
    vsftpd_ssl_privkey: /etc/vsftpd/ssl/vsftpd.key
    vsftpd_ssl_certificate: /etc/vsftpd/ssl/vsftpd.crt
    vsftpd_ssl_implicit: true
```


# Contribute

## Roadmap

- virtual users creation
