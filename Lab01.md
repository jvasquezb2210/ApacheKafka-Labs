estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-broker-api-versions.sh --bootstrap-server broker1:19092 | grep -E "^broker[0-9]"
broker1:19092 (id: 1 rack: null) -> (
broker2:19092 (id: 2 rack: null) -> (
broker3:19092 (id: 3 rack: null) -> (

Topic: equipo08.lab01.facturacion.emitida       TopicId: PnfBN2KFToauO8NWdiN4VQ PartitionCount: 3       ReplicationFactor: 3   Configs: min.insync.replicas=2,unclean.leader.election.enable=false
        Topic: equipo08.lab01.facturacion.emitida       Partition: 0    Leader: 1       Replicas: 1,2,3 Isr: 1,3,2      Elr:   LastKnownElr: 
        Topic: equipo08.lab01.facturacion.emitida       Partition: 1    Leader: 2       Replicas: 2,3,1 Isr: 1,3,2      Elr:   LastKnownElr: 
        Topic: equipo08.lab01.facturacion.emitida       Partition: 2    Leader: 3       Replicas: 3,1,2 Isr: 1,3,2      Elr:   LastKnownElr: 

-- 2026-10-15
estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-broker-api-versions.sh --bootstrap-server broker1:19092 | grep -E "^broker[0-9]"
broker3:19092 (id: 3 rack: null) -> (
broker2:19092 (id: 2 rack: null) -> (
broker1:19092 (id: 1 rack: null) -> (


estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ kt --describe --topic "$TOPIC"
Topic: equipo08est2.lab01.facturacion.emitida   TopicId: QUdmdEfxQte5CSPanpO1CQ PartitionCount: 3       ReplicationFactor: 3    Configs: min.insync.replicas=2,unclean.leader.election.enable=false
        Topic: equipo08est2.lab01.facturacion.emitida   Partition: 0    Leader: 1       Replicas: 1,2,3 Isr: 1,2,3      Elr:    LastKnownElr: 
        Topic: equipo08est2.lab01.facturacion.emitida   Partition: 1    Leader: 2       Replicas: 2,3,1 Isr: 2,3,1      Elr:    LastKnownElr: 
        Topic: equipo08est2.lab01.facturacion.emitida   Partition: 2    Leader: 3       Replicas: 3,1,2 Isr: 3,1,2      Elr:    LastKnownElr: 

estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-dump-log.sh --print-data-log \
  --files /var/lib/kafka/data/${TOPIC}-0/00000000000000000000.log | head -20
Dumping /var/lib/kafka/data/equipo08est2.lab01.facturacion.emitida-0/00000000000000000000.log
Log starting offset: 0
baseOffset: 0 lastOffset: 35 count: 36 baseSequence: 0 lastSequence: 35 producerId: 3000 producerEpoch: 0 partitionLeaderEpoch: 0 isTransactional: false isControl: false deleteHorizonMs: OptionalLong.empty position: 0 CreateTime: 1792072426917 size: 1812 magic: 2 compresscodec: none crc: 3492766023 isvalid: true
| offset: 0 CreateTime: 1792072426890 keySize: 4 valueSize: 35 sequence: 0 headerKeys: [] key: 9001 payload: {"nit":"9001","valor":264980,"n":1}
| offset: 1 CreateTime: 1792072426907 keySize: 4 valueSize: 34 sequence: 1 headerKeys: [] key: 9005 payload: {"nit":"9005","valor":60990,"n":5}
| offset: 2 CreateTime: 1792072426907 keySize: 5 valueSize: 37 sequence: 2 headerKeys: [] key: 90010 payload: {"nit":"90010","valor":193760,"n":10}
| offset: 3 CreateTime: 1792072426908 keySize: 5 valueSize: 37 sequence: 3 headerKeys: [] key: 90016 payload: {"nit":"90016","valor":244970,"n":16}
| offset: 4 CreateTime: 1792072426909 keySize: 5 valueSize: 36 sequence: 4 headerKeys: [] key: 90021 payload: {"nit":"90021","valor":83120,"n":21}
| offset: 5 CreateTime: 1792072426909 keySize: 5 valueSize: 37 sequence: 5 headerKeys: [] key: 90023 payload: {"nit":"90023","valor":279780,"n":23}
| offset: 6 CreateTime: 1792072426909 keySize: 5 valueSize: 37 sequence: 6 headerKeys: [] key: 90024 payload: {"nit":"90024","valor":213790,"n":24}
| offset: 7 CreateTime: 1792072426909 keySize: 5 valueSize: 37 sequence: 7 headerKeys: [] key: 90027 payload: {"nit":"90027","valor":302800,"n":27}
| offset: 8 CreateTime: 1792072426910 keySize: 5 valueSize: 36 sequence: 8 headerKeys: [] key: 90030 payload: {"nit":"90030","valor":46290,"n":30}
| offset: 9 CreateTime: 1792072426910 keySize: 5 valueSize: 37 sequence: 9 headerKeys: [] key: 90032 payload: {"nit":"90032","valor":149170,"n":32}
| offset: 10 CreateTime: 1792072426910 keySize: 5 valueSize: 37 sequence: 10 headerKeys: [] key: 90034 payload: {"nit":"90034","valor":183490,"n":34}
| offset: 11 CreateTime: 1792072426910 keySize: 5 valueSize: 37 sequence: 11 headerKeys: [] key: 90038 payload: {"nit":"90038","valor":234720,"n":38}
| offset: 12 CreateTime: 1792072426911 keySize: 5 valueSize: 37 sequence: 12 headerKeys: [] key: 90041 payload: {"nit":"90041","valor":194470,"n":41}
| offset: 13 CreateTime: 1792072426911 keySize: 5 valueSize: 37 sequence: 13 headerKeys: [] key: 90042 payload: {"nit":"90042","valor":101950,"n":42}
| offset: 14 CreateTime: 1792072426911 keySize: 5 valueSize: 36 sequence: 14 headerKeys: [] key: 90044 payload: {"nit":"90044","valor":58970,"n":44}
| offset: 15 CreateTime: 1792072426911 keySize: 5 valueSize: 37 sequence: 15 headerKeys: [] key: 90045 payload: {"nit":"90045","valor":281190,"n":45}
| offset: 16 CreateTime: 1792072426912 keySize: 5 valueSize: 37 sequence: 16 headerKeys: [] key: 90047 payload: {"nit":"90047","valor":258190,"n":47}

estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker stop broker1
broker1
estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ KB=broker2
estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ kt --describe --topic "$TOPIC"
Topic: equipo08est2.lab01.facturacion.emitida   TopicId: QUdmdEfxQte5CSPanpO1CQ PartitionCount: 3       ReplicationFactor: 3    Configs: min.insync.replicas=2,unclean.leader.election.enable=false
        Topic: equipo08est2.lab01.facturacion.emitida   Partition: 0    Leader: 2       Replicas: 1,2,3 Isr: 2,3        Elr:    LastKnownElr: 
        Topic: equipo08est2.lab01.facturacion.emitida   Partition: 1    Leader: 2       Replicas: 2,3,1 Isr: 2,3        Elr:    LastKnownElr: 
        Topic: equipo08est2.lab01.facturacion.emitida   Partition: 2    Leader: 3       Replicas: 3,1,2 Isr: 3,2        Elr:    LastKnownElr: 

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker start broker1
sleep 20
kt --describe --topic "$TOPIC"
broker1
Topic: equipo08est2.lab01.facturacion.emitida   TopicId: QUdmdEfxQte5CSPanpO1CQ PartitionCount: 3       ReplicationFactor: 3    Configs: min.insync.replicas=2,unclean.leader.election.enable=false
        Topic: equipo08est2.lab01.facturacion.emitida   Partition: 0    Leader: 1       Replicas: 1,2,3 Isr: 3,1,2      Elr:    LastKnownElr: 
        Topic: equipo08est2.lab01.facturacion.emitida   Partition: 1    Leader: 2       Replicas: 2,3,1 Isr: 3,1,2      Elr:    LastKnownElr: 
        Topic: equipo08est2.lab01.facturacion.emitida   Partition: 2    Leader: 3       Replicas: 3,1,2 Isr: 3,1,2      Elr:    LastKnownElr: 

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server broker1:19092   --topic "$TOPIC" --from-beginning --timeout-ms 10000 2>/dev/null | wc -l
110

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker compose -f docker-compose-lab-kafka.yml stop
[+] stop 5/5
 ✔ Container bootstrap-topics Stopped                                                                                                                                                                                                0.0s
 ✔ Container kafka-ui         Stopped                                                                                                                                                                                                2.4s
 ✔ Container broker2          Stopped                                                                                                                                                                                                7.5s
 ✔ Container broker3          Stopped                                                                                                                                                                                                0.0s
 ✔ Container broker1          Stopped