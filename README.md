# k3s_demo

## 环境准备
两台以上VM，假设hostname:IP分别为：
```
ubuntu-11: 192.168.0.11
ubuntu-12: 192.168.0.12
```
注意：hostname不能冲突

## 下载二进制

```
wget https://github.com/rancher/k3s/releases/download/v0.4.0/k3s -O /usr/local/bin/k3s
```

注意：拷贝二进制至每个VM下面；

## 下载配置文件

下载本项目的文件，如:

```
git clone https://github.com/lixiangyun/k3s_demo
```

假设目录为`/root/k3s_demo`

## 启动k3s server

登录ubuntu-11，执行如下操作：

```
cd /root/k3s_demo
touch /etc/systemd/system/k3s.service.env
cp k3s.service /etc/systemd/system/k3s.service
systemctl daemon-reload
systemctl enable k3s.service
systemctl start k3s.service
systemctl status k3s.service
```

如果启动状态OK，则k3s server启动成功。

将ubuntu-11上面生成node-token拷贝agent上面，如：

```
scp /var/lib/rancher/k3s/server/node-token root@192.168.0.12:/root/k3s_demo
```

## 启动k3s agent

添加ubuntu-11的IP地址添加到/etc/hosts

```
echo "192.168.0.11 k3s.server" >> /etc/hosts
```

将k3s.agent.service文件保存到系统文件中，并且启动。

```
touch /etc/systemd/system/k3s.service.env
echo "NODE_TOKEN=$(cat /root/k3s_demo/node-token)" > /etc/systemd/system/k3s.service.env
cp /root/k3s_demo/k3s.agent.service /etc/systemd/system/k3s.agent.service
```

```
systemctl daemon-reload
systemctl enable k3s.agent.service
systemctl start k3s.agent.service
systemctl status k3s.agent.service
```

## 查看状态
```
k3s kubectl get node
```



