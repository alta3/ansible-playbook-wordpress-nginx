# WordPress + Nginx Ansible Playbook
Ansible playbook and roles for installing WordPress + Nginx + PHP + Postfix server

### Requirements
- Ansible 2.0.0 or newer
- Ubuntu 16.04 (installed on your web server or virtual machine)

### Instructions:

0. You have properly booted a new server on the OpenStack cloud, which means you need to gather some information.
    * Write down the IP address of your server you just booted
    * Your ~/.ssh/id_rsa.pub key is trusted by your new server
    * Your user is ubuntu, which is a passwdless sudoer, but when you check visudo, the `user=ubuntu` is NOT there. however you notice that there is an include directory: `/etc/sudoers.d` at the bottom of the file
    * You check `/etc/sudoers.d` and you notice that there is a file called `90-cloud-init-users`.
    * You open `/etc/sudoers.d/90-cloud-init-users`
    * OK, so it looks like the "ansible-boxes" are checked, an RSA key is in place and ubuntu is a passwdless sudoer. You also have the IP address of your new server!

0. Edit your /etc/hosts file, and add a new host line like the one below EXCEPT replace `10.0.0.65 with the ip address of your new server. When your are finished, your new server will look like the /etc/hosts file below with the critical exception that the IP address will be your server, not `10.0.0.65`

    `student@beachhead:~$` `sudo vim /etc/hosts`

    `10.0.0.65 alta3.local`  
    
    ```
    127.0.0.1 beachhead localhost
    172.16.1.4 beachhead
    172.16.1.5 controller
    172.16.1.6 compute1
    172.16.1.7 compute2
    172.16.1.5 block1
    172.16.1.5 object1
    10.0.0.65 alta3.local

    # The following lines are desirable for IPv6 capable hosts
    ::1 ip6-localhost ip6-loopback
    fe00::0 ip6-localnet
    ff00::0 ip6-mcastprefix
    ff02::1 ip6-allnodes
    ff02::2 ip6-allrouters
    ff02::3 ip6-allhosts
    ```
    

o. Clone the repository

    ```
$ git clone https://github.com/tucsonlabs/ansible-wordpress-nginx-playbook.git
$ cd /wordpress-nginx
```

### 3. Set the web server IP address

Create a hosts file to set your web server's IP or move `hosts.example` to `hosts` if you're using vagrant:

```
mv hosts.example hosts
```

Change `192.168.100.10` to your server's URL or the IP address of your virtual machine:

```
[web-server]
192.168.100.10
```

### 4. Run the playbook

```
$ ansible-playbook playbook.yml -i hosts -u YOUR_REMOTE_USER_ID -K
```

This tells ansible to use the inventory file we've called "hosts". If you're using vagrant you can run the same command as above but exclude the username and sudo prompt:

```
$ ansible-playbook playbook.yml -i hosts
```

### 5. Finish the install

Open your web browser and navigate to [http://192.168.100.10](http://192.168.100.10) (or your webserver's IP) to finish the WordPress installation.
