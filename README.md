# 7doitnow_infra
7doitnow Infra repository

## First month

1. [HW2. Introduction](#201911_hw2)

2. [HW3. Git basic](#201911_hw3)

3. [HW4. ChatOps](#201911_hw4)

4. [HW5. Cloud Bastion](#201911_hw5)
   + [SSL](#201911_hw5_ssl)
     + [Additional task](#201911_hw5_ssl_a)
   + [VPN] (#201911_hw5_sb)
     + [Additional task](#201911_hw5__vpn_a)


## <a name="201911_hw3">HW3</a>
Nothing to do

## <a name="201911_hw3">HW3. Git basic</a>
git basic

[Pull request](https://pages.github.com/)

## <a name="201911_hw4">HW4. Chatops</a>
chatops

[Pull request](https://pages.github.com/)

## <a name="201911_hw5">HW5. Cloud Bastion </a>
**Cloud bastion**

First of all...

```
MacBook-Pro-Polina:7doitnow_infra polina$ tail -2 /etc/hosts
34.94.180.123	bastion
10.168.0.3	someinternalhost
```

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
sudo pritunl setup-key
6fb8ba8cdbb34337a84d6959125ca073

mongodb://localhost:27017/pritunl

[undefined][2019-12-22 18:47:13,423][INFO] Getting default administrator password
Administrator default password:
  username: "pritunl"
  password: "tVsgTq0xdyq0"

port 12165

#### <a name="201911_hw5_ssl_a">Additional task</a>
