---
title: Обновление Android на планшете Nexus 7 2013 (LTE)
updated: 2015-05-29 21:15
---

Процесс обновления ОС Android до версии 5.1.1 или наоборот отката до старой версии, на планшете Nexus 7 от Google\Asus состоит из нескольких шагов:
<ul>
	<li>установить на компьютере набор инструментов для android-разработчиков</li>
</ul>
<pre><code>ph@x1 ~ $ sudo emerge dev-util/android-tools  
...
&gt;&gt;&gt; Installing (1 of 1) dev-util/android-tools-0_p20130218::gentoo
&gt;&gt;&gt; Auto-cleaning packages...

&gt;&gt;&gt; No outdated packages were found on your system.

 * GNU info directory index is up-to-date.

ph@x1 ~ $ whereis adb  
adb: /usr/bin/adb

ph@x1 ~ $ whereis fastboot  
fastboot: /usr/bin/fastboot
</code></pre>
<ul>
	<li>включить на планшете режим разработчика: <em>Настройки - О планшете - Номер сборки [нажать 7 раз]</em></li>
	<li>включить отладку по USB в меню разработчика: <em>Настройки - Для разработчиков - Отладка по USB [on/off]</em></li>
	<li>скачать архив (прошивку) с нужной версией ОС Android с сайта <a href="https://developers.google.com/android/nexus/images" target="_blank">Google Developers</a></li>
</ul>
<pre><code>ph@x1 ~ $ curl https://dl.google.com/dl/android/aosp/razorg-lmy47v-factory-f230ab31.tgz | tar -zxf - -C /tmp/  
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  502M  100  502M    0     0  22.8M      0  0:00:22  0:00:22 --:--:-- 22.6M

ph@x1 ~ $ cd /tmp/razorg-lmy47v/

ph@x1 /tmp/razorg-lmy47v $ ls -al  
total 552M  
drwxr-x---  2 ph   ph    160 May 20 18:39 .  
drwxrwxrwx 21 root root 1.4K May 29 10:16 ..  
-rw-r-----  1 ph   ph   3.9M May 20 18:39 bootloader-deb-flo-04.05.img
-rw-r-----  1 ph   ph    960 May 20 18:39 flash-all.bat
-rwxr-x--x  1 ph   ph    831 May 20 18:39 flash-all.sh
-rwxr-x--x  1 ph   ph    788 May 20 18:39 flash-base.sh
-rw-r-----  1 ph   ph   474M May 20 18:39 image-razorg-lmy47v.zip
-rw-r-----  1 ph   ph    75M May 20 18:39 radio-deb-deb-z00_2.44.0_0213.img
</code></pre>
<ul>
	<li>подключиться к планшету с компьютера используя утилиту adb (на экране планшета будет сообщение с вопросом о принятии цифрового идентификатора компьютера) и выполнить перезагрузку планшета</li>
</ul>
<pre><code>ph@x1 /tmp/razorg-lmy47v $ adb devices  
List of devices attached  
0a4d995e     device

ph@x1 /tmp/razorg-lmy47v $ adb reboot bootloader  
</code></pre>
<ul>
	<li>разблокировать загрузчик c удалением всех данных на планшете</li>
</ul>
<pre><code>ph@x1 /tmp/razorg-lmy47v $ sudo fastboot devices  
0a4d995e    fastboot

ph@x1 /tmp/razorg-lmy47v $ sudo fastboot oem unlock  
...
(bootloader) Unlocking bootloader...
(bootloader) erasing userdata...
(bootloader) erasing userdata done
(bootloader) erasing cache...
(bootloader) erasing cache done
(bootloader) Unlocking bootloader done!
OKAY [ 35.194s]  
finished. total time: 35.194s  
</code></pre>
<ul>
	<li>загрузить прошивку в планшет</li>
</ul>
<pre><code>ph@x1 /tmp/razorg-lmy47v $ sudo ./flash-all.sh  
sending 'bootloader' (3911 KB)...  
OKAY [  0.133s]  
writing 'bootloader'...  
OKAY [  1.847s]  
finished. total time: 1.980s  
rebooting into bootloader...  
OKAY [  0.006s]  
finished. total time: 0.006s  
sending 'radio' (76038 KB)...  
OKAY [  2.407s]  
writing 'radio'...  
OKAY [  3.039s]  
finished. total time: 5.446s  
rebooting into bootloader...  
OKAY [  0.005s]  
finished. total time: 0.005s  
archive does not contain 'boot.sig'  
archive does not contain 'recovery.sig'  
archive does not contain 'system.sig'  
--------------------------------------------
Bootloader Version...: FLO-04.05  
Baseband Version.....: DEB-Z00_2.44.0_0213  
Serial Number........: 0a4d995e  
--------------------------------------------
checking product...  
OKAY [  0.003s]  
checking version-bootloader...  
OKAY [  0.004s]  
checking version-baseband...  
OKAY [  0.004s]  
sending 'boot' (7204 KB)...  
OKAY [  0.234s]  
writing 'boot'...  
OKAY [  0.406s]  
sending 'recovery' (7810 KB)...  
OKAY [  0.256s]  
writing 'recovery'...  
OKAY [  0.298s]  
erasing 'system'...  
OKAY [  1.584s]  
sending 'system' (826710 KB)...  
OKAY [ 26.055s]  
writing 'system'...  
OKAY [ 38.221s]  
erasing 'userdata'...  
OKAY [ 19.336s]  
formatting 'userdata' partition...  
Creating filesystem with parameters:  
    Size: 28856791040
    Block size: 4096
    Blocks per group: 32768
    Inodes per group: 8192
    Inode size: 256
    Journal blocks: 32768
    Label: 
    Blocks: 7045115
    Block groups: 215
    Reserved block group size: 1024
Created filesystem with 11/1761280 inodes and 154578/7045115 blocks  
sending 'userdata' (139085 KB)...  
writing 'userdata'...  
OKAY [ 10.512s]  
erasing 'cache'...  
OKAY [  0.380s]  
formatting 'cache' partition...  
Creating filesystem with parameters:  
    Size: 587202560
    Block size: 4096
    Blocks per group: 32768
    Inodes per group: 7168
    Inode size: 256
    Journal blocks: 2240
    Label: 
    Blocks: 143360
    Block groups: 5
    Reserved block group size: 39
Created filesystem with 11/35840 inodes and 4616/143360 blocks  
sending 'cache' (10984 KB)...  
writing 'cache'...  
OKAY [  0.848s]  
rebooting...

finished. total time: 98.160s  
</code></pre>
<ul>
	<li>после загрузки планшета выполнить начальную настройку Android</li>
	<li>включить режим разработчика и отладку по USB (как это сделать см. выше)</li>
	<li>перезагрузить планшет и заблокировать загрузчик</li>
</ul>
<pre><code>ph@x1 /tmp/razorg-lmy47v $ adb reboot bootloader

ph@x1 /tmp/razorg-lmy47v $ sudo fastboot oem lock  </code></pre>