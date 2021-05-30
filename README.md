# Ansible-Precedence

##From Ansible-2.8 we have 22 ways where variables can come from

##We should mind the order of precedence

https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable

##Difference between inventories group_vars and playbook group_vars

    [magoyal@localhost ansible_inventory]$ tree
    .
    ├── ansible.cfg
    ├── group_vars
    │   └── all
    ├── inventories
    │   └── group_vars
    │       └── all
    └── inventory

Here playbooks_vars/all as higher priority than inventories group_vars/all

    .
    ├
    ├── group_vars
    │   └── all

Example:-

    [magoyal@localhost ansible_inventory]$ cat group_vars/all
    group: "group_vars"

    [magoyal@localhost ansible_inventory]$ cat inventories/group_vars/all
    group: "inventory/group_vars"

    [magoyal@localhost ansible_inventory]$ ansible-playbook group_vars.yml 
    [WARNING]: log file at /home/magoyal/ansible_inventory/logs/logs is not writeable and we cannot create it, aborting
    
    
    PLAY [all] ********************************************************************************************************************************
    
    TASK [check precedence] *******************************************************************************************************************
    Sunday 30 May 2021  17:09:59 +0530 (0:00:00.016)       0:00:00.016 ************ 
    ok: [hp-bl460cg8-2.gsslab.rdu2.redhat.com] => {
        "msg": "group_vars"
    }
    ok: [sun-x4170m2-1.gsslab.rdu2.redhat.com] => {
        "msg": "group_vars"
    }
    ok: [hp-dl380pg8-5.gsslab.rdu2.redhat.com] => {
        "msg": "group_vars"
    }
    ok: [sun-x4-2l-1.gsslab.rdu2.redhat.com] => {
        "msg": "group_vars"
    }
    
    PLAY RECAP ********************************************************************************************************************************
    hp-bl460cg8-2.gsslab.rdu2.redhat.com : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    hp-dl380pg8-5.gsslab.rdu2.redhat.com : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    sun-x4-2l-1.gsslab.rdu2.redhat.com : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    sun-x4170m2-1.gsslab.rdu2.redhat.com : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    
    Sunday 30 May 2021  17:09:59 +0530 (0:00:00.041)       0:00:00.058 ************ 
    =============================================================================== 
    check precedence ------------------------------------------------------------------------------------------------------------------- 0.04s
    Playbook run took 0 days, 0 hours, 0 minutes, 0 seconds


----

Inventory group_vars/* (* means group name eg. lb_servers)


Playbook group_vars/* 


Playbook group_vars/* has higher precedence than inventory group_vars




inventory host_Vars
touch inventories/host_vars/serverc.lab.example.com
^ this is an example of inventories host_vars

playbook host_vars

    [magoyal@localhost ansible_inventory]$ mkdir host_vars
    [magoyal@localhost ansible_inventory]$ touch host_vars/serverc.lab.example.com


playbook host_vars have higher precedence than inventory host_vars

------------------------------

    [user@demo project2]$ tree -F group_vars
    group_vars/
    ├── db_servers/
    │   ├── 3.yml
    │   ├── a.yml
    │   └── myvars.yml
    ├── lb_servers/
    │   ├── 2.yml
    │   ├── b.yml
    │   └── something.yml
    └── web_servers/
        └── nothing.yml
    
here we have broke down the variable in much more individual files
for example:
in group db_servers, we have three files 3.yml, a.yml, myvars.yml

when we will execute ansible playbook (and if it does effect db_servers group) then all three file i.e 3.yml, a.yml, myvars.yml 
