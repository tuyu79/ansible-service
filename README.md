# 复制公钥到服务器
```shell
ssh-copy-id ubuntu@160.202.128.69
```

# 执行命令
```shell
ansible-playbook -i inventory.ini ./playbooks/solana-rpc-ansible/solana-rpc-ansible.yml
```