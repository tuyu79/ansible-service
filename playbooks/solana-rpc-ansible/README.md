# 执行 playbook 前还需要挂载磁盘到对应目录
```shell
# 格式化
df -h
lsblk -f
# 查看设备是否存在
# ls -l /dev/nvme2n1
# 格式化磁盘
sudo mkfs -t ext4 /dev/nvme2n1
sudo mkfs -t ext4 /dev/nvme1n1

# 创建目录并挂载磁盘到目录下
sudo mkdir -p /solana/ledger
sudo mount /dev/nvme0n1p1 /solana/ledger

sudo mkdir -p /solana/accounts
sudo mount /dev/nvme0n1p1 /solana/accounts

# 取消挂载
#sudo umount /dev/nvme0n1p1
#sudo umount /dev/nvme1n1p1

df -h
```

# 部署后将目录的所有权赋给 solana 用户
```shell
sudo chown -R solana:solana /solana/ledger
sudo chown -R solana:solana /solana/accounts
```

# 启动
```shell
# 切换到 solana 用户
sudo su -l solana

# 编辑启动命令, 第一次启动注释掉 --no-genesis-fetch 和 --no-snapshot-fetch, 后续再次启动时要把这两个注释打开
vi /home/solana/bin/solana-rpc.sh

# 启动
systemctl --user start solana-rpc
# 停止
systemctl --user stop solana-rpc
# 查看状态
solana catchup --our-localhost
# 查看状态
systemctl --user status solana-rpc
# 查看日志
journalctl --user -u solana-rpc -f

```

# 验证
```shell
curl http://127.0.0.1:8899 -X \
  POST -H "Content-Type: application/json" -d ' 
  {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getHealth"
  }
'
```