# Description
Install and configure vsftpd FTP server.

Features :

- local users
- virtual users
- chrooting
- unsecure / tls v1
- explicit and implicit TLS
- several vsftpd instance

Does not cover :

- SSL certificate and key creation


# Role installation

```
$ git clone https://github.com/Xat59/ansible-role-vsftpd
```

# Variables

* **vsftpd_enable_local_users** : Enable local users connection <br /> **Note** : If **vsftpd_enable_virt_users** is 'true', the value of this variable will be overwrite to 'true'
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

* **vsftpd_systemd_service_name** : Name of the vsftpd instance <br /> **Note** : If **redefined**, the vsftpd configuration file and systemd service file will be inherited from this name. If **not redefined**, the configuration fil and systemd service file will keep their default values. <br /> **Example** with vsftpd_systemd_service_name set to 'vsftpd-implicit', the configuration file will be /etc/vsftpd/vsftpd-implicit.conf and systemd service file will be vsftpd@vsftpd-implicit.service.
  * required: No
  * default value : vsftpd

* **vsftpd_guest_username** : A guest (all non-anonymous) login is remapped to the real user specified in this setting.
  * required: No

* **vsftpd_enable_virt_users** : Enable virtual users on the vsftpd instance <br /> **Note**: setting this variable to 'true' will overwrite the vsftpd_chroot_local_users variable to 'true'.
  * required : No. But, if you have to define virtual users via vsftpd_virt_users, you must set vsftpd_enable_virt_users to 'True'.
  * default value : false
  * choices : true or false

* **vsftpd_no_log** : Disable logging of tasks that handle sensitive information
  * required : No.
  * default value : true
  * choices : true or false

* **vsftpd_virt_users** : List of enabled virtual users with per-user parameter overwrites
  * required: No
 
    **Per-user available parameters** : 
      * username : current virtual user username
        * required : Yes
      * password : current virtual user password
        * required : Yes
      * local_root : current virtual user home directory
        * required : No
      * write_enable : current virtual user write permission
        * required : No
      * guest_username : current virtual user remapping to the specified local user
        * required : No

    **Example**: see examples below.


# Usage

- Unsecure FTP

```
---
- hosts: host01
  gather_facts: yes
  become: yes
    - role: ansible-role-vsftpd
```

- Secure FTP with explicit TLS (FTPes)

```
---
- hosts: host01
  gather_facts: yes
  become: yes
    - role: ansible-role-vsftpd
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
    - role: ansible-role-vsftpd
      vsftpd_ssl_enabled: true
      vsftpd_ssl_privkey: /etc/vsftpd/ssl/vsftpd.key
      vsftpd_ssl_certificate: /etc/vsftpd/ssl/vsftpd.crt
      vsftpd_ssl_implicit: true
```

- Unsecure FTP with virtual users

```
---
- hosts: host01
  gather_facts: yes
  become: yes
    - role: ansible-role-vsftpd
      vsftpd_enable_virt_users: true
      vsftpd_virt_users:
        - username: xat
          password: xat
          guest_username: www-data
          local_root: /var/www/
          write_enable: yes
        - username: jdoe
          password: jdoe
          guest_username: www-data
          local_root: /var/www
          write_enable: no
```


# Contribute

## Roadmap

