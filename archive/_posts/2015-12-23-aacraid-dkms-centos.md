---
title: Обновление модуля aacraid через DKMS в Centos 7
updated: 2015-12-23 12:31
---

Сборка и установка новой версии драйвера aacriad на сервере под управлением Centos с использованием механизма DKMS:
<ul>
	<li>скачать архив с DKMS на сайте <a href="http://www.adaptec.com/en-us/downloads/rh/rhel_7/productid=sas-7805&amp;dn=adaptec+raid+7805.php" target="_blank">Adaptec</a></li>
	<li>развернуть архив во временный каталог
<pre>[root@sandbox ~]# mkdir /tmp/aacraid

[root@sandbox ~]# tar -xf /tmp/aacraid-dkms-1.2.1.41024.tgz -C /tmp/accraid

[root@sandbox ~]# ls -1 /tmp/aacraid
GPL
aacraid-1.2.1.41024-dkms.noarch.rpm
dkms-2.2.0.3-1.noarch.rpm
readme.txt</pre>
</li>
	<li>установить исходники для сборки драйвера и перейти в рабочий каталог
<pre>[root@sandbox ~]# rpm -ihv /tmp/aacraid/aacraid-1.2.1.41024-dkms.noarch.rpm

[root@sandbox ~]# cd /usr/src/aacraid-1.2.1.41024</pre>
</li>
	<li>добавить информацию о новом драйвере в дерево DKMS и выполнить сборку
<pre>[root@sandbox aacraid-1.2.1.41024]# dkms add -m aacraid -v 1.2.1.41024

[root@sandbox aacraid-1.2.1.41024]# dkms build -m aacraid -v 1.2.1.41024
Module aacraid/1.2.1.41024 already built for kernel 3.10.0-229.11.1.el7.x86_64/4

[root@sandbox aacraid-1.2.1.41024]# dkms status
aacraid, 1.2.1.41024, 3.10.0-229.11.1.el7.x86_64, x86_64: installed (original_module exists)</pre>
</li>
	<li>убедиться что система  может корректно работать с новым модулем aacraid
<pre>[root@sandbox aacraid-1.2.1.41024]# modinfo -F version aacraid
1.2-1.41024dkms</pre>
</li>
	<li>пересобрать rpm-пакет для установки на рабочих серверах (необязательно)
<pre>[root@ph_rpmbuild aacraid-1.2.1.41024]# dkms mkrpm -m aacraid -v 1.2.1.41024

Marking 3.10.0-229.11.1.el7.x86_64 (x86_64) for RPM...
copying legacy postinstall template...
Copying source tree...
rpmbuild...

Wrote: /var/lib/dkms/aacraid/1.2.1.41024/rpm/aacraid-1.2.1.41024-1dkms.src.rpm
Wrote: /var/lib/dkms/aacraid/1.2.1.41024/rpm/aacraid-1.2.1.41024-1dkms.noarch.rpm

DKMS: mkrpm completed.</pre>
</li>
	<li> сборка и обновление драйвера завершена, осталось перезагрузить систему</li>
</ul>
Добавление обновленной версии модуля на серверах без DKMS:
<ul>
	<li>подготовить архив c модулем и распространить на нужные сервера, во временный каталог
<pre>[root@sandbox ~]# cd /lib/modules/3.10.0-229.11.1.el7.x86_64/extra/

[root@sandbox extra]# tar -czf /tmp/aacraid.tgz aacraid.ko</pre>
</li>
	<li>развернуть новую версию модуля их архива в каталог для новых версий драйверов дерева модулей (на каждом сервере)
<pre>[root@srvx ~]# tar -xf /tmp/aacraid.tgz -C /lib/modules/3.10.0-229.11.1.el7.x86_64/updates/</pre>
</li>
	<li>пересоздать зависимости между модулями (файл modules.dep) и убедиться что обновленная версия драйвера корректно определяется системой (на каждом сервере)
<pre>[root@srvx ~]# depmod -a

[root@srvx ~]# modinfo -n aacraid
/lib/modules/3.10.0-229.11.1.el7.x86_64/updates/aacraid.ko

[root@srvx ~]# modinfo -F version aacraid
1.2-1.41024dkms</pre>
</li>
	<li>пересоздать образ initramfs, для использования новой версии драйвера во время загрузки системы (на каждом сервере)
<pre>[root@srvx ~]# dracut --regenerate-all --force</pre>
</li>
	<li>перезагрузить систему на тех серверах, которые были обновлены</li>
	<li>после загрузки системы проверить какой драйвер использовался во время загрузки и какой драйвер виден через контроллер Adaptec
<pre> [root@srvx ~]# dmesg | grep Adaptec
 [ 5.253271] Adaptec aacraid driver 1.2-1.41024dkms

 [root@srvx ~]# arcconf getconfig 1 ad | grep Driver
 Driver : 1.2-1 (41024)</pre>
</li>
	<li>работы по обновлению драйвера aacraid завершены</li>
</ul>