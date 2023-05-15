# Ansible-ubuntu22.04
Ansible-ubuntu22.04


aws-ubuntu 22.04-t2.micro-KP-SG-4 INSTANCES
master node$ sudo apt update
$ sudo apt install ansible
$ cd .ssh
$ vim ansible_key
paste pem key open in note pad or visual studio etc
:wq
$ ls
$ cd
$ sudo ssh -i ~/.ssh/ansible_key ubuntu@172.31.16.118 (private ip of node 1)
yes
exit #connect to node sucessfully#
$    cat /etc/ansible/hosts
cat: /etc/ansible/hosts: No such file or directory

$ mkdir ansible
$ cd ansible
$ vim hosts
[servers]
server1 ansible_host=18.212.253.105
server2 ansible_host=3.208.12.109
server3 ansible_host=35.172.190.198

[all:vars]
ansible_python_interpreter=/usr/bin/python3
:wq

$ ansible-inventory --list -y    #error u didnt mention path
pwd
copy path
$ ansible-inventory --list -y -i /home/ubuntu/ansible/hosts/
================================================================
output 
all:
  children:
    servers:
      hosts:
        server1:
          ansible_host: 18.212.253.105
          ansible_python_interpreter: /usr/bin/python3
        server2:
          ansible_host: 3.208.12.109
          ansible_python_interpreter: /usr/bin/python3
        server3:
          ansible_host: 35.172.190.198
          ansible_python_interpreter: /usr/bin/python3
    ungrouped: {}
    ==================================================
$ ansible all -m ping -i /home/ubuntu/ansible/hosts
all nodes unreachable error will come

$ ansible all -m ping -i /home/ubuntu/ansible/hosts --private-key=~/.ssh/ansible_key
error private not accessible give permissions
cd ..
ubuntu@ip-172-31-30-226:~$ chmod 700 ~/.ssh        @write permission
ubuntu@ip-172-31-30-226:~$ chmod 600 ~/.ssh/ansible_key    @ read permission
$ ansible all -m ping -i /home/ubuntu/ansible/hosts --private-key=~/.ssh/ansible_key
=======================================output==========================================
The authenticity of host '3.208.12.109 (3.208.12.109)' can't be established.
ED25519 key fingerprint is SHA256:sOR7XYVG6r1cPqovF5efOd2GmQkKGj7+oJxfim6apBg.
This key is not known by any other names
The authenticity of host '35.172.190.198 (35.172.190.198)' can't be established.
ED25519 key fingerprint is SHA256:dpE3TweqOq6AxZURgVGeAhRivu28g51SZJRC13FwmHc.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yserver1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
es
server2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
======================================================================================
check free space of nodes from servers
ubuntu@ip-172-31-30-226:~$ ansible all -a "free -h" -i /home/ubuntu/ansible/hosts --private-key=~/.ssh/ansible_key
===============output===================
The authenticity of host '35.172.190.198 (35.172.190.198)' can't be established.
ED25519 key fingerprint is SHA256:dpE3TweqOq6AxZURgVGeAhRivu28g51SZJRC13FwmHc.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? server1 | CHANGED | rc=0 >>
               total        used        free      shared  buff/cache   available
Mem:           966Mi       176Mi       379Mi       0.0Ki       409Mi       638Mi
Swap:             0B          0B          0B
server2 | CHANGED | rc=0 >>
               total        used        free      shared  buff/cache   available
Mem:           966Mi       174Mi       380Mi       0.0Ki       411Mi       640Mi
Swap:             0B          0B          0B
==============================================================================================
check up time of servers/nodes
ubuntu@ip-172-31-30-226:~$ ansible servers -a "uptime" -i /home/ubuntu/ansible/hosts --private-key=~/.ssh/ansible_key
=================output===================================================
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? server1 | CHANGED | rc=0 >>
 16:58:20 up  1:11,  1 user,  load average: 0.01, 0.02, 0.00
server2 | CHANGED | rc=0 >>
 16:58:20 up  1:11,  1 user,  load average: 0.00, 0.00, 0.00
 ===========================================================================
