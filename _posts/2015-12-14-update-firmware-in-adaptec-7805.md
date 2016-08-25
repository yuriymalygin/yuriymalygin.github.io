---
title: Обновление прошивки RAID-контроллера Adaptec 7805
updated: 2015-12-14 08:43
---

Обновление прошивки RAID-контроллера Adaptec 7805 из под Linux средствами утилиты arcconf:
<ul>
	<li>собрать информацию о текущей версии прошивки raid-контроллера</li>
</ul>
<pre>[root@srv ~]# arcconf getconfig 1 ad
...
--------------------------------------------------------
 Controller Version Information
 --------------------------------------------------------
 BIOS : 7.5-0 (32033)
 Firmware : 7.5-0 (32033)
 Driver : 1.2-0 (30300)
 Boot Flash : 7.5-0 (32084)
 CPLD (Load version/ Flash version) : 8/ 10
 SEEPROM (Load version/ Flash version) : 1/ 1
...
Command completed successfully.</pre>
<ul>
	<li>скачать актуальную версию прошивки с офф-сайта <a href="http://www.adaptec.com/en-us/downloads/bios_fw/bios_fw_ver/productid=sas-7805&amp;dn=adaptec+raid+7805.php" target="_blank">Adaptec</a></li>
	<li>развернуть архив и скопировать образ прошивки (ufi-файл) на сервер</li>
	<li>прошить raid-контроллер утилитой arcconf, которая также доступна для скачивания на сайте <a href="http://www.adaptec.com/en-us/downloads/storage_manager/sm/productid=sas-7805&amp;dn=adaptec+raid+7805.php" target="_blank">Adaptec</a></li>
</ul>
<ol>
	<li>
<pre>[root@srv ~]# arcconf romupdate 1 /tmp/as780501.ufi
 Controllers found: 1

Are you sure you want to continue?
 Press y, then ENTER to continue or press ENTER to abort: y

Updating controller 1 firmware...Succeeded
 A new software image has been applied to controller 1.
 You must restart the system for firmware updates to take effect.

Command completed successfully.</pre>
</li>
</ol>
<ul>
	<li>перезагрузить сервер</li>
	<li>проверить версии прошивок после загрузки сервера</li>
</ul>
<pre>[root@srv ~]# arcconf getconfig 1 ad
...
--------------------------------------------------------
 Controller Version Information
 --------------------------------------------------------
 BIOS : 7.5-0 (32084)
 Firmware : 7.5-0 (32084)
 Driver : 1.2-0 (30300)
 Boot Flash : 7.5-0 (32084)
 CPLD (Load version/ Flash version) : 8/ 10
 SEEPROM (Load version/ Flash version) : 1/ 1
...
Command completed successfully.</pre>
<ul>
	<li>прошивка завершена успешно</li>
</ul>
&nbsp;