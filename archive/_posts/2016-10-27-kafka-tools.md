---
title:  Просмотр сообщений в отдельных партициях топика Apache Kafka
updated: 2016-10-27 11:54
---
В случае когда необходимо убедиться что сообщения распределяются между всеми партициями топика в Apache Kafka можно использовать следующий класс из родного System Tools:

    kafka-run-class.sh kafka.tools.SimpleConsumerShell --broker-list srv1:9092,srv2:9092,srv3:9092 --topic syslogMessages --partition 0
    
Приведенная выше команда покажет все сообщения в нулевой партиции топика *syslogMessages*

Количество партиций выбранного топика можно посмотреть так:

    kafka-topics.sh --zookeeper srv1:2181,srv2:2181,srv3:2181 --describe --topic syslogMessages
    Topic:syslogMessages	PartitionCount:24	ReplicationFactor:2	Configs:
	Topic: syslogMessages	Partition: 0	Leader: 2	Replicas: 2,4	Isr: 4,2
	Topic: syslogMessages	Partition: 1	Leader: 3	Replicas: 3,1	Isr: 3,1
	Topic: syslogMessages	Partition: 2	Leader: 4	Replicas: 4,2	Isr: 4,2
	Topic: syslogMessages	Partition: 3	Leader: 1	Replicas: 1,3	Isr: 1,3
	Topic: syslogMessages	Partition: 4	Leader: 2	Replicas: 2,1	Isr: 1,2
	Topic: syslogMessages	Partition: 5	Leader: 3	Replicas: 3,2	Isr: 3,2
	Topic: syslogMessages	Partition: 6	Leader: 4	Replicas: 4,3	Isr: 4,3
	Topic: syslogMessages	Partition: 7	Leader: 1	Replicas: 1,4	Isr: 1,4
	Topic: syslogMessages	Partition: 8	Leader: 2	Replicas: 2,3	Isr: 3,2
	Topic: syslogMessages	Partition: 9	Leader: 3	Replicas: 3,4	Isr: 3,4
	Topic: syslogMessages	Partition: 10	Leader: 4	Replicas: 4,1	Isr: 1,4
	Topic: syslogMessages	Partition: 11	Leader: 1	Replicas: 1,2	Isr: 1,2
	Topic: syslogMessages	Partition: 12	Leader: 2	Replicas: 2,4	Isr: 4,2
	Topic: syslogMessages	Partition: 13	Leader: 3	Replicas: 3,1	Isr: 3,1
	Topic: syslogMessages	Partition: 14	Leader: 4	Replicas: 4,2	Isr: 4,2
	Topic: syslogMessages	Partition: 15	Leader: 1	Replicas: 1,3	Isr: 1,3
	Topic: syslogMessages	Partition: 16	Leader: 2	Replicas: 2,1	Isr: 1,2
	Topic: syslogMessages	Partition: 17	Leader: 3	Replicas: 3,2	Isr: 3,2
	Topic: syslogMessages	Partition: 18	Leader: 4	Replicas: 4,3	Isr: 4,3
	Topic: syslogMessages	Partition: 19	Leader: 1	Replicas: 1,4	Isr: 1,4
	Topic: syslogMessages	Partition: 20	Leader: 2	Replicas: 2,3	Isr: 3,2
	Topic: syslogMessages	Partition: 21	Leader: 3	Replicas: 3,4	Isr: 3,4
	Topic: syslogMessages	Partition: 22	Leader: 4	Replicas: 4,1	Isr: 1,4
	Topic: syslogMessages	Partition: 23	Leader: 1	Replicas: 1,2	Isr: 1,2
	

Более подробная информация о Kafka System Tools - [https://cwiki.apache.org/confluence/display/KAFKA/System+Tools](https://cwiki.apache.org/confluence/display/KAFKA/System+Tools)