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

bastion_IP = 35.224.7.247
someinternalhost_IP = 10.128.0.4

### <a name="201911_hw5_ssl">SSL </a>

Activated ssh forwarding
```
$ ssh-keygen -t rsa -f ~/.ssh/appuser -C appuser -P "35.224.7.247"
$ ssh-add -L
The agent has no identities.
$ ssh-add ~/.ssh/appuser
ssh -i ~/.ssh/appuser -A appuser@35.224.7.247
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
  Hostname 35.224.7.247
  User appuser
  IdentityFile ~/.ssh/appuser.pub

Host someinternalhost
  Hostname 10.128.0.4
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

Порт сервера pritunl - 17157

Установила оф. клиент pritunl.
Подключилась по ssh.

```
MacBook-Pro-Polina:7doitnow_infra polina$ ssh -i ~/.ssh/appuser appuser@10.128.0.4
```

Успешно подключилась

```

https://35.224.7.274.sslip.io/

ssl - настроила. :D
