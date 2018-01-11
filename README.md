# zookeeper-ansible
1. 通过ansible自动化安装zookeeper集群
2. 确保zookeeper集群节点数为2N+1
3. 目前仅支持Centos7.x版本
4. zookeeper 版本为 3.4.10


## Configuration

1. 下载zookeeper至本地任意文件夹
2. 更新vars/zk.yml 中的 download_path 路径
```
---
# zookeeper version
zookeeper_version: 3.4.10

# zookeeper user                   
user: "zookeeper"
group: "zookeeper"

# zookeeper data path
data_dir: zookeeper_storage
zookeeper_log_path: "{{install_dir}}/zookeeper_log"  

# zookeeper port
leader_port: 12888
vote_port: 13888
client_port: 12181
jmx_port: 11911
random_port: "30001-65006"

firewall_ports:
  - "{{ leader_port }}"
  - "{{ vote_port }}"
  - "{{ client_port }}"
  - "{{ jmx_port }}"
  - "{{ random_port }}"

# env path
install_dir: "/home/{{ user }}"
download_path: "/home/pippo/Downloads/"               # your local download path
tmp_path: "/tmp"

# host group
zk_hosts: zookeeper                                   # the group define in hosts/host

```

## Install
1. check  zookeeper.yml
2. install_zk, config_zk, start_service, add_user, open_firewall 修改为false即可取消执行
```
---

- hosts: zookeeper

  user: root

  vars_files:

    - "vars/zk.yml"

  vars:

    install_zk: true      # install zookeeper

    config_zk: true       # config zookeeper

    start_service: true   # auto start zookeeper service

    add_user: true        # add zookeeper user 

    open_firewall: true   # open firewall

  roles:

    - user                # add user

    - zookeeper           # zookeeper

```
3. 安装zookeeper集群

```
ansible-playbook -i hosts zookeeper.yml

```


