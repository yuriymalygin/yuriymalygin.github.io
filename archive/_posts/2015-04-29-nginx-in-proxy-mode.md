---
title: Настройка NGINX в режиме проксирования
updated: 2015-04-29 21:07
---

<em>Иногда встречаются задачи поднятия платформы Node.js для запуска других web-приложений, которое должно общаться с внешним миром. В таких случаях наиболее удобным и безопастным вариантом будет запуск web-приложене на localhost, а обработку входящих запросов из внешнего мира передать отдельной системе. Ниже пойдет речь о том как быстро настроить Nginx в Centos 7 для проксирования внешнего трафика на localhost.</em><!--more-->
<h1>Установка</h1>
Для начала работ необходимо установить Nginx и добавить его в автозагрузку:
<pre><code>sudo yum install nginx  
sudo systemctl enable nginx.service  
</code></pre>
<h1>Конфигурация</h1>
Для настройки необходимо создать новый конфигурационный файл Nginx <code>/etc/nginx/nginx.conf</code> с двумя контекстами:
<ul>
	<li>Events, в котором задаются директивы влияющие на обработку соединений:</li>
</ul>
<pre><code>events {  
  worker_connections 1024;
}
</code></pre>
<ul>
	<li>Http, отвечающий за основную часть конфигурации HTTP-сервера:</li>
</ul>
<pre><code>http {  
  server {
    listen 80 default_server;
    server_name malyg.in;

    location / {
      proxy_pass              http://127.0.0.1:8080;
    }
  }
}
</code></pre>
Приведенная выше конфигурация описывает работу Nginx в режиме проксирования, а именно - принимать все запросы с 80 порта и передавать на на 8080-ый порт localhost, где запущено основное web-приложение.
<h1>Тестирование и запуск</h1>
Перед запуском Nginx, необходимо протестировать обновленную конфигурацию следующей командой:
<pre><code>nginx -t -c /etc/nginx/nginx.conf  
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok  
nginx: configuration file /etc/nginx/nginx.conf test is successful  
</code></pre>
Тест пройден без ошибок, поэтому запускаем сервис и проверяем логи:
<pre><code>sudo systemctl restart nginx.service  
less /var/log/nginx/error.log  
</code></pre>
После запуска сервиса Nginx все запросы приходящие на 80-ый порт будут транслироваться на localhost:8080.