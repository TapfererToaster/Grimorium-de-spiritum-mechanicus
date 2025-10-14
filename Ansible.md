# Vagrant
A login into the VM with `vagrant ssh` lets you interact with the Bash shell, but Ansible needs to connect to the VM via regular SSH. 
To achieve this you need to output the SSH configuration of Vagrant:
```
$ vagrant ssh-config
```

The output could look like this, although the important lines are the `hostname`, `user`, `port` and `IdentityFIle`
```
Host default 
HostName 127.0.0.1 
User vagrant 
Port 2222 
UserKnownHostsFile /dev/null 
StrictHostKeyChecking no 
PasswordAuthentication no 
IdentityFile C:/Users/basme/.vagrant.d/insecure_private_key 
IdentitiesOnly yes LogLevel FATAL
```

To connect to the VM use: 
```
$ ssh User@HostName -p Port \
  -i IdentityFile
$ ssh vagrant@127.0.0.1 -p 2222 \ -i .vagrant/machines/default/virtualbox/private_key
```

## Configuration Options 
### Port Forwarding and Private IP Addresses
>[!note]
>When starting a new Vagrant file by using `vagrant init` the default networking configuration allows you to reach the Vagrant box only via a SSH port that is forwarded from local host.  

Web applications listen on ports that we can not access. To work around this problem we can set up forwarding ports. For example, the application listens on port 80 inside the Vagrant machine, so you can configure Vagrant to forward port 8040 on your local machine to port 80 on the VM.
![[Vagrant Port forwarding.png]]

To configure port forwarding add the following in the `Vagrantfile`:
```
config.vm.network :forwarded_port, host: 8000, guest: 80 
config.vm.network :forwarded_port, host: 8443, guest: 443
```

It is also possible to assign the VM a private IP address that is only accessible from the host machine.
```
config.vm.network "private_network", ip: "192168.33.10"
```
If we run a web server on the VM, we can access it via `http:// 192.168.33.10` with a browser.

# Inventory Files
>[!note]
>The default location for inventory files is `/etc/ansible/hosts`, but it can be stored in other places, like project directories along with playbooks and `ansible.cfg` files.

Ansible can only manage servers it explicitly knows, which is why you have to specify them in an *inventory*.
>[!note]
>If you do not have an inventory directory simply create one:
>```
>$ mkdir inventory
>$ mkdir ~/inventory
>```

Inside the inventory directory create a text file called `vagrant.ini` when using a Vagrant machine, The `.ini` file is your inventory file, in it you list the infrastructure you want to manage under groups, which are denoted in square brackets `[]`.

The inventory for the testserver should look like this:
```
[webservers]
testserver ansible_port=2222

[webservers:vars]
ansible_host=127.0.0.1
ansible_user=vagrant
ansible_private_key_file=.vagrant.d/insecure_private_key 
```

# ansible.cfg Files
`ansible.cfg` Files are used to set default variables, such as the location of the inventory file and other parameters that affect the way Ansible runs.
For the testserver it looks like this:
```
[defaults] 
inventory = inventory/vagrant.ini 
host_key_checking = False 
stdout_callback = yaml 
callback_enabled = timer
```

>[!note] Where to store the ansible.cfg File?
>Ansible looks for an `ansible.cfg` in the following places in this order: 
>- File specified by the ANSIBLE_CONFIG environment variable 
>- ./ansible.cfg (ansible.cfg in the current directory) 
>- ~/.ansible.cfg (.ansible.cfg in your home directory) 
>- /etc/ansible/ansible.cfg (Linux) or /usr/local/etc/ansible/ansi‐ ble.cfg (*BSD)

>[!problem]
>Error solution:
>replace std_callback = yaml 
>with:
>```
>[defaults] stdout_callback = ansible.builtin.default 
>result_format = yaml
>```
# Playbooks
*Playbooks* are configuration management scripts used in Ansible.
## Simple Webserver
Here we are configuring a playbook to install a NGINX web server and configure it for secure communication.
The directory will look like this:
```
.
├── Vagrantfile
├── ansible.cfg
├── files
│ ├── index.html
│ ├── nginx.conf
│ ├── nginx.crt
│ └── nginx.key
├── inventory
│ └── vagrant.ini
├── requirements.txt
├── templates
│ ├── index.html.j2
│ └── nginx.conf.j2
├── webservers-tls.yml
├── webservers.yml
└── webservers2.yml
```
### Preliminaries
Modify the Vagrantfile to look like this:
```
Vagrant.configure(2) do |config| 
# start a ubuntu/focal64 server
config.vm.box = "ubuntu/focal64" 
config.vm.hostname = "testserver" 
config.vm.network "forwarded_port",
# map ssh port on localhost 2202 to 22 on VM
id: 'ssh', guest: 22, host: 2202, host_ip: "127.0.0.1", auto_correct: false
 config.vm.network "forwarded_port",
 # map port 8080 on localhost to 80 on VM
 id: 'http', guest: 80, host: 8080, host_ip: "127.0.0.1"
 config.vm.network "forwarded_port",
 id: 'https', guest: 443, host: 8443, host_ip: "127.0.0.1"
 # disable updating guest additions
 if Vagrant.has_plugin?("vagrant-vbguest")
 config.vbguest.auto_update = false
 end
 config.vm.provider "virtualbox" do |virtualbox|
 virtualbox.name = "ch03"
 end
end
```

Start Vagrant with `vagrant up`.
### Simple Playbook
In this simple playbook called `webservers.yml` we will configure a host to run a simple HTTP server.
```
---
- name: Configure webserver with nginx
 hosts: webservers
 become: True
 tasks:
	 - name: Ensure nginx is installed
	   package: name=nginx update_cache=yes
	 - name: Copy nginx config file
	   copy:
		 src: nginx.conf
		 dest: /etc/nginx/sites-available/default
	 - name: Enable configuration
	   file: >
		 dest=/etc/nginx/sites-enabled/default
		 src=/etc/nginx/sites-available/default
		 state=link
	 - name: Copy index.html
	  template: >
		 src=index.html.j2
		 dest=/usr/share/nginx/html/index.html
	 - name: Restart nginx
	   service: name=nginx state=restarted
...
```
### Specifying an NGINX Config File
NGINX has a default configuration file that you can use if you just want to serve static files, but you will have to customize it.
The `nginx.conf` file for the webserver looks like this:
```
server {
 listen 80 default_server;
 listen [::]:80 default_server ipv6only=on;
 root /usr/share/nginx/html;
 index index.html index.htm;
 server_name localhost;
 location / {
 try_files $uri $uri/ =404;
 }
}
```

### Creating a Web Page
Ansible has a system to generate HTML page from a template file. 
The code for the web page is:
```
<html>
 <head>
 <title>Welcome to ansible</title>
 </head>
 <body>
 <h1>Nginx, configured by Ansible</h1>
 <p>If you can see this, Ansible successfully installed nginx.</p>
 <p>Running on {{ inventory_hostname }}</p>
 </body>
</html>
```

### Create a Group
We can define groups in inventory files.
We modify the `vagrant.ini` file to add the `testserver` to the `[webserver]` group: 
```
[webservers]
testserver ansible_port=2222

[webservers:vars]
ansible_host=127.0.0.1
ansible_user=vagrant
ansible_private_key_file=.vagrant.d/insecure_private_key
```

### Running the playbook
Execute the playbook with: 
```
$ ansible-playbook webserver.yml
```

If you did not get any errors point your browser to `http://local-host:8080`

### Ansible Commands
You can use the ansible command-line tool and the `command` module to run commands on the remote machine.
When using the `command` module you need to pass the `-a` flag, which is the command to run. But as the `command` module is so commonly used that it is the default and can be ommited.
- Check the uptime of the server
  ```
	$ ansible testserver -m command -a uptime
	$ ansible -a uptime
	```
- If your command has spaces quote it; f.e. you want to view the last ten lines of `/var/log/dmesg`:
  ```
	$ ansible testserver -a "tail /var/log/dmesg"
	```
 - If a command needs elevated privileges use the `-b` flag 
	```
	$ ansible testserver -b -a "tail /var/log/syslog"
	```
- You can also use other modules; f.e. you can install NGINX on Ubuntu:
	```
	$ ansible testserver -b -m package -a name=nginx
	```
	>[!tip]
	>If installing NGINX fails you might need to update the package list. To tell Ansible to update before installing a package use:
	>```
	>$ ansible testserver -b -m package -a "name=nginx update_cache=yes"
	>```
	>After that restart NGINX:
	>```
	>$ ansible testserver -b -m service -a "name=nginx state=restarted"
	>```
### Killing VMs
To destroy a VM and remove it use:
```
$ vagrant destroy -f
```

# YAML
Ansible playbooks are written in YAML syntax. 

>[!note] 
>Ansible files have only one YAML document each.
## Start and End of a Document
The beginning of a document is marked by three dashes `---`, but they are not strictly required.
The end of an YAML file is marked by three dots `...`, but no error is thrown if they are not written.
## Comments
Comments are start with a hashmark `#` like in Python or Ruby.
## Indentation and Whitespace
Like Python, YAML uses space indentation.
## Strings
Strings do not need to be quoted in YAML. A good practice nonetheless is to double-quote variables and other expressions; and single-qoute values that should not be evaluated, like version numbers.
## Booleans 
YAML provides a variety of boolean values, but you can just use `true` and `false`.
## Lists
YAML calls lists *sequences*, but the offical Ansible documentation still refers to lists.
Lists have a name followed by a colon, you then indent list items and delimit them with hyphens:
```
shows:
	- My fair Lady
	- Oklajoma
	- The Pirates of Penzance
```

You can also use the inline format:
```
shows: [My fair Lady, Oklahoma, The Pirates of Penzance]
```

## Dictionaries
YAML call dictionaries *mappings*, but the offical Ansible documentation still refers to them as dictonaries.
The syntax is:
```
address:
	street: Main Street
	appt: 742
	city: Logan
	state: Ohio
```

You can also use the inline format:
```
address: {street: Main Street, appt: '742', city: Logan, state: Ohio}
```
## Multiline Strings
You can format multiline strings with YAML by combining a block style indicator (`|` or `>`), a block chomping indicator (`+` or `-`) and even an indentation indicator (`1` to `9`).
For example, when we need a a preformatted block, we use the pipe character with a plus sign `|+`, telling the YAML parser to keep all line breaks as entered:
```
---
visiting_address: |+
	Department of Computer Science
	
	A.V. Williams Building
	University of Maryland
city: College Park
state: Maryland
```