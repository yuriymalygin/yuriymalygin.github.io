---
title: Красивая замена 'grep -v grep'
updated: 2015-12-09 10:40
---

По работе (и не только) часто приходится искать определенный запущенный процесс в Linux (Unix) по имени или маске, и для этой задачи использовал классическую связку:
<pre> ps ax | grep &lt;имя процесса&gt;</pre>
Данный способ прост, но не удобен тем что среди вывода всегда есть и сам процесс <em>grep</em>, поэтому приходилось его убирать и тогда команда уже принимает вид:
<pre>ps ax | grep -v grep | grep &lt;имя процесса&gt;</pre>
Не элегантно и не красиво!

Есть способ лучше:
<pre> ps ax | grep &lt;[и]мя процесса&gt;</pre>
Примеры:
<pre>localhost:~ ph$ ps ax | grep syslog
 44 ?? Ss 1:36.11 /usr/sbin/syslogd
 17725 s002 R+ 0:00.00 grep syslog</pre>
<pre>localhost:~ ph$ ps ax | grep -v grep | grep syslog
 44 ?? Us 1:36.12 /usr/sbin/syslogd</pre>
<pre>localhost:~ ph$ ps ax | grep [s]yslog
 44 ?? Us 1:36.12 /usr/sbin/syslogd</pre>
Просто и красиво.