
# Ansible Configuration Management

Automating Infrastructure Configuration using Ansible. Ansible is an open source community project sponsored by Red Hat, it's the simplest way to automate IT. Ansible is the only automation language that can be used across entire IT teams from systems and network administrators to developers and managers.

## Project Requirements

Architecture Diagram

![Architecture diagram](https://darey.io/wp-content/uploads/2021/07/bastion.png)


### Install and configure Ansible on EC2 instances
Create an EC2 instance and enable SSH access on port 22
Install Ansible on the EC2 instance

```
sudo apt update
sudo apt install -y ansible
ansible --version
```

### Install and configure Jenkins on EC2 instances
Create an EC2 instance and enable inbound connection on port 8080 for Jenkins server
Install Java JDK and Jenkins on the EC2 instance

```
sudo apt update 
sudo apt-get install openjdk-11-jdk
sudo apt install jenkins 
```

Create a repository and configure a Jenkins webhook on the repository
Create a Freestly project on the Jenkins server to confirm that the webhook is working.

### Ansible Development
- Create a playbook common.yml to contain instructions you want to perform on the target machine
- Create an inventory file. inventory files contain the host target IP address, username if they are not the same OS, and also the python3 interpreter path, this is the only requirements for Ansible to be able to execute on the target machine.

In the common playbook file, write ansible instructions to install wireshark on all the host in the inventory files

```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
```

### Run the playbook

```
ansible-playbook -i inventory/dev.yml playbooks/common.yml
```


#### Project Screenshots

![Screenshots](images/01.png)

![Screenshots](images/02.png)

![Screenshots](images/03.png)

![Screenshots](images/04.png)

![Screenshots](images/05.png)

![Screenshots](images/06.png)

![Screenshots](images/07.png)

![Screenshots](images/08.png)




