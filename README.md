# Ansible

## What is YAML?

```
Server:
  -name: Serve1
  owner: John 
  created: 12232012
  status: active 
```

A YAML file is used to represent data, consfiguration data. 


- **Key value pari**

```
Fruit: Apple
Vegetable: Carrot
Liquid: Water
Meat: Chichken 
```
- We must have a space followed by a colon 


- **Array/Lists**

```
Fuits:
 -  Orange
 -  Apple
 -  Bananad
 
 Vegetable: 
 -  Carrot
 -  Cauliflower
 -  Tomato
 ```
 That dash indicates that it's an el  ement fo an array
  
- **Dictionary/Map**

```
Banana: 
  Calories: 105
  Fat: 0.4 g
  Carbs: 27g
  
Grapes: 
  Calories: 62
  Fat: 0.3 g
  Carbs: 16 g
```

We must have equal number of blank spaces before the properties of a single item, they are all aligned together 

## Dictionary vs Listl vs List of dictionaries


**Dictionary in dictionary

```
Color: Blue
Model: 
  Name: Corvette
  Year: 1995
Transition: Manual 
Price: $20, 000
```

**List of strings**

 ```
- Blue corvette
- Grey corvette
- Red corvette
- Green corvette
- Blue corvette
- Black corvette
```


**List of dictionaries**

```
- Color: Blue
Model: 
  Name: Corvette
  Year: 1995
Transition: Manual 
Price: $20, 000

- Color: Grey
Model: 
  Name: Corvette
  Year: 1995
Transition: Manual 
Price: $20, 000

- Color: Pink
Model: 
  Name: Corvette
  Year: 1995
Transition: Manual 
Price: $20, 000
```

Dictionary ----> Unordered
List       ----> Ordered

## Ansible Inventory 

**Agentless** 
Means that you don't need to install any additional software 


**Inventory**
The target systems before you can invoke any kind of automation.
Information about these target systems is sotored in an inventory file. 
If you don't create a new inventory file, Ansible uses the default inventory file located ar Etsy Ansible host location. 

The inventory file is an INI-like format

```
server1.company.com 
server2.company.com 
```
it's simply a number os servers listed one after the other. 

group: 

```
[mail]
server3.company.com
server4.company.com 
```
We can get differents groups in a inventroy file: 

```
[mail]
server3.company.com
server4.company.com

[db]
server5.company.com
server6.company.com

[web]
server7.company.com
server8.company.com
```

Create a group of groups:
```
[parent_group:children]
child_group1
child_group2
```
if i would like to refer to these servers in Ansible using an alias such as web server or database server. I could do this by adding an alias for each server: 
```
web anisble_host=server1.company.com
db ansible_host=server2.company.com
mail ansible_host=server3.company.com 
web2 ansible_host=server4.company.com 
```
*ansible_host* is an inventory parameter 

- Inventory parametes: 
  ansible_connection-ssh/winrm/localhost
  ansible_port-22/5986
  ansible_user0-root/administrator
  ansible_ssh_pass-Paswword 
  
**ansible_connection**
Ansible connection is what defines how ansible connects to the target server. 

```
web anisble_host=server1.company.com ansible_connection=ssh 
db ansible_host=server2.company.com ansible_connection=winrm
mail ansible_host=server3.company.com ansible_connection=ssh
web2 ansible_host=server4.company.com ansible_connection=winrm

localhost ansible_connection=localhost
```
**ansible_port**
Defines which port to connect to.

**ansible_usr**
ansible_user defines the user used to make remote connections

```
web anisble_host=server1.company.com ansible_connection=ssh ansible_user=root
db ansible_host=server2.company.com ansible_connection=winrm ansible_user=admin
mail ansible_host=server3.company.com ansible_connection=ssh
web2 ansible_hostd=server4.company.com ansible_connection=winrm

localhost ansible_connection=localhost
```
**ansible_ssh_pass**

Defines the ssh password for linux
```
web anisble_host=server1.company.com ansible_connection=ssh ansible_user=root
db ansible_host=server2.company.com ansible_connection=winrm ansible_user=admin
mail ansible_host=server3.company.com ansible_connection=ssh ansible_ssh_pass=P@asSWord
web2 ansible_host=server4.company.com ansible_connection=winrm

localhost ansible_connection=localhost
```

Note: For Linux based hosts, use ansible_ssh_pass parameter and for Windows based hosts, use ansible_password parameter.

## Ansible playbooks
 -Playbook - A single YAML file
  *play* defines a set of activities (tasks) to be run on hosts
  *task* An action to be performed on the host
    - Execute a command
    - Run a script
    - Install a package
    - Shutdown/Restart
    
   
```
-
  name: play 1
  hosts: localhost
  tasks: 
          - name: Execute command 'date'
            command: date
          
          - name: Execute script on server 
            script: test_script.sh
  -
  name: play 2
  hosts: localhost
  tasks: 
          - name: Install web service
            yum: 
                  name: httpd
                  state: present
                
          - name: Start web server 
            service: 
                    name: httpd
                    state: started 
                    
```

Contanisn a list of two plays. This is noted by the dash **-**
Each play is a dictionary has a set of preperties called: 
- name 
- post
- task


**Modules**

The different action run by tasks are called moudles. In this case command script YAML. 

```
- 
  name: Play 1
  hosts: localhost
  tasks: 
    - name: Execute command 'date'
      command: date
      
     - name: Execute script on server
       script: test_script.sh
      
     - name: Install httpd service
       yum:
          name: httpd
          state: present
          
       - name: Start web server
         service: 
              name: httpd
              state: started
 ```
 
 The different actions run by tasks are called **modules**. In this: 
 - Command
 - Script
 - Yum 
 - Service 
 
 
 for see all the modules run: 
  
  ```
  ansible-doc -l 
  ```
  
 ### Run 
 Execute ansible playbook 
 Syntax: 
 
 ```
 ansible-playbook <playbook file name>
 ```
 
 - ansible-playbook playbook.yml
 - ansible playbook --help 


Run the playbook.

```
ansible-playbook -i inventory playbook.yaml
```

## Ansible Modules
- System 
- Commands
- files
- Database
- Cloud
- Windows
- More...

-- System: 
    -User
    - Group
    - HOstname
    - Iptables
    - Lvg
    - Lvol 
    - Make 
    - Make
    - Moun
 
-- Commands: 
      - Command
      - Expect
      - Raw
      - Script 
      - Shell 
      
-- Files: 
       - Acl 
       - Archive
       - Copy
       - File
       - Find
       - Lineinfile
       - Replace 
       - Stat
       
-- Database:
         - Mongodb
         - Mssql 
         - Mysql 
         - Postgresql 
         - Proxysql 
         - Vertica
         
-- Cloud: 
         - Amazon
         - Azure
         - Docker
         - Google
         - Digital Ocean 
         
         
 -- Windows:
          - Win_copy
          - Win_command
          - Win_file
          etc..
          
          
### Command module

Executes a command on a remote node 

```
- 
  name: Play 1
  hosts: localhost
  tasks: 
    - name: Execute command 'date'
      command: date
    - name: Display resolv.conf contents
      command: cat /etc/resolv.conf
```

**Free form**

```
- name: copy file from source to destination 
  copy: mkdir /folder creates=/folder 
```
**Script**

Runs a local script on a remote node after transferring it 

  1.- Copy script to remote systems
  2.- Execute script on remote systems 

```
  name: Play 1
  hosts: localhost
  tasks: 
    - name: Run a script on remote server
      script: /some/local/script.sh -arg1 -arg2
```

**Service**
Manage services - start, stop, restart

This ansible playbook is used to start various services in a particular order.

 There are two ways or writing this statement
Dictionary format: 
```
- 
  name: Start Services in order 
  hosts: localhost
  tasks: 
    - name: Start the database service
      service: name=postgresql state=started 
 ```
 Why "started" and not "start"? 
 "start" the service httpd 
 "started" the service httpd

 Ensure service httpd is started 
 
 if httpd is not already started => start it
 if httpd is alreday started => do nothing 
 
 This is called **idempotency**
 An operations id idempotent if the result of performing it once is exactly the same as the result of performaing it repearedly without any intervening actoins. 
 
 Map format: 
 ```
 - 
  name: Start Services in order 
  hosts: localhost
  tasks:
    - name: Start the database service 
      service: 
      name: postgresql 
      state: started
 ```
**lineinfile**

Search for a line in a file and replace it or add it if it doesn't exist


### problem: 

Update the playbook /home/bob/playbooks/playbook.yaml to add a new task to start httpd service on all web nodes defined in /home/bob/playbooks/inventory file.


Use the service module.


solution 
```
- name: 'hosts'
  hosts: all
  become: yes
  tasks:
    - name: 'Execute a script'
      script: '/tmp/install_script.sh'
    - name: 'Start httpd service'
      service:
        name: 'httpd'
        state: 'started'
```
for run the file: 

```
ansible-playbook -i inventory playbook.yaml 
```


