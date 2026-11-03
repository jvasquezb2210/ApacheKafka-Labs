Lab04

estudiante@ip-192-168-166-59:/home$ cd ~/kafka-dian/entorno
EQUIPO=equipo08Est2
TOPIC=$EQUIPO.lab04.auditoria.acceso
estudiante@ip-192-168-166-59:~/kafka-dian/entorno$ alias kt='docker exec broker1 /opt/kafka/bin/kafka-topics.sh --bootstrap-server broker1:19092'
alias kc='docker exec broker1 /opt/kafka/bin/kafka-configs.sh --bootstrap-server broker1:19092'
estudiante@ip-192-168-166-59:~/kafka-dian/entorno$ kt --create --topic "$TOPIC" --partitions 6 --replication-factor 3 \
   --config cleanup.policy=compact --config min.insync.replicas=2 --config segment.bytes=1048576
kt --describe --topic "$TOPIC" | head -1
WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Created topic equipo08Est2.lab04.auditoria.acceso.
Topic: equipo08Est2.lab04.auditoria.acceso      TopicId: uLjr9i96S_a7vuskAzZ64w PartitionCount: 6       ReplicationFactor: 3    Configs: min.insync.replicas=2,cleanup.policy=compact,segment.bytes=1048576,unclean.leader.election.enable=false

estudiante@ip-192-168-166-59:~/kafka-dian/entorno$ kc --alter --entity-type topics --entity-name "$TOPIC" --delete-config min.compaction.lag.ms
Invalid config(s): min.compaction.lag.ms

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 gen_eventos.py --topic dian.facturacion.emitida --n 20000            # client.id = gen-facturacion (con c
uota)
python3 gen_eventos.py --topic dian.facturacion.emitida --n 20000 --client-id sin-cuota
enviados: 20000
enviados: 20000
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ kc --alter --entity-type clients --entity-name gen-facturacion --delete-config producer_byte_rate
Completed updating config for client gen-facturacion.
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-log-dirs.sh --bootstrap-server broker1:19092 --describe --broker-list 1,2,3 --topic-list dian.facturacion.emitida | tail -1 | python3 -m json.tool | grep -E '"partition"|"size"' | head -12
                            "partition": "dian.facturacion.emitida-5",
                            "size": 1806414,
                            "partition": "dian.facturacion.emitida-4",
                            "size": 1847248,
                            "partition": "dian.facturacion.emitida-1",
                            "size": 2177076,
                            "partition": "dian.facturacion.emitida-0",
                            "size": 1937571,
                            "partition": "dian.facturacion.emitida-3",
                            "size": 1791761,
                            "partition": "dian.facturacion.emitida-2",
                            "size": 1712975,
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ echo "x" | docker exec -i broker1 /opt/kafka/bin/kafka-console-producer.sh --bootstrap-server broker1:19092 --topic topic.que.no.existe
kt --list | grep "no.existe" || echo "no se creó: correcto"
[2026-10-29 14:55:30,525] WARN [Producer clientId=console-producer] The metadata response from the cluster reported a recoverable issue with correlation id 1 : {topic.que.no.existe=UNKNOWN_TOPIC_OR_PARTITION} (org.apache.kafka.clients.NetworkClient)
[2026-10-29 14:55:30,641] WARN [Producer clientId=console-producer] The metadata response from the cluster reported a recoverable issue with correlation id 5 : {topic.que.no.existe=UNKNOWN_TOPIC_OR_PARTITION} (org.apache.kafka.clients.NetworkClient)
[2026-10-29 14:55:30,835] WARN [Producer clientId=console-producer] The metadata response from the cluster reported a recoverable issue with correlation id 6 : {topic.que.no.existe=UNKNOWN_TOPIC_OR_PARTITION} (org.apache.kafka.clients.NetworkClient)
[2026-10-29 14:55:31,300] WARN [Producer clientId=console-producer] The metadata response from the cluster reported a recoverable issue with correlation id 7 : {topic.que.no.existe=UNKNOWN_TOPIC_OR_PARTITION} (org.apache.kafka.clients.NetworkClient)
[2026-10-29 14:55:32,155] WARN [Producer clientId=console-producer] The metadata response from the cluster reported a recoverable issue with correlation id 8 : {topic.que.no.existe=UNKNOWN_TOPIC_OR_PARTITION} (org.apache.kafka.clients.NetworkClient)
...
[2026-10-29 14:55:57,755] WARN [Producer clientId=console-producer] The metadata response from the cluster reported a recoverable issue with correlation id 34 : {topic.que.no.existe=UNKNOWN_TOPIC_OR_PARTITION} (org.apache.kafka.clients.NetworkClient)
^Cno se creó: correcto