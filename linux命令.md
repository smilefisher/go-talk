#### 1 防火墙

```shell
# 1 关闭防火墙
systemctl stop firewalld

# 2 开启防火墙
systemctl start firewalld

# 3 查看开放端口
firewall-cmd --list-ports

# 4 开放3306端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent
--zone #作用域
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
--permanent  #永久生效，没有此参数重启后失效

# 5 需要重载一下
 firewall-cmd --reload

# 6 移除开放的端口
 firewall-cmd --zone=public --remove-port=3306/udp --permanent
```

