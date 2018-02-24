# ansible
ansible_ssh_host 
ansible_ssh_port
ansible_ssh_user
ansible_ssh_pass
ansbile_connection
ansible_ssh_private_key_file
ansible_shell_type
ansible_python_interpreter

[vagrant]
vagrant1 ansbile_ssh_host=127.0.0.1 ansbile_ssh_port=2200
vagrant2 ansbile_ssh_host=127.0.0.1 ansbile_ssh_port=2200

#ansible中文结果返回乱码
module_lang    = zh_CN.UTF-8
module_set_locale = True

#查看与某台服务器关联的所有fact
gather_facts: True
ansible server1 -m setup

#查看子集
ansible web -m setup -a 'filter=ansible_eth*'
#列出playbook中的task
ansible-playbook --list-tasks mezzanine.yml

#使用迭代(with_items) 安装多个软件包
- name: install apt packages
  apt: pkg={{ item }} update_cache=yes cache_valid_time=3600
  sudo: True
  with_items:
    - git
    - libjpeg-dev
    - nginx
  #通过requirements.txt安装软件包
  - name: copy requirements.txt file
    copy: src=files/requirements.txt dest=~/requirements.txt
  - name: install packages
    pip: requirements=~/requirements.txt virtualenv={{ venv_path }}
    
 #在控制主机上运行task
 - name: wait for ssh server to be running
   local_action: wait_for port=22 host="{{ inventory_hostname}}" search_regex=OpenSSH
 #在涉及的主机以外的机器上运行task
 - name: enable alerts for web servers
   hosts: web
   tasks:
     - name: enable alerts
       nagios: action=enable_alerts service=web host={{inventory_hostname}}
       delegate_to: nagios.example.com
       
  #逐台主机执行
  - name: upgrade packages on servers bebind load balancer
    hosts: myhosts
    serial: 1
    max_fail_percentage: 25
    如果我们在负载均衡后面有四台主机，并且有一台主机执行task失败，那么anisble还将继续执行play,因为没有超过25%，如果有第二台主机执行task失败，anible将会让整个play失败，如果希望ansible在任何主机出现task执行失败的时候都放弃，则需要将max_fail_percentage 设置为0
