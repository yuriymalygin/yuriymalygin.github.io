---
title: Настройка Netflow в Centos 7
updated: 2015-10-08 14:26
---

<h1>Подготовка системы</h1>
<pre>[root@sandbox ~]# # yum update -y</pre>
<h1>Сборка и установка пакета инструментов NFDUMP</h1>
<pre>[root@sandbox ~]# yum install rrdtool-devel
[root@sandbox ~]# tar -xf nfdump-1.6.13.tar.gz
[root@sandbox ~]# cd nfdump-1.6.13/
[root@sandbox nfdump-1.6.13]# ./configure --prefix=/opt/nfdump-1.6.13 --enable-nfprofile
[root@sandbox nfdump-1.6.13]# make &amp;&amp; make check
[root@sandbox nfdump-1.6.13]# make install &amp;&amp; make install check
[root@sandbox nfdump-1.6.13]# find /opt/nfdump-1.6.13/
 /opt/nfdump-1.6.13/
 /opt/nfdump-1.6.13/bin
 /opt/nfdump-1.6.13/bin/nfcapd
 /opt/nfdump-1.6.13/bin/nfdump
 /opt/nfdump-1.6.13/bin/nfreplay
 /opt/nfdump-1.6.13/bin/nfexpire
 /opt/nfdump-1.6.13/bin/nfanon
 /opt/nfdump-1.6.13/bin/nfprofile
 /opt/nfdump-1.6.13/share
 /opt/nfdump-1.6.13/share/man
 /opt/nfdump-1.6.13/share/man/man1
 /opt/nfdump-1.6.13/share/man/man1/ft2nfdump.1
 /opt/nfdump-1.6.13/share/man/man1/nfcapd.1
 /opt/nfdump-1.6.13/share/man/man1/nfdump.1
 /opt/nfdump-1.6.13/share/man/man1/nfexpire.1
 /opt/nfdump-1.6.13/share/man/man1/nfprofile.1
 /opt/nfdump-1.6.13/share/man/man1/nfreplay.1
 /opt/nfdump-1.6.13/share/man/man1/nfanon.1
 /opt/nfdump-1.6.13/share/man/man1/sfcapd.1</pre>
Подготовка архива для переноса на рабочий сервер:
<pre>[root@sandbox ~]# cd /opt/
[root@sandbox opt]# tar -czf /root/nfdump.tgz nfdump-1.6.13/</pre>
Развертывание NFDUMP на рабочем сервере:
<pre>[root@srvX ~]# tar -xf /tmp/nfdump.tgz -C /opt/
[root@srvX ~]# /opt/nfdump-1.6.13/bin/nfcapd -V
/opt/nfdump-1.6.13/bin/nfcapd: Version: 1.6.13</pre>
<h1>Установка и настройка NfSen</h1>
Подготовка хранилища для данных:
<pre>[root@srvX ~]# mkdir /mnt/netflow
[root@srvX ~]# echo '/dev/sdb1 /mnt/netflow auto defaults 0 0' &gt;&gt;/etc/fstab
[root@srvX ~]# mount -a &amp;&amp; df -h /mnt/netflow
 Filesystem Size Used Avail Use% Mounted on
 /dev/sdb1 6.4T 33M 6.4T 1% /mnt/netflow</pre>
Первичная установка NfSen из архива:
<pre>[root@srvX ~]# tar -xf /tmp/nfsen-1.3.6p1.tar.gz -C /usr/src/
[root@srvX ~]# cd /usr/src/nfsen-1.3.6p1/
[root@srvX nfsen-1.3.6p1]# cp etc/nfsen-dist.conf etc/nfsen.conf</pre>
Подготовка конфигурации для рабочей установки:
<pre>[root@srvX nfsen-1.3.6p1]# vim etc/nfsen.conf
 $BASEDIR='/opt/nfsen';
 $HTMLDIR='/var/www/html/nfsen';
 $PROFILEDATADIR='/mnt/netflow/profiles-data';
 $PREFIX='/opt/nfdump-1.6.13/bin';
 $WWWUSER='apache';
 $WWWGROUP='apache';
 %sources = (
   'sw1' =&gt;{'port' =&gt; 9996, 'IP' =&gt; '1.1.1.1', 'optarg' =&gt; ' -s 2500', 'col' =&gt; '#0000ff' },
   'sw2' =&gt;{'port' =&gt; 9996, 'IP' =&gt; '2.2.2.2', 'optarg' =&gt; ' -s 2500', 'col' =&gt; '#00ff00' },
   'sw3' =&gt;{'port' =&gt; 9996, 'IP' =&gt; '3.3.3.3', 'optarg' =&gt; ' -s 2500', 'col' =&gt; '#ff0000' },
 );</pre>
Установка зависимостей для полноценной работы NfSen:
<pre>[root@srvX nfsen-1.3.6p1]# yum install httpd php rrdtool-perl
[root@srvX nfsen-1.3.6p1]# yum install perl-MailTools perl-Socket6 perl-Sys-Syslog</pre>
Добавить отдельного пользователя под которым будут работать NfSen в связке с инструментами из пакета NFDUMP:
<pre>[root@srvX nfsen-1.3.6p1]# useradd -c 'Netflow user' -G apache netflow</pre>
Указать правильную часовую зону для PHP:
<pre>[root@srvX nfsen-1.3.6p1]# vim /etc/php.ini
date.timezone = Europe/Moscow</pre>
Запустить perl-скрипт первичной установки:
<pre>[root@srvX nfsen-1.3.6p1]# ./install.pl etc/nfsen.conf
 Check for required Perl modules: All modules found.
 Setup NfSen:
 Version: 1.3.6p1: $Id: install.pl 53 2012-01-23 16:36:02Z peter $

Perl to use: [/usr/bin/perl]
 Found /opt/nfdump-1.6.13/bin/nfdump: Version: 1.6.13
 Setup php and html files.
 ...
 Rebuilding profile stats for './live'
 Reconfig: No changes found!
 Setup done.</pre>
Обновить NfSen до последней доступной версии 1.3.7 (не обязательно):
<pre>[root@srvX nfsen-1.3.6p1]# tar -xf /tmp/nfsen-1.3.7.tar.gz -C /usr/src/
[root@srvX ~]# cd ../nfsen-1.3.7/
[root@srvX nfsen-1.3.7]# cp ../nfsen-1.3.6p1/etc/nfsen.conf etc/
[root@srvX nfsen-1.3.7]# ./install.pl etc/nfsen.conf
 Check for required Perl modules: All modules found.
 Upgrade from version '1.3.6p1' installed at Thu Oct 8 14:47:06 2015
 ...
 Setup done.</pre>
Добавить символьную ссылку, чтобы не смотреть содержимое каталога при обращении по адресу http://srvX/nfsen:
<pre>[root@srvX ~]# cd /var/www/html/nfsen/
[root@srvX nfsen]# ln -s nfsen.php index.php</pre>
Запуск Apache HTTP server и проверка его работоспособности:
<pre>[root@srvX ~]# systemctl start httpd.service
[root@srvX ~]# systemctl enable httpd.service

[root@srvX ~]# netstat -nlp | grep httpd
 tcp 0 0 0.0.0.0:80 0.0.0.0:* LISTEN 6114/httpd</pre>
Настройка редиректа в Apache:
<pre>[root@srvX ~]# vim /etc/httpd/conf/httpd.conf
&lt;Directory /&gt;
 RedirectMatch ^/$ /nfsen/nfsen.php
...
&lt;/Directory&gt;</pre>
<h1>Управление NfSen</h1>
Запуск:
<pre>[root@srvX ~]# /opt/nfsen/bin/nfsen start</pre>
Обновление конфигурации "на лету":
<pre>[root@srvX ~]# vim /opt/nfsen/etc/nfsen.conf
[root@srvX ~]# /opt/nfsen/bin/nfsen reconfig</pre>
Остановка:
<pre>[root@srvX ~]# /opt/nfsen/bin/nfsen stop</pre>
Обновление конфигурации при остановленном NfSen:
<pre>[root@srvX ~]# vim /opt/nfsen/etc/nfsen.conf
[root@srvX ~]# /opt/nfsen/bin/nfsen reconfig
[root@srvX ~]# /opt/nfsen/bin/nfsen start</pre>
<h1>Итоги</h1>
После сборки, переноса на рабочий сервер и настройки\установки NfSen, имеем web-инструмент по адресу http://srvX, который умеет работать с собранными дампами Netflow от сетевого оборудования.