# 7doitnow_infra
7doitnow Infra repository

## First month

2. [HW2. Introduction](#201911_hw2)

3. [HW3. Git basic](#201911_hw3)

4. [HW4. ChatOps](#201911_hw4)

5. [HW5. Cloud Bastion](#201911_hw5)
   + [SSL](#201911_hw5_ssl)
     + [Additional task](#201911_hw5_ssl_a)
   + [VPN](#201911_hw5_vpn)

6. [HW6. Cloud testapp](#201911_hw6)
   + [Additional task](#201911_hw6_a)

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

## <a name="201911_hw6">HW6. Cloud testapp </a>

testapp_IP = 35.228.226.109
testapp_port = 9292

check gloud utils
```
MacBook-Pro-Polina:7doitnow_infra polina$ sudo chown -R polina ~/.config/gcloud
MacBook-Pro-Polina:7doitnow_infra polina$ gcloud auth list
  Credentialed Accounts
ACTIVE  ACCOUNT
*       someemailaddress@gmail.com

To set the active account, run:
    $ gcloud config set account `ACCOUNT`

```

created the machine

```
NAME        ZONE             MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
reddit-app  europe-north1-b  g1-small                   10.166.0.3   35.228.36.236  RUNNING

```

```
MacBook-Pro-Polina:7doitnow_infra polina$ tail -1 /etc/hosts
35.228.226.109	reddit-app

ssh -i ~/.ssh/appuser -A appuser@reddit-app
```

Пума запустилась
```
appuser@reddit-app:~/reddit$ puma -d
Puma starting in single mode...
* Version 3.10.0 (ruby 2.3.1-p112), codename: Russell's Teapot
* Min threads: 0, max threads: 16
* Environment: development
* Daemonizing...
appuser@reddit-app:~/reddit$ ps aux | grep puma
appuser  21909  4.5  1.5 515388 26776 ?        Sl   19:00   0:00 puma 3.10.0 (tcp://0.0.0.0:9292) [reddit]
appuser  21923  0.0  0.0  12916   984 pts/0    S+   19:01   0:00 grep --color=auto puma
```

### <a name="201911_hw6_a">Additional task</a>


+ Создание vm c помощью локального стартап файлика
```
gcloud compute instances create reddit-app\
  --boot-disk-size=10GB \
  --image-family ubuntu-1604-lts \
  --image-project=ubuntu-os-cloud \
  --machine-type=g1-small \
  --tags puma-server \
  --restart-on-failure \
  --metadata-from-file startup-script=startup_script.sh

```
+ Создание vm c помощью файлика, который указан через URL

```
gcloud compute instances create reddit-app\
  --boot-disk-size=10GB \
  --image-family ubuntu-1604-lts \
  --image-project=ubuntu-os-cloud \
  --machine-type=g1-small \
  --tags puma-server \
  --restart-on-failure \
  --metadata startup-script-url=https://github.com/Otus-DevOps-2019-11/7doitnow_infra/blob/cloud-testapp/startup_script.sh
```

Правило фаервола

```
gcloud compute firewall-rules create default-puma-server \
  --allow tcp:9292
  --target-tags=puma-server
s
```


##### Happy end!
