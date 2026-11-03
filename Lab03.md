Lab03

estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ tar -xzf kafka_2.13-3.9.2.tgz && cd kafka_2.13-3.9.2/
ls bin | head; ls config/kraft
connect-distributed.sh
connect-mirror-maker.sh
connect-plugin-path.sh
connect-standalone.sh
kafka-acls.sh
kafka-broker-api-versions.sh
kafka-client-metrics.sh
kafka-cluster.sh
kafka-configs.sh
kafka-console-consumer.sh
broker.properties  controller.properties  reconfig-server.properties  server.properties
estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno/kafka_2.13-3.9.2$ export KAFKA_CLUSTER_ID=$(bin/kafka-storage.sh random-uuid)
echo $KAFKA_CLUSTER_ID
bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c config/kraft/server.properties
AnuT8M1rQtW2GGYkJ5_44g
Formatting metadata directory /tmp/kraft-combined-logs with metadata.version 3.9-IV0.

estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno/kafka_2.13-3.9.2$ bin/kafka-server-start.sh config/kraft/server.properties > /tmp/kafka-manual.log 2>&1 &
sleep 8
grep -E "Kafka Server started|KafkaRaftServer|Transition from|ERROR" /tmp/kafka-manual.log | head
[1] 73497
[1]+  Exit 1                  bin/kafka-server-start.sh config/kraft/server.properties > /tmp/kafka-manual.log 2>&1
[2026-10-29 13:41:48,237] INFO [BrokerServer id=1] Transition from SHUTDOWN to STARTING (kafka.server.BrokerServer)
[2026-10-29 13:41:48,885] ERROR Unable to start acceptor for ListenerName(PLAINTEXT) (kafka.network.DataPlaneAcceptor)
        at kafka.server.KafkaRaftServer.$anonfun$startup$2(KafkaRaftServer.scala:99)
        at kafka.server.KafkaRaftServer.$anonfun$startup$2$adapted(KafkaRaftServer.scala:99)
        at kafka.server.KafkaRaftServer.startup(KafkaRaftServer.scala:99)
[2026-10-29 13:41:48,891] ERROR [BrokerServer id=1] Received a fatal error while waiting for all of the SocketServer Acceptors to be started (kafka.server.BrokerServer)
        at kafka.server.KafkaRaftServer.$anonfun$startup$2(KafkaRaftServer.scala:99)
        at kafka.server.KafkaRaftServer.$anonfun$startup$2$adapted(KafkaRaftServer.scala:99)
        at kafka.server.KafkaRaftServer.startup(KafkaRaftServer.scala:99)
[2026-10-29 13:41:48,892] INFO [BrokerServer id=1] Transition from STARTING to STARTED (kafka.server.BrokerServer)

estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno/kafka_2.13-3.9.2$ ss -ltnp | grep -E "9092|9093"     # o: netstat -an | grep -E "9092|9093"
LISTEN 0      4096         0.0.0.0:9092       0.0.0.0:*                                         
LISTEN 0      4096            [::]:9092          [::]:*   

estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno/kafka_2.13-3.9.2$ bin/kafka-topics.sh --create --topic pruebaest2.instalacion --bootstrap-server localhost:9092
echo "hola desde la instalacion manual" | bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic pruebaest2.instalacion
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic pruebaest2.instalacion --from-beginning --timeout-ms 5000
bin/kafka-metadata-quorum.sh --bootstrap-server localhost:9092 describe --status | head -3
WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Created topic pruebaest2.instalacion.
hola desde la instalacion manual
[2026-10-29 13:46:42,098] ERROR Error processing message, terminating consumer process:  (org.apache.kafka.tools.consumer.ConsoleConsumer)
org.apache.kafka.common.errors.TimeoutException
Processed a total of 1 messages
ClusterId:              dian-kafka-lab-2026
LeaderId:               3
LeaderEpoch:            46

estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno/kafka_2.13-3.9.2$ bin/kafka-server-stop.sh; sleep 3; ss -ltnp | grep 9092 || echo "puerto 9092 libre"
bin/kafka-server-stop.sh: line 41: kill: (48325) - Operation not permitted
bin/kafka-server-stop.sh: line 41: kill: (48447) - Operation not permitted
bin/kafka-server-stop.sh: line 41: kill: (48838) - Operation not permitted
LISTEN 0      4096         0.0.0.0:9092       0.0.0.0:*                                         
LISTEN 0      4096            [::]:9092          [::]:*    

estudiante@ip-192-168-166-59:~/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-topics.sh --bootstrap-server broker1:19092 --describe --topic dian.auditoria.acceso | head -2
Topic: dian.auditoria.acceso    TopicId: LqlBOynyTDC8H5yiQe6L2Q PartitionCount: 6       ReplicationFactor: 3    Configs: min.insync.replicas=2,cleanup.policy=compact,unclean.leader.election.enable=false
        Topic: dian.auditoria.acceso    Partition: 0    Leader: 1       Replicas: 1,2,3 Isr: 2,3,1      Elr:    LastKnownElr: 

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 gen_eventos.py --topic dian.facturacion.emitida --rate 50 --minutes 2
enviados: 5964

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 \
/opt/kafka/bin/kafka-get-offsets.sh \
--bootstrap-server broker1:19092 \
--topic dian.facturacion.emitida
dian.facturacion.emitida:0:8261
dian.facturacion.emitida:1:9612
dian.facturacion.emitida:2:7123
dian.facturacion.emitida:3:7019
dian.facturacion.emitida:4:7671
dian.facturacion.emitida:5:6670