---
title: Ошибки 'ext3_lookup&#058; deleted inode referenced' в системных логах Linux
updated: 2015-12-24 09:57
---

В системного логе ОС встречаются сообщения следующего вида:
<pre>Dec 23 13:29:18 sandbox kernel: EXT3-fs error (device md1): ext3_lookup: deleted inode referenced: 5096436</pre>
Для исправления ошибки в файловой системе необходимо выполнить ее проверку средствами утилиты fsck, но из-за того что проблема на корневом разделе придется перезагружать систему и выполнять проверку во время загрузки ОС.

Чтобы fsck выполнил проверку раздела во время загрузки, необходимо создать для него файл-метку в корне раздела, который надо проверить и перезагрузить систему:

<pre>[root@sandbox ~]# touch /forcefsck

[root@sandbox ~]# reboot</pre>

Во время загрузки ОС будет видно как запустилась проверка файловой системы:
<pre>Activating swap-devices in /etc/fstab... done
/bin/mknod -m600 /dev/shm/root b 9 1
Checking root file system...
fsck 1.41.14 (22-Dec-2010)
/dev/shm/root: Entry 'blkid.tab.old' in /etc (5095425) has deleted/unused inode 5096436. CLEARED.
/dev/shm/root: ***** REBOOT LINUX *****
/dev/shm/root: 225962/7528448 files (0.4% non-contiguous), 3096112/301done8 blocks

fsck succeed, but reboot is required.

Restarting system.</pre>
После того как система загрузится файла-метки не будет, т.к. fsck его удаляет по завершении проверки.