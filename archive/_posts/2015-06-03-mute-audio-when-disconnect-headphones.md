---
title: Отключение звука при отключении наушников в Linux
updated: 2015-06-03 16:25
---

<em>Есть желание автоматически выключать звук при отключении наушников? Такое поведение можно добавить в несколько шагов. Ниже приведен пример как это сделать в Gentoo Linux.</em><!--more-->

Информация о звуковой карте:
<pre><code>ph@x1 ~ $ sudo lspci -s 00:1b.0 -v  
00:1b.0 Audio device: Intel Corporation 6 Series/C200 Series Chipset Family High Definition Audio Controller (rev 04)  
    Subsystem: Lenovo Device 21e8
    Flags: bus master, fast devsel, latency 0, IRQ 26
    Memory at f2620000 (64-bit, non-prefetchable) [size=16K]
    Capabilities: [50] Power Management version 2
    Capabilities: [60] MSI: Enable+ Count=1/1 Maskable- 64bit+
    Capabilities: [70] Express Root Complex Integrated Endpoint, MSI 00
    Capabilities: [100] Virtual Channel
    Capabilities: [130] Root Complex Link
    Kernel driver in use: snd_hda_intel
</code></pre>
Проверяем параметр в ядре, который отвечает за нотификации о подключении по jack-у:
<pre><code>ph@x1 ~ $ zgrep CONFIG_SND_HDA_INPUT_JACK /proc/config.gz  
CONFIG_SND_HDA_INPUT_JACK=y  
</code></pre>
Устанавливаем демон ACPI, если его нет:
<pre><code>ph@x1 ~ $ sudo acpid --version || sudo emerge -v sys-power/acpid

ph@x1 ~ $ sudo rc-service acpid start  
</code></pre>
Проверяем что доступна информация о событиях подключения наушников по minijack-у:
<pre><code>ph@x1 ~ $ acpi_listen  
jack/headphone HEADPHONE plug  
jack/headphone HEADPHONE unplug  
^C
</code></pre>
Добавить обработчик события ACPI:
<pre><code>ph@x1 ~ $ cat /etc/acpi/events/headphones  
event=jack/headphone*  
action=/etc/acpi/actions/headphones.sh %e  
</code></pre>
Добавить action-скрипт, который будет выполняться при наступлении события и сделать его исполняемым:
<pre><code>ph@x1 ~ $ cat /etc/acpi/actions/headphones.sh  
#!/bin/sh

case $3 in  
  plug) amixer -q set Master unmute ;;
  unplug) amixer -q set Master mute ;;
  *) echo "Usage: $0 {plug|unplug}" ;;
esac

ph@x1 ~ $ sudo chmod +x /etc/acpi/actions/headphones.sh  
</code></pre>
Готово! Проверяем результат - при отключении наушников звук будет выключен (mute), а при подключении - включен (unmute).