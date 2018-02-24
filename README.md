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
