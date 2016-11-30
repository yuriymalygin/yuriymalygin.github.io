---
title:  Добавление и удаление ключевой фразы без обновления ssh-ключа
updated: 2016-11-30 10:35
---
Чтобы добавить/удалить/поменять ключевую фразу *без обновления* ssh-ключа, то достаточно выполнить следующую команду:

    ph@t460:~$ ssh-keygen -p
    Enter file in which the key is (/home/ph/.ssh/id_rsa): 
    Enter old passphrase: 
    Key has comment 'rsa w/o comment'
    Enter new passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved with the new passphrase.

Изменения сразу можно проверить, например подключившись к любому серверу где используется авторизация по ключу:

    ph@t460:~$ ssh srv1
    Enter passphrase for key '/home/ph/.ssh/id_rsa': 
    Last login: Wed Nov 30 10:42:31 2016 from x.x.x.x

    [ph@srv1 ~]$ 

