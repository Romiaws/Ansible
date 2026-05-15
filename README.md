## Ansible
## control node installation
```
#!/bin/bash
sudo yum update -y
sudo yum install python3-pip -y
sudo yum install ansible -y
```
managed node
#!/bin/bash
sudo yum update -y
sudo yum install python3-pip -y
install the pageant - upload the key
putty - allow agent port forwarding
[web]
172.31.89.251
172.31.90.183
vi inventory.txt
ansible web -i inventory.txt -m ping
cat inventory.txt
ansible web -i inventory.txt -m yum -a "name=httpd state=present" -b
ansible web -i inventory.txt -m service -a "name=httpd state=started" -b
cat inventory.txt
create the playbook newplay.yaml
execute command -
ansible-playbook -i inventory.txt newplay.yaml
- name: install the httpd and then start
  hosts: all
  become: yes
  tasks:
    - name: install the httpd
      yum:
        name: httpd
        state: present
    - name:
      service:
        name: httpd
        state: started

- name: uninstall the httpd
  hosts: all
  become: yes
  tasks:
    - name: install the httpd
      yum:
        name: httpd
        state: absent

- name: install the httpd and then start
  hosts: all
  become: yes
  tasks:
    - name: install the httpd
      yum:
        name: httpd
        state: present
    - name: start the service 
      service:
        name: httpd
        state: started
    - name: copy the index.html
      copy:
         src: index.html
         dest: /var/www/html/
- name: Install Java on all servers
  hosts: all
  become: yes

  tasks:
    - name: Install Java (Debian/Ubuntu)
      yum:
        name: openjdk-17-jdk
        state: present

    - name: Verify Java Installation
      command: java -version
      register: java_output

    - name: Display Java Version
      debug:
        var: java_output.stderr
- name: Install Python on all servers
  hosts: all
  become: yes

  tasks:
    - name: Install Python (RHEL/CentOS/Amazon Linux)
      yum:
        name: python3
        state: present

    - name: Verify Python Installation
      command: python3 --version
      register: python_output

    - name: Display Python Version
      debug:
        var: python_output.stdout

- name: Install MySQL on all servers
  hosts: all
  become: yes

  tasks:
    - name: Install MySQL Server
      yum:
        name: mysql-server
        state: present

    - name: Start MySQL Service
      service:
        name: mysqld
        state: started
        enabled: yes

    - name: Verify MySQL Installation
      command: mysql --version
      register: mysql_output

    - name: Display MySQL Version
      debug:
        var: mysql_output.stdout

Ansible roles
create the Ansible role
command ->
ansible-galaxy init myrole
create the yaml file for the role - newfile.yaml
- name: Install Java on all servers
  hosts: all
  become: yes
  roles:
    - myrole
role --> tasks --> main.yml
- name: install the httpd
  yum:
    name: httpd
    state: present
ansiblle-playbook -i inventory.txt newfile.yaml
