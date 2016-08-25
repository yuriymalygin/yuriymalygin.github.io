---
title: Установка Android 6.0 на планшет Nexus 7 2013 (LTE)
updated: 2015-10-06 10:25
---

<div>Разработчики выложили в открытый доступ новый образ ОС Android версии 6.0 - <a href="https://developers.google.com/android/nexus/images" target="_blank" shape="rect">https://developers.google.com/android/nexus/images</a>. Ниже описаны шаги как поставить новую версию на планшет Nexus 7 2013 (LTE).</div>
<!--more-->

Подготовить инструменты для работы с Android:
<pre>ph@localhost:~$ sudo apt-get install android-tools-adb android-tools-fastboot</pre>
Подготовить планшет и включить режим для разработчика:
<em>Настройки – О планшете – Номер сборки [нажать 7 раз]</em>

Включить отладку по USB в меню разработчика:
<em>Настройки – Для разработчиков – Отладка по USB [on/off]</em>

Скачать новый образ операционной системы и подготовить для загрузки в устройство:
<pre>ph@localhost:~$ cd Downloads/

ph@localhost:~/Downloads$ wget https://dl.google.com/dl/android/aosp/razorg-mra58k-factory-7221a6d9.tgz

ph@localhost:~/Downloads$ tar -xf razorg-mra58k-factory-7221a6d9.tgz

ph@localhost:~/Downloads$ cd razorg-mra58k/</pre>
<div></div>
Подключиться к планшету средствами утилиты adb, принять подключение на стороне устройства и выполнить перезагрузку:
<pre>ph@localhost:~/Downloads/razorg-mra58k$ adb devices<br clear="none" />* daemon not running. starting it now on port 5037 *<br clear="none" />* daemon started successfully *<br clear="none" />List of devices attached <br clear="none" />0a474ccd offline
</pre>
![](http://gdriv.es/malygin/android6-tablet_incoming_req.png)
<br clear="none" />ph@localhost:~/Downloads/razorg-mra58k$ adb devices<br clear="none" />List of devices attached <br clear="none" />0a474ccd device
<pre>
ph@localhost:~/Downloads/razorg-mra58k$ adb reboot bootloader</pre>
Разблокировать загрузчик ОС:
<pre>ph@localhost:~/Downloads/razorg-mra58k$ fastboot devices
0a474ccd fastboot

ph@localhost:~/Downloads/razorg-mra58k$ fastboot oem unlock<br clear="none" />...<br clear="none" />(bootloader) Unlocking bootloader...<br clear="none" />(bootloader) erasing userdata...<br clear="none" />(bootloader) erasing userdata done<br clear="none" />(bootloader) erasing cache...<br clear="none" />(bootloader) erasing cache done<br clear="none" />(bootloader) Unlocking bootloader done!<br clear="none" />OKAY [ 45.759s]<br clear="none" />finished. total time: 45.759s</pre>
Загрузить новый образ ОС в планшет
<pre>ph@localhost:~/Downloads/razorg-mra58k$ ./flash-all.sh <br clear="none" />sending 'bootloader' (3911 KB)...<br clear="none" />OKAY [ 0.694s]<br clear="none" />writing 'bootloader'...<br clear="none" />OKAY [ 1.284s]<br clear="none" />finished. total time: 1.978s<br clear="none" />rebooting into bootloader...<br clear="none" />OKAY [ 0.008s]<br clear="none" />finished. total time: 0.008s<br clear="none" />&lt; waiting for device &gt;<br clear="none" />sending 'radio' (76038 KB)...<br clear="none" />OKAY [ 13.140s]<br clear="none" />writing 'radio'...<br clear="none" />OKAY [ 3.031s]<br clear="none" />finished. total time: 16.171s<br clear="none" />rebooting into bootloader...<br clear="none" />OKAY [ 0.007s]<br clear="none" />finished. total time: 0.007s<br clear="none" />&lt; waiting for device &gt;<br clear="none" />archive does not contain 'boot.sig'<br clear="none" />archive does not contain 'recovery.sig'<br clear="none" />archive does not contain 'system.sig'<br clear="none" />--------------------------------------------<br clear="none" />Bootloader Version...: FLO-04.05<br clear="none" />Baseband Version.....: DEB-Z00_2.44.0_0213<br clear="none" />Serial Number........: 0a474ccd<br clear="none" />--------------------------------------------<br clear="none" />checking product...<br clear="none" />OKAY [ 0.005s]<br clear="none" />checking version-bootloader...<br clear="none" />OKAY [ 0.006s]<br clear="none" />checking version-baseband...<br clear="none" />OKAY [ 0.005s]<br clear="none" />sending 'boot' (7448 KB)...<br clear="none" />OKAY [ 1.303s]<br clear="none" />writing 'boot'...<br clear="none" />OKAY [ 0.414s]<br clear="none" />sending 'recovery' (8194 KB)...<br clear="none" />OKAY [ 1.447s]<br clear="none" />writing 'recovery'...<br clear="none" />OKAY [ 0.326s]<br clear="none" />erasing 'system'...<br clear="none" />OKAY [ 1.511s]<br clear="none" />sending 'system' (843198 KB)...<br clear="none" />OKAY [144.961s]<br clear="none" />writing 'system'...<br clear="none" />OKAY [ 40.138s]<br clear="none" />erasing 'userdata'...<br clear="none" />OKAY [ 22.916s]<br clear="none" />formatting 'userdata' partition...<br clear="none" />Creating filesystem with parameters:<br clear="none" /> Size: 28856791040<br clear="none" /> Block size: 4096<br clear="none" /> Blocks per group: 32768<br clear="none" /> Inodes per group: 8192<br clear="none" /> Inode size: 256<br clear="none" /> Journal blocks: 32768<br clear="none" /> Label: <br clear="none" /> Blocks: 7045115<br clear="none" /> Block groups: 215<br clear="none" /> Reserved block group size: 1024<br clear="none" />Created filesystem with 11/1761280 inodes and 154578/7045115 blocks<br clear="none" />sending 'userdata' (139085 KB)...<br clear="none" />writing 'userdata'...<br clear="none" />OKAY [ 32.195s]<br clear="none" />erasing 'cache'...<br clear="none" />OKAY [ 0.397s]<br clear="none" />formatting 'cache' partition...<br clear="none" />Creating filesystem with parameters:<br clear="none" /> Size: 587202560<br clear="none" /> Block size: 4096<br clear="none" /> Blocks per group: 32768<br clear="none" /> Inodes per group: 7168<br clear="none" /> Inode size: 256<br clear="none" /> Journal blocks: 2240<br clear="none" /> Label: <br clear="none" /> Blocks: 143360<br clear="none" /> Block groups: 5<br clear="none" /> Reserved block group size: 39<br clear="none" />Created filesystem with 11/35840 inodes and 4616/143360 blocks<br clear="none" />sending 'cache' (10984 KB)...<br clear="none" />writing 'cache'...<br clear="none" />OKAY [ 2.416s]<br clear="none" />rebooting...<br clear="none" /><br clear="none" />finished. total time: 248.067s</pre>
После окончания загрузки прошивки и обновления планшет перезагрузится и начнет работу мастер первичной настройки, после которой необходимо:
<ul>
	<li>включить режим для разработчика в настройках (как это сделать см. выше) и подключиться и перезагрузить планшет средствами adb:
<pre>ph@localhost:~/Downloads/razorg-mra58k$ adb devices 
List of devices attached 
0a474ccd device

ph@localhost:~/Downloads/razorg-mra58k$ adb reboot bootloader</pre>
</li>
	<li>заблокировать загрузчик ОС и перезагрузить планшет:
<pre>ph@localhost:~/Downloads/razorg-mra58k$ fastboot oem lock
&lt; waiting for device &gt;
...
(bootloader) Locking bootloader...
(bootloader) Locking bootloader done!
OKAY [ 0.218s]
finished. total time: 0.218s

ph@localhost:~/Downloads/razorg-mra58k$ fastboot reboot
rebooting...

finished. total time: 0.006s</pre>
</li>
</ul>
![](http://gdriv.es/malygin/android6-about.jpg)

Устройство готово к работе. Конец.