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
0. Ping alta3.local and you must be back a positive response.

    `student@beachhead:~$`` ping alta3.local`
    
    ```
    PING alta3.local (10.0.0.65) 56(84) bytes of data.
    64 bytes from alta3.local (10.0.0.65): icmp_seq=1 ttl=62 time=2.25 ms
    64 bytes from alta3.local (10.0.0.65): icmp_seq=2 ttl=62 time=0.650 ms
    64 bytes from alta3.local (10.0.0.65): icmp_seq=3 ttl=62 time=0.676 ms
    64 bytes from alta3.local (10.0.0.65): icmp_seq=4 ttl=62 time=0.640 ms
    ```

0. Clone the repository

    `student@beachhead:~$` `git clone https://github.com/alta3/ansible-playbook-wordpress-nginx.git`
    
0. cd into the new directory

    `student@beachhead:~$` `cd ansible-playbook-wordpress-nginx/`

0. Edit the hosts file to point to alta3.local's IP address. An example of your hosts file is below, so just REAPLACE `10.0.0.65` with the IP address of your cloud host. 

    `student@beachhead:~/ansible-playbook-wordpress-nginx$` `vim hosts`  

    ```
    [webservers]
    alta3.local  ansible_host=10.0.0.65 ansible_ssh_user=ubuntu
    ```

 0. Let's run the playbook

    `student@beachhead:~/ansible-playbook-wordpress-nginx$` `ansible-playbook playbook.yml -i hosts`  
    

 0. Let's test our web page!  Open a browser to:
 
    `http://alta3.local`
    
 0. You should be looking at a wordpress config page after being automatically redirected to `http://alta3.local/wp-admin/install.php`
 
 0. Totally up to you, but if you got this far, let's configure your new web site.
 
     `Select ENGISH` and continue
     
 0. Set up credentuals    
 
     ```
     Site Title:  alta3 	
     Username:    ubuntu 	
     Password:    alta3 
     Confirm use of weak password:    check
     Your Email::  your spam email or whatever 	
     Search Engine Visibility: check
     ```
     
     

 
 
