---
title:  Очистка сообщений в топике Apache Kafka
updated: 2016-11-14 17:40
---
Для удаления данных в топике Apache Kafka необходимо выполнить следующие команды:

 1. уменьшить время жизни сообщений в топике

     kafka-topics.sh --zookeeper srv1:2181,srv2:2181,srv3:2181 --alter --topic temporaryQueue --config retention.ms=5000

 2. дождаться автоматической очистки очереди сообщений в топике (5 минут) и проверить очередь

   kafka-console-consumer.sh ---zookeeper srv1:2181,srv2:2181,srv3:2181 --topic temporaryQueue --from-beginning | wc -l
^CConsumed 0 messages

 3. восстановить конфигурацию 

     kafka-topics.sh --zookeeper srv1:2181,srv2:2181,srv3:2181 --alter --topic vpnUserActivityStream --delete-config retention.ms