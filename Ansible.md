# Install
```
pip --install ansible
```
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
config.vm.network "private_network", ip: "192.168.33.10"
```
If we run a web server on the VM, we can access it via `http:// 192.168.33.10` with a browser.

# Inventory Files
>[!note]
>The default location for inventory files is `/etc/ansible/hosts`, but it can be stored in other places, like project directories along with playbooks and `ansible.cfg` files.

Ansible uses `.ini` format for inventory hosts files; it groups configuration values into sections.

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

## Creating Multiple VMs with Vagrant
To create multiple Vagrant machines create a new Vagrantfile:
```
Vagrant.configure("2") do |config|
# Use the same key for each machine
  config.ssh.insert_key = false
  config.vm.define "vagrant1" do |vagrant1|
	vagrant1.vm.box = "ubuntu/focal64"
	vagrant1.vm.network "forwarded_port", guest: 80, host: 8080
	vagrant1.vm.network "forwarded_port", guest: 443, host: 8443
  end
  config.vm.define "vagrant2" do |vagrant2|
	vagrant2.vm.box = "ubuntu/focal64"
	vagrant2.vm.network "forwarded_port", guest: 80, host: 8081
	vagrant2.vm.network "forwarded_port", guest: 443, host: 8444
  end
  config.vm.define "vagrant3" do |vagrant3|
	vagrant3.vm.box = "centos/8"
	vagrant3.vm.network "forwarded_port", guest: 80, host: 8082
	vagrant3.vm.network "forwarded_port", guest: 443, host: 8445
  end
end
```

After we started the VMs with `vagrant up` we inspect the ssh settings of them with:
```
$ vagrant ssh-config
```

We now modify the file `~/.ssh.config`
```
Host vagrant*
 Hostname 127.0.0.1
 User vagrant
 UserKnownHostsFile /dev/null
 StrictHostKeyChecking no
 PasswordAuthentication no
 IdentityFile ~/.vagrant.d/insecure_private_key
 IdentitiesOnly yes
 LogLevel FATAL
```

We also modify the inventory file `vagrant.ini` and add the ssh informations:
```
vagrant1 ansible_port=2222
vagrant2 ansible_port=2200
vagrant3 ansible_port=2201
```

>[!note]
>We also need to add the ssh key with;
>```
>$ ssh-add /path/to/key
>```

## Behavioral Inventory Parameters
To describe a host in an inventory file you use *behavioral inventory parameters*, which have a default value that can be overwritten:
![[Behavioral Inventory Parameters.png|588x240]]

- **ansible_connection**:
  Ansible supports multiple transport mechanism to connect to the host. The default `smart` checks if the host's SSH client supports `ControlPersist`. If not Ansible will use a Python-based SSH client (Paramiko) instead of the SSH client.
- **ansible_shell_type**:
  Ansible assumes the remot shell is a bash shell at `/bin/sh` and generates appropriate command-line parameters, stored in a temporary directories.
  Ansible also accepts `csh`,  `fish` and `powershell` as values.
- **ansible_pythone_interpreter**:
  Ansible needs to know the location of the Python interpreter on the remote machine.
  The easiest way to run Ansible under Python 3 is to install it with pip3 and set:
  `ansible_python_interpreter="/usr/bin/env python3`
- **`ansible_*_interpreter`**:
  If you run a module that is not written in Python you can set this parameter to the location of the interpreter

>[!note] Changing Behavioral Parameter Defaults
>You can overwrite these parameters in the inventory file or in the `defaults` section in the `ansible.cfg` file.
>

## Groups
By default Ansible creates a default group `all` or `*`, which includes all hosts in the inventory.
You can create own groups in the inventory file by defining it with `[groupname]`:
```
[webservers]
web1@example.com
web2@example.com

[testserver]
test1 ansible_port=2221
test2 ansible_port=2222
```

### Groups of Groups 
Ansible allows you to create groups that consists of groups, f.e. you have two groups that need the same software and its dependencies, it can be useful to define a new group for these two
```
[django:children]
web 
task
```

## Numbered Hosts
If you a lot of hosts that do not have specific names, but are refered to by a identification number, f.e. (webserver1, webserver2, ...) you can specify them in the inventory file like this
```
[group]
hosts[1:x]

[web]
webserver[1:20].example.com
```

## Host and Group Variables
### Inside the Inventory
You can specify behavioral inventory parameters for each host:
```
vagrant1 ansible_host=127.0.0.1 ansible_port=2222
vagrant2 ansible_host=127.0.0.1 ansible_port=2200
vagrant3 ansible_host=127.0.0.1 ansible_port=2201
```

You can also specify group variables:
```
[all:vars]
ntp_server=ntp.ubuntu.com

[production:vars]
c
```

### Inside a own file
When inventories become larger it can get difficult to manage them. 
Ansible allows you to create [[YAML]] files for each host and group. Host variable files should be stored in your `inventory` directory in a new directory called `host_vars` and group variables should stored in a `group_vars` directory.

You can define vars either like this:
`/group_vars/production.yml`
```
---
db_primary_host=host1.example.com
db_primary_port=5432
db_name=widget_production
db_user=widgetuser
db_password=pFmMxcyD;Fc6)6
```

Or use YAML dictionaries:
```
db:
	user:widgetuser
	password: 'pFmMxcyD;Fc6)6'
	name:widget_production
	primary:
		host:host1example.com
		port: 5432
```

>[!note]
>If we choose YAML dictionaries we would access them with dot notation:
>```
>"{{ db.primary.host }}"
>```
>
>Otherwise we use:
>```
>"{{ db_primary_host }}"
>```

## Dynamic Inventory
If an external service keeps track of your servers (f.e. Amazon EC2, Cobbler or configuration management databases (CMDBs) ) we can use a dynamic inventory.
### Plug-ins
Ansible comes with several executables that can connect to various cloud systems, provided you install the requirements and set uo authentication.
You must store them in a YAML configuration file in the inventory directory
>[!tip]
>To list the available plug-ins:
>```
>$ ansible-doc -t inventory -l
>```
>
>To see plug-in specific documentation and examples:
>```
>$ ansible-doc -t inventory plug-in-Name
>```

If you are using Amazon EC2, install the requirements:
```
$ pip3 install boto3 botocore
```
Create a file inventory/aws_ec2.yml with, at the very least:
- plugin: aws_ec2

Install these requirements in a Python virtualenv with Ansible 2.9.xx:
```
$ pip3 install msrest msrestazure
```
Create a file inventory/azure_rm.yml with, at the very least:
```
plugin: azure_rm
platform: azure_rm
auth_source: auto
plain_host_names: true
```
### Writing a Dynamic Inventory Script
Here we are writing a dynamic script for the vagrant machines.
The script has to invoke the commands `vagrant status` and `vagrant ssh-config`, parse the outputs and create a JSON file.

We can use the Pyhton [Paramiko](https://www.paramiko.org/) library. 
Install with:
```
pip3 install --user paramiko
```

We now interact with Paramiko to parse the output of `vagrant ssh-config:
```
$ python3
>>> import io
>>> import subprocess
>>> import paramike
>>> cmd = ["vagrant", "ssh-config", "vagrant2"]
>>> ssh_config = subprocess.check_output(cmd).decode("utf-8")
>>> config = paramiko.SSHConfig()
>>> config.parse(io.StringIO(ssh_config))
>>> host_config = config.lookuup("vagrant2")
>>> print (host_config)
```

The complete script would look like this:
`vagrant.py`
```python
#!/usr/bin/env python3
""" Vagrant inventory script """
# Adapted from Mark Mandel's implementation
# https://github.com/markmandel/vagrant_ansible_example
import argparse
import io
import json
import subprocess
import sys
import paramiko
def parse_args():
 """command-line options"""
 parser = argparse.ArgumentParser(description="Vagrant inventory script")
 group = parser.add_mutually_exclusive_group(required=True)
 group.add_argument('--list', action='store_true')
 group.add_argument('--host')
 return parser.parse_args()
def list_running_hosts():
 """vagrant.py --list function"""
 cmd = ["vagrant", "status", "--machine-readable"]
 
status = subprocess.check_output(cmd).rstrip().decode("utf-8")
 hosts = []
 for line in status.splitlines():
 (_, host, key, value) = line.split(',')[:4]
 if key == 'state' and value == 'running':
 hosts.append(host)
 return hosts
def get_host_details(host):
 """vagrant.py --host <hostname> function"""
 cmd = ["vagrant", "ssh-config", host]
 ssh_config = subprocess.check_output(cmd).decode("utf-8")
 config = paramiko.SSHConfig()
 config.parse(io.StringIO(ssh_config))
 host_config = config.lookup(host)
 return {'ansible_host': host_config['hostname'],
 'ansible_port': host_config['port'],
 'ansible_user': host_config['user'],
 'ansible_private_key_file': host_config['identityfile'][0]}
def main():
 """main"""
 args = parse_args()
 if args.list:
 hosts = list_running_hosts()
 json.dump({'vagrant': hosts}, sys.stdout)
 else:
 details = get_host_details(args.host)
 json.dump(details, sys.stdout)
if __name__ == '__main__':
 main()

```

## Adding Entries at Runtime
Ansible will let you add hosts and groups to the inventory during the execution of a playbook.
### add_host
With the `add_host` module you can add a host to the inventory, which is useful when using Ansible to provision new VMs inside an IaaS cloud. 

>[!note] add_host and Dynamic Inventories
>If a new host comes online while a playbook is executing, the dynamic inventory script will not pick up this new host. This is because the dynamic inventory script is executed at the beginning of the playbook: if any new hosts are added while the playbook is executing, Ansible won’t see them.

Invoking the module in a playbook could look like this:
```YAML
---
- name: Provision a Vagrant machine
  hosts: localhost
  vars:
 	box: centos/stream8
  tasks:
 	- name: Create a Vagrantfile
 	  command: "vagrant init {{ box }}"
 	  args:
 		creates: Vagrantfile
 	- name: Bring up the vagrant machine
 	  command: vagrant up
 	  args:
 		creates: .vagrant/machines/default/virtualbox/box_meta
 	- name: Add the vagrant machine to the inventory
 	  add_host:
 		name: default
 		ansible_host: 127.0.0.1
 		ansible_port: 2222
 		ansible_user: vagrant
 		ansible_private_key_file: >
 			.vagrant/machines/default/virtualbox/private_key
	- name: Do something to the vagrant machine
 	  hosts: default
 	  tasks:
 		# The list of tasks would go here
 		- name: ping
 		  ping:
...
```

### group_by
`group_by` allows you to create new groups while a playbook is executing.

>[!note]
>You can create separate groups for different Linux distributions. Then using f.e. `apt` module for Ubuntu machines and `yum` for CentOS.

A playbook could look like this:
```YAML
---
- name: Group hosts by distribution
  hosts: all
  gather_facts: true
  tasks:
	- name: Create groups based on distro
	   group_by:
		 key: "{{ ansible_facts.distribution }}"
	- name: Do something to Ubuntu hosts
	  hosts: Ubuntu
	  become: true
	  tasks:
		 - name: Install jdk and jre
		   apt:
			 update_cache: true
			 name:
				 - openjdk-11-jdk-headless
				 - openjdk-11-jre-headless
	- name: Do something else to CentOS hosts
	  hosts: CentOS
	  become: true
	  tasks:
		 - name: Install jdk
		   yum:
			name:
				 - java-11-openjdk-headless
				 - java-11-openjdk-devel
```
#  ansible.cfg Files
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

## Anatomy of a Playbook

![[Anatomy of a Playbook.png]]
This Entity-relationship diagram describes the anatomy of a playbook:
- A *playbook* contains one or more plays
- A *play* associates a set of hosts with an ordered list of one or more tasks
- A *task* is associated with one module
### Playbook in YAML-style
This playbook has the same function as `webservers.yml`, but it is written in YAML style.
It is also used to visualize the different parts of a playbook.
```
--- 
- name: Configurewebserver with nginx
  hosts: webserver
  become: true
  tasks:
	- name: Ensuren nginx is installed
	  package:
		name: nginx
		update_cache: true
		    
	- name: Copy nginx config file
	  copy:
		src: nginx.conf
		dest: /etc/nginx/sitesavailable/default
	
	- name: Enable configuration
	  file:
		src: /etc/nginx/sites-available/default
		dest: /etc/nginx/sites-enabled/default
		state: link
	
	- name: Copy home page template
	  template:
		src: index.html.j2
		dest: /usr/share/nginx/html/index.html
		
	- name: Restart nginx
	  service:
		name: nginx
		state: restarted
...
```
### Plays
A playbook is a list of dictionaries, specifically a list of *plays*. A play can be thought of as the thing that connects to a group of hosts and indicates what to do on those hosts. 
Because of this every play must contain the `hosts` variable, which can be assigned to a group like `webservers`, the group `all` or a set of hosts and a list of tasks.
The example playbook has only one play, but it is possible to have multiple plays for different hosts in a single playbook.
Additionally plays also support optional settings:
- **name**: 
	- Describes what the play is about
	- Ansible prints the name when the play starts to run
- **become**: 
	- If this value is true, Ansible will become the `become_user`
	- default is `root` but other user can be specified
	- can be specified by task or by play
- **vars**:
	- a list of variables and values

### Tasks
The example playbook has five tasks, with the first one being: 
```
- name: Ensure nginx is installed
  package:
	  name: nginx
	  update_cache: true
```

A task defines what will be done on the hosts:
- tell the `package` module to install the package `nginx` and update the package cache before installing

> [!tip]
> If you do not want to run an entire play as root, you can set `become: true` for individual tasks that require root access: 
> ```
> - name: Install package
>   become: true
>   ...
> ```

### Modules
*Modules* are Python scripts that come prepackaged with Ansible and perform actions on the host. Actions on the host are performed by generating a custom script based on the module name and arguments, which is then copied to the host and run.
In the example we used the following modules:
- **package**: Installs or removes packages by using the host's package manager
- **copy**: Copies a file from the machine where you run Ansible to the web server
- **file**: Sets the attribute of a file, symlink or directory
- **template**: Generates a file from a template and copies it to the host
- **service**: Starts, stops or restarts a service

>[!note]
>Modules for Windows are written in PowerShell, with a Python counterpart that contains only the documentation.

#### Installing from a Git Repository
If you want to install an application from a Git repository you can use the `git` module:
```
- name: Install Mezzanine
  git:
	  repo: git@github.com:ansiblebook/mezzanine_example.git
	  dest: path/to/directory
	  version: master
	  accept_hostkey: true
```

For this you will need a GitHub account and you must add your [public SSH key](https://github.com/settings/keys) to your machine:
1. Start your SSH agent:
	```
	$ eval $(ssh-agent)
   ```
2. Add your key
   ```
   $ ssh-add path/to/privateKey
   ```
3. Check the output of the Key
   ```
   $ ssh-add -L
   ```
4. Enable agent forwarding, by adding the following to the `anisble.cfg` file:
   ```
   [ssh_connection]
   ssh_args = -o ForwardAent=yes
   ```
5. Check that you can reach the GitHub SSH server:
   ```
   $ ansible web -a "ssh -T git@github.com"
   ``` 

#### Ansible Module Documentation
To view the documentation of a module use:
```
$ ansible-doc moduleName
$ ansible-doc service
```

You can also search for specific modules, f.e. a search for modules related to the Ubuntu `apt` package would be done with:
```
$ ansible-doc -l | grep ^apt
```

#### debug Module
The `debug` module is Ansible's version of a `print` statement and use it to print out the value of a variable or an arbitrary string.
```
- debug: var=myvariable
- debug: msg="The calue of myvariable is {{ var }}
```

#### stat Module
The `stat` Module collects information about the state of a filepath and returns a dictionary with [values](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/stat_module.html#return-values):
 f.e.:

| Field        | Description                                                         |
| ------------ | ------------------------------------------------------------------- |
| `isdir`      | The path is a directory                                             |
| `isreg`      | The path is a regular file                                          |
| `checksum`   | Hash value of the file                                              |
| `executable` | Tells you if the invoking user has execute permissions for the path |

#### assert Module
The `assert` Module fails with an error if a specified condition is not met, this can be useful when debugging so that a failure happens as soon as any assumption you've made is violated.

If you want to check the status of a file on the host you call the `stat` module and make an assertion based on the return value:
```
- name: Stat /boot/grub
  stat:
	  path: /boot/grub
  register: st
  
- name: Assert that /boot/grub is a directory
  assert:
	  that: st.stat.isdir
```

>[!important]
>The code in an `assert` statement is Jinja2 not Python.
>If you want to assert the length of a list have to use
>```
>assert:
>	that: "ports|length == 2"
>```
>and not Python
>```
>assert:
>	that: "len(ports) == 2"
>```
## Adding TLS Support
We are now using another playbook in which we will add TLS support to the webserver. 
Additionally we will explore more Ansible features.
`webservers-tls.yml`
```
---
- name: Configure webserver with Nginx and TLS
	 hosts: webservers
	 become: true
	 gather_facts: false
	 
	 vars:
		 tls_dir: /etc/nginx/ssl/
		 key_file: nginx.key
		 cert_file: nginx.crt
		 conf_file: /etc/nginx/sites-available/default
		 server_name: localhost
 
	 handlers:
		 - name: Restart nginx
		   service:
			 name: nginx
			 state: restarted
 
	 tasks:
		 - name: Ensure nginx is installed
		   package:
			 name: nginx
			 update_cache: true
		   notify: Restart nginx
 
		 - name: Create directories for TLS certificates
		   file:
			 path: "{{ tls_dir }}"
			 state: directory
			 mode: '0750'
		   notify: Restart nginx
 
		 - name: Copy TLS files
		   copy:
			 src: "{{ item }}"
			 dest: "{{ tls_dir }}"
			 mode: '0600'
		   loop:
			 - "{{ key_file }}"
			 - "{{ cert_file }}"
		   notify: Restart nginx
 
		 - name: Manage nginx config template
		   template:
			 src: nginx.conf.j2
			 dest: "{{ conf_file }}"
			 mode: '0644'
		   notify: Restart nginx
 
		 - name: Enable configuration
		   file:
			 src: /etc/nginx/sites-available/default
			 dest: /etc/nginx/sites-enabled/default
			 state: link
 
		 - name: Install home page
		   template:
			 src: index.html.j2
			 dest: /usr/share/nginx/html/index.html
			 mode: '0644'
 
		 - name: Restart nginx
		   meta: flush_handlers
 
		- name: "Test it! https://localhost:8443/index.html"
		  delegate_to: localhost
		  become: false
		  uri:
			 url: 'https://localhost:8443/index.html'
			 validate_certs: false
			 return_content: true
		  register: this
		  failed_when: "'Running on ' not in this.content"
		  tags:
			 - test
...
```
### Generating a TLS Certificate
First we will generate a self-signed TLS certificate.
For this we will use the following command to create a `nginx.key` and a `nginx.crt` file, both being stored in the `/files` subdirectory:
```
$ openssl  req -x509 -nodes -days 365 -newkey rsa:2048 \
 -subj /CN=localhost \
 -keyout files/nginx.key -out files/nginx.crt
```

### Variables
Variables can be used in tasks and templates files.
```
vars:
	tls_dir: /etc/nginx/ssl/
	key_file: nginx.key
	cert_file: nginx.crt
	conf_file: /etc/nginx/sites-available/default
	server_name: localhost
```
They can be used by using `{{ variable }}` in tasks:
```
- name: Manage nginx config template
  template:
	  src: nginx.conf.j2
	  dest: "{{ conf_file }}"
	  mode: '0644'
  notify: Restart nginx
```

### Configuration Templates
Ansible uses the Jinja2 template engine to implement templating. Templating allows to reuse the same bits of configuration data (f.e.IP address of a server or database credentials), so you do not have to hand-edit multiple configuration files.

Here we are creating a nginx configuration file, specifing where to find the TLS key and certificate.
The file is called `nginx.conf.j2`
```
server {
	listen 80 default_server;
	listen [::]:80 default_server ipv6only=on;
	
	listen 443 ssl;
	ssl_protocols TLSv1.2;
	ssl_prefer_server_ciphers on;
	root /usr/share/nginx/html;
	
	index index.html;
	server_tokens off;
	add_header X-Frame-Options DENY;
	add_header X-Content-Type-Options nosniff;
 
	server_name {{ server_name }};
	ssl_certificate {{ tls_dir }}{{ cert_file }};
	ssl_certificate_key {{ tls_dir }}{{ key_file }};
 
	location / {
		try_files $uri $uri/ =404;
	}
}
```

### Loops
You can use loops to execute a task multiple times, each time replacing the `{{ item }}` with a specified value:
In this example `{{ item }}` will be replaced with `key_file` and one time with `cert_file`.

```
- name: Copy TLS files
  copy:
	src: "{{ item }}"
	dest: "{{ tls_dir }}"
	mode: '0600'
	
  loop:
	- "{{ key_file }}"
	- "{{ cert_file }}"
  notify: Restart nginx
```

### Handlers
*Handlers* are a type of conditional form, being similar to a task, but only running when notified by another task.
To define a handler:
```
handlers:
	- name Restart nginx
	  service:
		  name: nginx
		  state: restarted
```

For a task to notify a handler use:
```
- name: Manage nginx config template
  template:
	  src: nginx.conf.j2
	  dest: "{{ conf_file }}"
	  mode: '0644'
  notify: Restart nginx
```

>[!note]
>In this playbook nginx needs to be restarted if:
>- the TLS key or certificate changes
>- the configuration file changes
>- the contents of the `sites-enabled` directory change

To force a notified handler in the middle of a play we use:
```
- name: Restart nginx
  meta: flush_handlers
```

Handlers are run in the order they are defined and not in the notification order.
The official Ansible documentation advises to only use handlers to reboot and restart services.

## Checking Playbooks before execution
### Syntax Check
Use the `--syntax-check` flag to check the syntax of your playbook
```
$ ansible-playbook --syntax-check playbook.yml
```

### List Hosts
You can list the hosts against which the playbook will run with the `--list-hosts` flag:
```
$ ansible-playbook --list-hosts playbook.yml
```

>[!tip]
>If you get a warning that your inventory is empty you can add hosts to the inventory:
>```
>ansible-playbook --list-hosts -i web,db playbook.yml
>```

## Tags
Ansible allows you to add tags to task, roles or plays:
```
- name: deploy postgres on db
  hosts: db
  vars_files:
	  - secrets.yml
  roles:
	  - role: database
	    tags: database
	    database_name: name
	    database_user: user
```

You can then use the `-t` or `--tags` flag to run only plays and tasks that have that tag. You can also use `--skip-tags` to list the tags that should be skipped:
```
$ ansible-playbook -tdatabase playbook.yml
$ ansible-playbook --tags=database,tag2 playbook.yml
$ ansible-playbook --skip-tags=database playbook.yml
```

# Variables and Facts
## Defining Variables in Playbooks
To define variables in a playbook create a `vars` section in your playbookwit hthe names and values of your variables: 
```
vars:
	tls_dir: /etc/nginx/ssl/
	key_file: nginc.key
	cert_file: nginx.crt
	conf_file: /etc/nginx/sites-available/default
	server_name: localhost
```

## Defining Variables in separate Files
You can store variables in one or more files and reference them in your playbook in a `vars_files` section:
`vars-yml`
```
key_file: nginx.key
cert_file: nginx.crt
cont_file: /etc/nginx/sites-available/default
server_name: localhost
```

```
vars_files:
 - vars.yml
```

## Viewing Values
You can print the value of variables using the `debug` module: 
```
- debug: var = varName
```

Or you can display a debug message with a variable:
```
- name: Display the variable
  debug:
    msg: "The file used was {{ conf_file }}"
```

Variables can be concatenated between double braces by using the tilde operator `~`
```
- name: Concatenate variables
  debug:
	  msg: "The URL is https://{{ ser_name ~'.'~ domain_name}}"
```
## Registering Variables
When you need to set the result of a task as the value of a variable you can create a *registered variable* using the `register` clause when invoking a module. 

Example Capturing the output of a command:
```
---
- name: Show return value of command module
  hosts: hosts
  gather_facts: false
  tasks:
	  - name: Capture output of id command
	    command: id -un
	    register: login
	    
	  - debug: var=login
	  - debug: msg:"Logged in as user {{ login.stdout }}"  
```


# Virtual Environments
When installing Python packages it is a good practice to install these in an isolated virtual environment to avoid polluting the system-level Python packages or cause compatibility errors. User can create multiple `virtualenvs` and install packages without needing root access.

The `pip` module allows to create `virtualenvs` and install packages into them:
Create a `virtualenv`:
```
- name: Create python3 virutalenv
  pip:
	name:
		- pip
		- wheel
		- setuptools
	state: latest
	virtualenv: /path/to/.virtualenvs/virtualenvName
	virtualenv_command: /usr/bin/python3 -m venv		
```

Install packages into `virtualenv`:
```
- name: Copy requirements.txt to home directory
  copy:
	src: requirements.txt
	dest: path/to/directory
	mode: '0644'
	
- name: Install packages listed in requirements.txt
  pip:
	  virtualenv: /path/to/.virtualenvs/example
	  requirements: path/to/directory
```

A common practice is Python projects is to specify the package dependencies in a file called `requirements.txt` stored in the directory with the playbooks. 
An example would be:
```
beautifulsoup4==4.9.3
bleach==3.3.0
certifi==2021.5.30
chardet==4.0.0
Django==1.11.29
django-appconf==1.0.4
django-compressor==2.4.1
django-contrib-comments==2.0.0
filebrowser-safe==0.5.0
future==0.18.2
grappelli-safe==0.5.2
gunicorn==20.1.0
idna==2.10
Mezzanine==4.3.1
oauthlib==3.1.1
packaging==21.0
Pillow==8.3.1
pkg-resources==0.0.0
psycopg2==2.9.1
pyparsing==2.4.7
python-memcached==1.59
pytz==2021.1
rcssmin==1.0.6
requests==2.25.1
requests-oauthlib==1.3.0
rjsmin==1.1.0
setproctitle==1.2.2
six==1.16.0
soupsieve==2.2.1
tzlocal==2.1
urllib3==1.26.6
webencodings==0.5.1
```

If you already have an existing virtualenv with the packages installed you ca nuse the `pip freeze`command to print a list of the packages, then activate a new virtualenv and save the packages into a `requirements.txt`: 
```
$ source virtualenvs/virtualenvironment/bin/activate
$ pip freeze > requirements.txt
```

Einrichten
Paython3 -m venv Ansible
source vctivate im / Ansible/bin verzeichnic

# Debugging Playbooks
## Making Error Messages readable
To make the output of error messages more readable we can use the `debug` callback plug-in. We enable the plug-in by adding the following to the `defaults` section on `ansible.cfg`:
```
[defaults]
stdout_callback = debug
```

>[!note]
>This plug-in does not print all information; the `YAML` plug-in is more verbose

## SSH Issues
### SSH server is not responding
When a SSH server is not responding it could look like this:
```
$ ansible web -m ping
web | UNREACHABLE! => {
	"changed": false,
	"msg": "Failed to connect to the host via ssh:
kex_exchange_identification: Connection closed by remote host",
	"unreachable": true
}
```

Possible causes are: 
-  The SSH server is not running at all.
-  The SSH server is running on a nonstandard port.
-  Something else is running on the port you expect.
-  The port might be filtered by the firewall on the host.
-  The port might be filtered by another firewall.
-  Tcpwrappers is configured, check /etc/hosts.allow and /etc/hosts.deny.
-  The host runs in a hypervisor with micro-segmetation.

After verifying that the SSH server is running you can try to connect with `nc` to check the banner:
```
$ nc host-name 2222
SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.4
```

Then try to connect with SSH and set the verbose flag:
```
$ ssh -v user@host-name
```

If that works, try to ping the host with ansible:
```
$ ansible web -vvvv -m ping
```

### PasswordAuthentication no
With `PasswordAuthentication no` SSH authentication is only possible with keys, meaning you can not login with a password.
To distribute the keys onto a machine you want to manage you use can use the `authorized_key` module:
```
- name: Install authorized_keys taken from file
  authorized_key.
	  user: "{{ the_user }}"
	  state: present
	  key: "{{ lookup('file', pub_key) }}"
	  key_options: 'no-port-forwarding,from'
	  exclusive: true
```

### SSH as a different User
You can connect to different hosts with different user. 
To set a user you can configure `ansible_user` in the inventory file:
```
[server]
web ansible_host=192.168.0.20 ansible_user=webmaster
db ansible_host=192.168.0.21 ansible_user=dbmaster
```

You can also specify a different user on the command line when executing a playbook:
```
$ ansible-playbook --user user1 -i inventory/hosts playbook1
```

Another way would be to use the SSH config file to define a user for each host or set `remote_user:` in the header of a play or in a task.

### Host Key Verification Failed
When you get an error such as:
```
$ ansible -m ping web
web | UNREACHABLE! => {
 "changed": false,
 "msg": "Failed to connect to the host via ssh:
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\r\n@ WARNING:
REMOTE HOST IDENTIFICATION HAS CHANGED!
...
```

Remove the old host key and add the new key:
```
$ ssh-keygen -R host-IP
$ ssh-keyscan host-IP >> ~/.ssh/known_hosts
```
# Collection




