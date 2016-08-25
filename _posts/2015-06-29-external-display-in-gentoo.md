---
title: Внешний монитор и спящий режим в Gentoo Linux
updated: 2015-06-29 16:34
---

ЗАДАЧА: <em>при использовании внешнего монитора отключать экран ноутбука, а при отключении монитора - автоматически включать экран обратно, учесть что при использовании спящего режима, ноутбук должен просыпаться с включенным экраном, если внешнего монитора больше нет.</em>

Определение наличия внешнего монитора
<pre><code>ph@localhost ~ $ cat .bin/second_display  
#!/bin/sh
# Connect external monitor to laptop

LAPTOPDISPLAY=eDP1  
HDMIDISPLAY=HDMI1

export DISPLAY=:0  
export XAUTHORITY=~ph/.Xauthority

function connect(){
  xrandr --output $HDMIDISPLAY --auto &amp;&amp; xrandr --output $LAPTOPDISPLAY --off
}

function disconnect(){
  xrandr --output $LAPTOPDISPLAY --auto &amp;&amp; xrandr --output $HDMIDISPLAY --off 
}

# Check second monitor
xrandr | grep --silent "$HDMIDISPLAY connected" &amp;&amp; connect || disconnect

# Reload background
su ph -c 'feh --bg-scale ~ph/.background'

#  Reload Conky position
kill -HUP $(pidof conky)  
</code></pre>
<p id="udev">Проверка событий Udev при отключении\подключении монитора</p>

<pre><code>ph@localhost ~ $ udevadm monitor  
monitor will print the received events for:  
UDEV - the event which udev sends out after rule processing  
KERNEL - the kernel uevent

# disconnect external monitor
KERNEL[1491.297884] change   /devices/pci0000:00/0000:00:02.0/drm/card0 (drm)  
KERNEL[1491.774172] change   /devices/pci0000:00/0000:00:02.0/drm/card0/card0-eDP-1/intel_backlight (backlight)

# connect external monitor
UDEV  [1491.871364] change   /devices/pci0000:00/0000:00:02.0/drm/card0 (drm)  
UDEV  [1491.871506] change   /devices/pci0000:00/0000:00:02.0/drm/card0/card0-eDP-1/intel_backlight (backlight)  
KERNEL[1496.636560] change   /devices/pci0000:00/0000:00:02.0/drm/card0 (drm)  
KERNEL[1496.942118] change   /devices/pci0000:00/0000:00:02.0/drm/card0/card0-eDP-1/intel_backlight (backlight)  
UDEV  [1497.241464] change   /devices/pci0000:00/0000:00:02.0/drm/card0 (drm)  
UDEV  [1497.241485] change   /devices/pci0000:00/0000:00:02.0/drm/card0/card0-eDP-1/intel_backlight (backlight)  
</code></pre>
<p id="udev">Правило Udev для автоматизации переключения экранов</p>

<pre><code>ph@localhost ~ $ cat /etc/udev/rules.d/80-my-display.rules  
KERNEL=="card0", SUBSYSTEM=="drm", RUN+="/home/ph/.bin/second_display"  
</code></pre>
<p id="acpi">Обработка ACPI-события в момент открытия\закрытия ноутбука</p>

<pre><code>ph@localhost ~ $ cat /etc/acpi/actions/lm_lid.sh  
#!/bin/sh
...
# Detect external monitor
/home/ph/.bin/second_display
</code></pre>
Готово! Можно проверять.