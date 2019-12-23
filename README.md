# 7doitnow_infra
7doitnow Infra repository

## First month

1. [HW2. Introduction](#201911_hw2)

2. [HW3. Git basic](#201911_hw3)

3. [HW4. ChatOps](#201911_hw4)

4. [HW5. Cloud Bastion](#201911_hw5)
   + [SSL](#201911_hw5_ssl)
     + [Additional task](#201911_hw5_ssl_a)
   + [VPN](#201911_hw5_sb)

## <a name="201911_hw3">HW3</a>
Nothing to do

## <a name="201911_hw3">HW3. Git basic</a>
git basic

Pull request

## <a name="201911_hw4">HW4. Chatops</a>
chatops

Pull request

## <a name="201911_hw5">HW5. Cloud Bastion </a>
**Cloud bastion**

First of all...

```
MacBook-Pro-Polina:7doitnow_infra polina$ tail -2 /etc/hosts
34.94.180.123	bastion
10.168.0.3	someinternalhost
```

bastion_IP = 34.94.180.123
someinternalhost_IP = 10.168.0.3

### <a name="201911_hw5_ssl">SSL </a>

Activated ssh forwarding
```
$ ssh-keygen -t rsa -f ~/.ssh/appuser -C appuser -P ""
$ ssh-add -L
The agent has no identities.
$ ssh-add ~/.ssh/appuser
ssh -i ~/.ssh/appuser -A appuser@34.94.180.123
```

### <a name="201911_hw5_ssl_a">Additional task</a>


1. >ToDo: Подключиться к someinternalhost в одну команду. Предложить вариант решения для подключения из консоли при помощи команды вида ssh someinternalhost


* Первый способ. Это одна команда с аргументом.
```
MacBook-Pro-Polina:~ polina$ ssh -i ~/.ssh/appuser -A appuser@bastion 'ssh -tt appuser@someinternalhost'
```

* Второй способ. Настроить файлик ~/.ssh/config

```
MacBook-Pro-Polina:7doitnow_infra polina$ cat ~/.ssh/config
Host bastion
  Hostname 34.94.180.123
  User appuser
  IdentityFile ~/.ssh/appuser.pub

Host someinternalhost
  Hostname 10.168.0.3
  User appuser
  ProxyCommand ssh -W %h:%p bastion
  IdentityFile ~/.ssh/appuser.pub
```
Запуск

```
MacBook-Pro-Polina:7doitnow_infra polina$ ssh -L 7777:bastion:22 someinternalhost
```

debug:
```
lsof -i -P | grep LISTEN
netstat -a | grep LISTEN
ps aux |grep ssh
sudo kill id
```


### <a name="201911_hw5_vpn">VPN </a>

Установила оф. клиент pritunl.
Подключилась по ssh c ошибкой
```
MacBook-Pro-Polina:7doitnow_infra polina$ ssh -i ~/.ssh/appuser appuser@10.168.0.3
Tunnel device open failed.
Could not request tunnel forwarding.
```

Несмотря на ошибку в консоли успешно подключилась
```buildoutcfg
Mon Dec 23 20:42:41 2019 Opened utun device utun1
Mon Dec 23 20:42:41 2019 do_ifconfig, tt->did_ifconfig_ipv6_setup=0
Mon Dec 23 20:42:41 2019 /sbin/ifconfig utun1 delete
ifconfig: ioctl (SIOCDIFADDR): Can't assign requested address
Mon Dec 23 20:42:42 2019 NOTE: Tried to delete pre-existing tun/tap instance -- No Problem if failure
Mon Dec 23 20:42:42 2019 /sbin/ifconfig utun1 192.168.231.2 192.168.231.2 netmask 255.255.255.0 mtu 1500 up
add net 192.168.231.0: gateway 192.168.231.2
Mon Dec 23 20:42:42 2019 /tmp/pritunl/8c308393f6c48c0cb8bd7a076d6ab832-up.sh utun1 1500 1558 192.168.231.2 255.255.255.0 init
dhcp-option DNS 8.8.8.8
add net 34.94.180.123: gateway 192.168.1.1
add net 0.0.0.0: gateway 192.168.231.1
add net 128.0.0.0: gateway 192.168.231.1
Mon Dec 23 20:42:42 2019 Initialization Sequence Completed
```

https://34.94.180.123.sslip.io/

ssl - настроила. :D
