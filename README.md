# 7doitnow_infra
7doitnow Infra repository




#Добавим приватный ключ в ssh агент авторизации:
#activate ssh forwarding
$ ssh-add -L
#The agent has no identities.
$ ssh-add ~/.ssh/appuser

ssh -i ~/.ssh/appuser -A appuser@146.148.80.202
