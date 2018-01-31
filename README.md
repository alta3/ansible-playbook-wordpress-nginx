# WordPress + Nginx Ansible Playbook
Ansible playbook and roles for installing WordPress + Nginx + PHP + Postfix server

### Requirements
- Ansible 2.0.0 or newer
- Ubuntu 16.04 (installed on your web server or virtual machine)

0. You have properly booted a new server on the OpenStack cloud, which means you need to gather some information.
    * Write down the IP address of your server you just booted
    * Your ~/.ssh/id_rsa.pub key is trusted by your new server
    * Your user is ubuntu, which is a passwdless sudoer, but when you check visudo, the `user=ubuntu` is NOT there. however you notice that there is an include directory: `/etc/sudoers.d` at the bottom of the file
    * You check `/etc/sudoers.d` and you notice that there is a file called `90-cloud-init-users`.
    * You open `/etc/sudoers.d/90-cloud-init-users`
    * OK, so it looks like the "ansible-boxes" are checked, an RSA key is in place and ubuntu is a passwdless sudoer. You also have the IP address of your new server!

### Instructions:

0. Edit /etc/hosts file, and add a new host line like the one below EXCEPT replace `10.0.0.65 with the ip address of your new server. When your are finished, your new server will look like the /etc/hosts file below with the critical exception that the IP address will be your server, not `10.0.0.65`

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
    
Allow connections from your development machine to the web server over ssh. This is essential for ansible to work, so make sure to configure your remote or local server to allow connections via ssh. You may find `ssh-copy-id` helpful. 

You can skip the step below if you're not using vagrant and replace `192.168.100.10` with your websever's IP address, but make sure you're able to SSH into your web server before continuing.

If you're using vagrant add these lines to your **Vagrantfile**:

```
config.vm.network :forwarded_port, guest: 80, host: 4567
config.vm.network "private_network", ip: "192.168.100.10"
```

This allows a connection to the machine over ssh on the specified IP address. `"192.168.100.10"` can be swapped out for a different IP, but make sure it matches whatever is set in your ansible inventory file. 

Run `vagrant ssh-config` to see where your key is stored, and create or update your host machine's `~/.ssh/config` file. It should looks something like this with your IdentityFile switched out:

```
Host 192.168.100.10
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
  IdentitiesOnly yes
  User vagrant
  IdentityFile /Users/joshua/Boxes/bento/ubuntu-16.04/.vagrant/machines/default/virtualbox/private_key
  PasswordAuthentication no
```


**A note about ubuntu 16.04**

For whatever reason, the Ubuntu team is not following the standard vagrant box configuration settings so you're better off either creating a new box, or using an independently packaged one like [https://atlas.hashicorp.com/bento/boxes/ubuntu-16.04].

Verify that you're able to ssh into the machine:

`ssh vagrant@192.168.100.10`

### 2. Clone the repository

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
