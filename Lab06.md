Lab06

(.venv) estudiante@ip-192-168-166-59:~/kafka-dian/entorno$ EQUIPO=equipo08est2
(.venv) estudiante@ip-192-168-166-59:~/kafka-dian/entorno$ TOPIC=$EQUIPO.lab05.facturacion.emitida
(.venv) estudiante@ip-192-168-166-59:~/kafka-dian/entorno$ GROUP=${EQUIPO}-lab06-sink
(.venv) estudiante@ip-192-168-166-59:~/kafka-dian/entorno$ docker compose -f docker-compose.override.yml up -d
WARN[0000] Found orphan containers (kafka-ui, bootstrap-topics, broker2, broker1, broker3) for this project. If you removed or renamed this service in your compose file, you can run this command with the --remove-orphans flag to clean it up. 
[+] up 1/1
 ✔ Container postgres Started  

 (.venv) estudiante@ip-192-168-166-59:~/kafka-dian/entorno$ docker ps
CONTAINER ID   IMAGE                            COMMAND                  CREATED          STATUS                    PORTS NAMES
938e5c20315d   postgres:16                      "docker-entrypoint.s…"   3 minutes ago    Up 26 seconds postgres
1bb1263746a2   ghcr.io/kafbat/kafka-ui:v1.1.0   "/bin/sh -c 'java --…"   14 minutes ago   Up 14 minutes             0.0.0.0:8081->8080/tcp, [::]:8081->8080/tcp kafka-ui
1b0ee4bd912a   apache/kafka:3.9.0               "/__cacert_entrypoin…"   2 weeks ago      Up 14 minutes (healthy)   0.0.0.0:9094->9094/tcp, [::]:9094->9094/tcp, 9092/tcp, 0.0.0.0:9102->9102/tcp, [::]:9102->9102/tcp broker2
56c9dae195d0   apache/kafka:3.9.0               "/__cacert_entrypoin…"   2 weeks ago      Up 14 minutes (healthy)   0.0.0.0:9092->9092/tcp, [::]:9092->9092/tcp, 0.0.0.0:9101->9101/tcp, [::]:9101->9101/tcp broker1
8f002e6774a6   apache/kafka:3.9.0               "/__cacert_entrypoin…"   2 weeks ago      Up 14 minutes (healthy)   0.0.0.0:9096->9096/tcp, [::]:9096->9096/tcp, 9092/tcp, 0.0.0.0:9103->9103/tcp, [::]:9103->9103/tcp broker3

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ EQUIPO=equipo08
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ TOPIC=$EQUIPO.lab05.facturacion.emitida
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ GROUP=${EQUIPO}-lab06-sink
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 gen_eventos.py --topic "$TOPIC" --n 500
enviados: 500

^C(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec -it postgres psql -U dian -c "SELECT count(*) FROM facturas;"
 count  
--------
 656460
(1 row)

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server broker1:19092 --group "$GROUP" --topic "$TOPIC" --reset-offsets --to-earliest --execute

GROUP                          TOPIC                          PARTITION  NEW-OFFSET     equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 0          0              equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 2          0              equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 1          0              equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 4          0              equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 3          0              equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 5          0              (.venv) 

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 consumidor_facturacion.py --topic "$TOPIC" --group "$GROUP" --kill-after 60
Simulando caída tras 60 mensajes, SIN commit
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 consumidor_facturacion.py --topic "$TOPIC" --group "$GROUP" 

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 consumidor_facturacion.py --topic "$TOPIC" --group "$GROUP" 
[intento 1] p=5 o=59 error=UniqueViolation: duplicate key value violates unique constraint "facturas_pkey"
DETAIL:  Key (id_evento)=(65083788-e39d-4418-9b45-f16196059258) already exists.

[intento 2] p=5 o=59 error=UniqueViolation: duplicate key value violates unique constraint "facturas_pkey"
DETAIL:  Key (id_evento)=(65083788-e39d-4418-9b45-f16196059258) already exists.

[intento 3] p=5 o=59 error=UniqueViolation: duplicate key value violates unique constraint "facturas_pkey"
DETAIL:  Key (id_evento)=(65083788-e39d-4418-9b45-f16196059258) already exists.

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec -it postgres psql -U dian -c "SELECT count(*) total, count(DISTINCT id_evento) unicos FROM facturas;"
 total | unicos 
-------+--------
 41325 |  41325
(1 row)

PASO 3

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec -it postgres psql -U dian -c "TRUNCATE facturas;"
TRUNCATE TABLE
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-consumer-groups.sh --bootstrap-server broker1:19092 --group "$GROUP" --topic "$TOPIC" --reset-offsets --to-earliest --execute

GROUP                          TOPIC                          PARTITION  NEW-OFFSET     equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 0          0              equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 2          0              equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 1          0              equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 4          0              equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 3          0              equipo08-lab06-sink            equipo08.lab05.facturacion.emitida 5          0              (.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 python3 codigo/consumidor_facturacion.py --topic "$TOPIC" --group "$GROUP" --upsert --kill-a^Cer 60
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 consumidor_facturacion.py --topic "$TOPIC" --group "$GROUP" --upsert --kill-after 60
Simulando caída tras 60 mensajes, SIN commit
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 consumidor_facturacion.py --topic "$TOPIC" --group "$GROUP" --upsert
^C(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entornodocker exec -it postgres psql -U dian -c "SELECT count(*) total, count(DISTINCT id_evento) unicos FROM facturas;";"
 total | unicos 
-------+--------
 12241 |  12241
(1 row)

PASO 4


PASO  5
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-topics.sh \
  --bootstrap-server broker1:19092 \
  --create \
  --topic "${TOPIC}.dlq" \
  --partitions 3 \
  --replication-factor 3
WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Created topic equipo08.lab05.facturacion.emitida.dlq.
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ echo '800999|{esto no es json' | docker exec -i broker1 /opt/kafka/bin/kafka-console-producer.sh --bootstrap-server broker1:19092 \
  --topic "$TOPIC" --property parse.key=true --property key.separator="|"
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 consumidor_facturacion.py --topic "$TOPIC" --group "$GROUP" --upsert
