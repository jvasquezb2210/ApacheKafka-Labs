estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ kq describe --status
ClusterId:              dian-kafka-lab-2026
LeaderId:               2
LeaderEpoch:            24
HighWatermark:          33554
MaxFollowerLag:         0
MaxFollowerLagTimeMs:   154
CurrentVoters:          [{"id": 1, "directoryId": null, "endpoints": ["CONTROLLER://broker1:9093"]}, {"id": 2, "directoryId": null, "endpoints": ["CONTROLLER://broker2:9093"]}, {"id": 3, "directoryId": null, "endpoints": ["CONTROLLER://broker3:9093"]}]
CurrentObservers:       []

estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ kq describe --replication
NodeId  DirectoryId             LogEndOffset    Lag     LastFetchTimestamp      LastCaughtUpTimestamp   Status  
2       AAAAAAAAAAAAAAAAAAAAAA  33951           0       1792077853642           1792077853642           Leader  
1       AAAAAAAAAAAAAAAAAAAAAA  33951           0       1792077853212           1792077853212           Follower
3       AAAAAAAAAAAAAAAAAAAAAA  33951           0       1792077853213           1792077853213           Follower

estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 ls -lh /var/lib/kafka/data/__cluster_metadata-0/
total 2.4M
-rw-r--r-- 1 appuser appuser 2.3K Oct 15 15:20 00000000000000000000.index
-rw-r--r-- 1 appuser appuser 1.2M Oct 13 16:19 00000000000000000000.log
-rw-r--r-- 1 appuser appuser 3.5K Oct 15 15:20 00000000000000000000.timeindex
-rw-r--r-- 1 appuser appuser  10M Oct 15 15:25 00000000000000016947.index
-rw-r--r-- 1 appuser appuser 1.2M Oct 15 15:25 00000000000000016947.log
-rw-r--r-- 1 appuser appuser   10 Oct 15 15:20 00000000000000016947.snapshot
-rw-r--r-- 1 appuser appuser  10M Oct 15 15:25 00000000000000016947.timeindex
-rw-r--r-- 1 appuser appuser   10 Oct 15 15:20 00000000000000030423.snapshot
-rw-r--r-- 1 appuser appuser   94 Oct 15 15:20 leader-epoch-checkpoint
-rw-r--r-- 1 appuser appuser   43 Oct  6 16:19 partition.metadata
-rw-r--r-- 1 appuser appuser  154 Oct 15 15:20 quorum-state
estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-dump-log.sh --cluster-metadata-decoder \
  --files /var/lib/kafka/data/__cluster_metadata-0/00000000000000000000.log | grep -E "TOPIC_RECORD|PARTITION_RECORD|REGISTER_BROKER" | head -20
| offset: 5 CreateTime: 1791303544215 keySize: -1 valueSize: 144 sequence: -1 headerKeys: [] payload: {"type":"REGISTER_BROKER_RECORD","version":3,"data":{"brokerId":2,"isMigratingZkBroker":false,"incarnationId":"XtYP3TF_RQi9d3tNCWBDEg","brokerEpoch":5,"endPoints":[{"name":"INTERNAL","host":"broker2","port":19092,"securityProtocol":0},{"name":"EXTERNAL","host":"localhost","port":9094,"securityProtocol":0}],"features":[{"name":"kraft.version","minSupportedVersion":0,"maxSupportedVersion":1},{"name":"metadata.version","minSupportedVersion":1,"maxSupportedVersion":21}],"rack":null,"fenced":true,"inControlledShutdown":false,"logDirs":["EOWH96lZJcvClvtINjxdRA"]}}
| offset: 6 CreateTime: 1791303544308 keySize: -1 valueSize: 144 sequence: -1 headerKeys: [] payload: {"type":"REGISTER_BROKER_RECORD","version":3,"data":{"brokerId":1,"isMigratingZkBroker":false,"incarnationId":"ySZEVZo5QSS3hfifAnHb-A","brokerEpoch":6,"endPoints":[{"name":"INTERNAL","host":"broker1","port":19092,"securityProtocol":0},{"name":"EXTERNAL","host":"localhost","port":9092,"securityProtocol":0}],"features":[{"name":"kraft.version","minSupportedVersion":0,"maxSupportedVersion":1},{"name":"metadata.version","minSupportedVersion":1,"maxSupportedVersion":21}],"rack":null,"fenced":true,"inControlledShutdown":false,"logDirs":["oSLJ7Yhf08Yk9pGBbw2qmQ"]}}
| offset: 7 CreateTime: 1791303544339 keySize: -1 valueSize: 144 sequence: -1 headerKeys: [] payload: {"type":"REGISTER_BROKER_RECORD","version":3,"data":{"brokerId":3,"isMigratingZkBroker":false,"incarnationId":"hkZ0TgK1Q3S-af2j-eIUAA","brokerEpoch":7,"endPoints":[{"name":"INTERNAL","host":"broker3","port":19092,"securityProtocol":0},{"name":"EXTERNAL","host":"localhost","port":9096,"securityProtocol":0}],"features":[{"name":"kraft.version","minSupportedVersion":0,"maxSupportedVersion":1},{"name":"metadata.version","minSupportedVersion":1,"maxSupportedVersion":21}],"rack":null,"fenced":true,"inControlledShutdown":false,"logDirs":["50wn-UtCy9KN9q0aqDXDbg"]}}
| offset: 42 CreateTime: 1791303558607 keySize: -1 valueSize: 45 sequence: -1 headerKeys: [] payload: {"type":"TOPIC_RECORD","version":0,"data":{"name":"dian.facturacion.emitida","topicId":"yLQJTlhLT_iwsYwUhfLKHw"}}
| offset: 45 CreateTime: 1791303558607 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":0,"topicId":"yLQJTlhLT_iwsYwUhfLKHw","replicas":[2,3,1],"isr":[2,3,1],"removingReplicas":[],"addingReplicas":[],"leader":2,"leaderEpoch":0,"partitionEpoch":0,"directories":["EOWH96lZJcvClvtINjxdRA","50wn-UtCy9KN9q0aqDXDbg","oSLJ7Yhf08Yk9pGBbw2qmQ"]}}
| offset: 46 CreateTime: 1791303558607 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":1,"topicId":"yLQJTlhLT_iwsYwUhfLKHw","replicas":[3,1,2],"isr":[3,1,2],"removingReplicas":[],"addingReplicas":[],"leader":3,"leaderEpoch":0,"partitionEpoch":0,"directories":["50wn-UtCy9KN9q0aqDXDbg","oSLJ7Yhf08Yk9pGBbw2qmQ","EOWH96lZJcvClvtINjxdRA"]}}
| offset: 47 CreateTime: 1791303558607 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":2,"topicId":"yLQJTlhLT_iwsYwUhfLKHw","replicas":[1,2,3],"isr":[1,2,3],"removingReplicas":[],"addingReplicas":[],"leader":1,"leaderEpoch":0,"partitionEpoch":0,"directories":["oSLJ7Yhf08Yk9pGBbw2qmQ","EOWH96lZJcvClvtINjxdRA","50wn-UtCy9KN9q0aqDXDbg"]}}
| offset: 48 CreateTime: 1791303558607 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":3,"topicId":"yLQJTlhLT_iwsYwUhfLKHw","replicas":[1,2,3],"isr":[1,2,3],"removingReplicas":[],"addingReplicas":[],"leader":1,"leaderEpoch":0,"partitionEpoch":0,"directories":["oSLJ7Yhf08Yk9pGBbw2qmQ","EOWH96lZJcvClvtINjxdRA","50wn-UtCy9KN9q0aqDXDbg"]}}
| offset: 49 CreateTime: 1791303558607 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":4,"topicId":"yLQJTlhLT_iwsYwUhfLKHw","replicas":[2,3,1],"isr":[2,3,1],"removingReplicas":[],"addingReplicas":[],"leader":2,"leaderEpoch":0,"partitionEpoch":0,"directories":["EOWH96lZJcvClvtINjxdRA","50wn-UtCy9KN9q0aqDXDbg","oSLJ7Yhf08Yk9pGBbw2qmQ"]}}
| offset: 50 CreateTime: 1791303558607 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":5,"topicId":"yLQJTlhLT_iwsYwUhfLKHw","replicas":[3,1,2],"isr":[3,1,2],"removingReplicas":[],"addingReplicas":[],"leader":3,"leaderEpoch":0,"partitionEpoch":0,"directories":["50wn-UtCy9KN9q0aqDXDbg","oSLJ7Yhf08Yk9pGBbw2qmQ","EOWH96lZJcvClvtINjxdRA"]}}
| offset: 57 CreateTime: 1791303561282 keySize: -1 valueSize: 48 sequence: -1 headerKeys: [] payload: {"type":"TOPIC_RECORD","version":0,"data":{"name":"dian.declaracion.presentada","topicId":"Rjm7kEz7QbWrSrwLKMNDRw"}}
| offset: 60 CreateTime: 1791303561282 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":0,"topicId":"Rjm7kEz7QbWrSrwLKMNDRw","replicas":[3,1,2],"isr":[3,1,2],"removingReplicas":[],"addingReplicas":[],"leader":3,"leaderEpoch":0,"partitionEpoch":0,"directories":["50wn-UtCy9KN9q0aqDXDbg","oSLJ7Yhf08Yk9pGBbw2qmQ","EOWH96lZJcvClvtINjxdRA"]}}
| offset: 61 CreateTime: 1791303561282 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":1,"topicId":"Rjm7kEz7QbWrSrwLKMNDRw","replicas":[1,2,3],"isr":[1,2,3],"removingReplicas":[],"addingReplicas":[],"leader":1,"leaderEpoch":0,"partitionEpoch":0,"directories":["oSLJ7Yhf08Yk9pGBbw2qmQ","EOWH96lZJcvClvtINjxdRA","50wn-UtCy9KN9q0aqDXDbg"]}}
| offset: 62 CreateTime: 1791303561282 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":2,"topicId":"Rjm7kEz7QbWrSrwLKMNDRw","replicas":[2,3,1],"isr":[2,3,1],"removingReplicas":[],"addingReplicas":[],"leader":2,"leaderEpoch":0,"partitionEpoch":0,"directories":["EOWH96lZJcvClvtINjxdRA","50wn-UtCy9KN9q0aqDXDbg","oSLJ7Yhf08Yk9pGBbw2qmQ"]}}
| offset: 63 CreateTime: 1791303561282 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":3,"topicId":"Rjm7kEz7QbWrSrwLKMNDRw","replicas":[3,2,1],"isr":[3,2,1],"removingReplicas":[],"addingReplicas":[],"leader":3,"leaderEpoch":0,"partitionEpoch":0,"directories":["50wn-UtCy9KN9q0aqDXDbg","EOWH96lZJcvClvtINjxdRA","oSLJ7Yhf08Yk9pGBbw2qmQ"]}}
| offset: 64 CreateTime: 1791303561282 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":4,"topicId":"Rjm7kEz7QbWrSrwLKMNDRw","replicas":[2,1,3],"isr":[2,1,3],"removingReplicas":[],"addingReplicas":[],"leader":2,"leaderEpoch":0,"partitionEpoch":0,"directories":["EOWH96lZJcvClvtINjxdRA","oSLJ7Yhf08Yk9pGBbw2qmQ","50wn-UtCy9KN9q0aqDXDbg"]}}
| offset: 65 CreateTime: 1791303561282 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":5,"topicId":"Rjm7kEz7QbWrSrwLKMNDRw","replicas":[1,3,2],"isr":[1,3,2],"removingReplicas":[],"addingReplicas":[],"leader":1,"leaderEpoch":0,"partitionEpoch":0,"directories":["oSLJ7Yhf08Yk9pGBbw2qmQ","50wn-UtCy9KN9q0aqDXDbg","EOWH96lZJcvClvtINjxdRA"]}}
| offset: 71 CreateTime: 1791303563979 keySize: -1 valueSize: 57 sequence: -1 headerKeys: [] payload: {"type":"TOPIC_RECORD","version":0,"data":{"name":"dian.aduanas.declaracion-importacion","topicId":"KwujfBPSQeKxNtJYQkxM5Q"}}
| offset: 73 CreateTime: 1791303563979 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":0,"topicId":"KwujfBPSQeKxNtJYQkxM5Q","replicas":[2,3,1],"isr":[2,3,1],"removingReplicas":[],"addingReplicas":[],"leader":2,"leaderEpoch":0,"partitionEpoch":0,"directories":["EOWH96lZJcvClvtINjxdRA","50wn-UtCy9KN9q0aqDXDbg","oSLJ7Yhf08Yk9pGBbw2qmQ"]}}
| offset: 74 CreateTime: 1791303563979 keySize: -1 valueSize: 113 sequence: -1 headerKeys: [] payload: {"type":"PARTITION_RECORD","version":1,"data":{"partitionId":1,"topicId":"KwujfBPSQeKxNtJYQkxM5Q","replicas":[3,1,2],"isr":[3,1,2],"removingReplicas":[],"addingReplicas":[],"leader":3,"leaderEpoch":0,"partitionEpoch":0,"directories":["50wn-UtCy9KN9q0aqDXDbg","oSLJ7Yhf08Yk9pGBbw2qmQ","EOWH96lZJcvClvtINjxdRA"]}}

estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ EQUIPO=equipo08est2                                                                                                      
TOPIC=$EQUIPO.lab02.declaracion.presentada
kt --create --topic "$TOPIC" --partitions 3 --replication-factor 3
docker exec broker1 /opt/kafka/bin/kafka-dump-log.sh --cluster-metadata-decoder \
  --files /var/lib/kafka/data/__cluster_metadata-0/00000000000000000000.log | grep -A3 "$TOPIC" | head -20
WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Created topic equipo08est2.lab02.declaracion.presentada.


