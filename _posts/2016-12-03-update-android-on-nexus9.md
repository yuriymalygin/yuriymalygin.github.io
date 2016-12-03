---
title:  Нюансы обновления Android на HTC Nexus 9 LTE до версии 7.0.0 (NRD91N)
updated: 2016-12-03 18:41
---
Заводские образы ОС Android для устройств серии Nexus доступы на сайте <a href="https://developers.google.com/android/images?hl=ru"  target="_blank">developers.google.com</a>, в том числе и для планшета HTC Nexus 9 LTE (<a href="https://dl.google.com/dl/android/aosp/volantisg-nrd91n-factory-972fb42b.zip?hl=ru" target="_blank">volantisg</a>)

Для работы потребуется два инструмента:
<pre>
sudo apt-get install android-tools-adb android-tools-fastboot
</pre>

Стандартная инструкция по обновлению системы от Google выглядит довольно просто:
<ol>
  <li>включить режим разработчика на устройстве</li>
  <li>разрешить подключаться для отладки по USB</li>
  <li>перезагрузить устройство в меню загрузчика</li>
  <li>разблокировать загрузчик</li>
  <li>запустить скрипт прошивки (flash-all.sh) из архива с обновлением</li>
</ol>

Вот только скрипт flash-all.sh при попытке загрузить образ системного раздела завершается с ошибкой:
<pre>
...
sending 'system' (1414164 KB)...
FAILED (remote: data length is too large)
finished. total time: 4.181s
</pre>

Для решения этой проблемы придется разбить процесс загрузки образов для каждого раздела на этапы.

Итоговый процесс обновления прошивки выглядит немного иначе (после того как разрешена отладка по USB):
<ol>
  <li> скачать архив с обновлением и развернуть его содержимое
<pre>
wget https://dl.google.com/dl/android/aosp/volantisg-nrd91n-factory-972fb42b.zip
unzip volantisg-nrd91n-factory-972fb42b.zip
cd volantisg-nrd91n/
</pre>
  <li> развернуть содержимое архива с образами для разделов в текущий каталог
<pre>
unzip image-volantisg-nrd91n.zip
</pre>
  <li> перезагрузить планшет в меню загрузчика
<pre>
adb reboot bootloader
</pre>
  <li> разблокировать загрузчик (если на устройстве уже Android 7, то заранее разрешить это в меню разработчика)
<pre>
fastboot oem unlock
</pre>
  <li> обновить загрузчик и перезапустить его
<pre>
fastboot flash bootloader bootloader-flounder_lte-3.48.0.0141.img
fastboot reboot-bootloader
</pre>
  <li> поочередно обновит все разделы с из новых образов
<pre>
fastboot flash boot boot.img
fastboot flash recovery recovery.img
fastboot flash system system.img
fastboot flash cache cache.img
fastboot flash vendor vendor.img
</pre>
  <li> перезагрузить устройство для начала установки новой системы
<pre>
fastboot reboot
</pre>

