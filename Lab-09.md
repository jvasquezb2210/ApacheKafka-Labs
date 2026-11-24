# Lab-09

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker ps

__Output__
CONTAINER ID   IMAGE                                   COMMAND                  CREATED         STATUS                            PORTS                                                            NAMES
e8785e61c348   postgres:16                             "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes                      0.0.0.0:5433->5432/tcp, [::]:5433->5432/tcp                                                          postgres
0c30ccf0942d   dian-kafka-connect:1.0.0                "/etc/confluent/dock…"   6 minutes ago   Up 6 minutes (health: starting)   0.0.0.0:8083->8083/tcp, [::]:8083->8083/tcp, 9092/tcp                                                connect
d1ee390e6174   confluentinc/cp-schema-registry:7.7.0   "/etc/confluent/dock…"   47 hours ago    Up 6 minutes                      0.0.0.0:8081->8081/tcp, [::]:8081->8081/tcp                                                          schema-registry
f25215d2ad67   ghcr.io/kafbat/kafka-ui:v1.1.0          "/bin/sh -c 'java --…"   47 hours ago    Up 10 minutes                     0.0.0.0:8085->8080/tcp, [::]:8085->8080/tcp                                                          kafka-ui
1b0ee4bd912a   apache/kafka:3.9.0                      "/__cacert_entrypoin…"   5 weeks ago     Up 10 minutes (healthy)           0.0.0.0:9094->9094/tcp, [::]:9094->9094/tcp, 9092/tcp, 0.0.0.0:9102->9102/tcp, [::]:9102->9102/tcp   broker2
56c9dae195d0   apache/kafka:3.9.0                      "/__cacert_entrypoin…"   5 weeks ago     Up 10 minutes (healthy)           0.0.0.0:9092->9092/tcp, [::]:9092->9092/tcp, 0.0.0.0:9101->9101/tcp, [::]:9101->9101/tcp             broker1
8f002e6774a6   apache/kafka:3.9.0                      "/__cacert_entrypoin…"   5 weeks ago     Up 10 minutes (healthy)           0.0.0.0:9096->9096/tcp, [::]:9096->9096/tcp, 9092/tcp, 0.0.0.0:9103->9103/tcp, [::]:9103->9103/tcp   broker3

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s localhost:8083/connector-plugins | python3 -c "import json,sys;[print(p['class']) for p in json.load(sys.stdin)]" | grep -Ei "jdbc|s3|postgres"
__Output__
io.confluent.connect.jdbc.JdbcSinkConnector
io.confluent.connect.s3.S3SinkConnector
io.confluent.connect.jdbc.JdbcSourceConnector
io.debezium.connector.postgresql.PostgresConnector

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s -X POST -H "Content-Type: application/json" --data @conector-jdbc-source.json localhos
t:8083/connectors
sleep 20
curl -s localhost:8083/connectors/declaraciones-jdbc-source/status | python3 -m json.tool
__Output__
{"name":"declaraciones-jdbc-source","config":{"connector.class":"io.confluent.connect.jdbc.JdbcSourceConnector","tasks.max":"1","connection.url":"jdbc:postgresql://postgres:5433/dian","connection.user":"dian","connection.password":"dian","table.whitelist":"declaraciones","mode":"timestamp","timestamp.column.name":"actualizado_en","poll.interval.ms":"5000","topic.prefix":"jdbc.","key.converter":"org.apache.kafka.connect.storage.StringConverter","value.converter":"io.confluent.connect.avro.AvroConverter","value.converter.schema.registry.url":"http://schema-registry:8081","transforms":"clave,extraer,mascara","transforms.clave.type":"org.apache.kafka.connect.transforms.ValueToKey","transforms.clave.fields":"nit","transforms.extraer.type":"org.apache.kafka.connect.transforms.ExtractField$Key","transforms.extraer.field":"nit","transforms.mascara.type":"org.apache.kafka.connect.transforms.MaskField$Value","transforms.mascara.fields":"nit","transforms.mascara.replacement":"SEUDONIMIZADO","name":"declaraciones-jdbc-source"},"tasks":[],"type":"source"}{
    "error_code": 404,
    "message": "No status found for connector declaraciones-jdbc-source"
}

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s localhost:8083/connectors/declaraciones-jdbc-source/status | python3 -m json.tool
docker exec broker1 /opt/kafka/bin/kafka-topics.sh --bootstrap-server broker1:19092 --list | grep jdbc
__Output__
{
    "name": "declaraciones-jdbc-source",
    "connector": {
        "state": "RUNNING",
        "worker_id": "connect:8083"
    },
    "tasks": [
        {
            "id": 0,
            "state": "RUNNING",
            "worker_id": "connect:8083"
        }
    ],
    "type": "source"
}

## Paso 2 

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s localhost:8081/subjects | python3 -m json.tool
__Output__
[
    "dian.declaracion.presentada-value",
    "jdbc.declaraciones-value"
]

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s localhost:8081/subjects/jdbc.declaraciones-value/versions/1 | python3 -m json.tool | head -25
__Output__
{
    "subject": "jdbc.declaraciones-value",
    "version": 1,
    "id": 2,
    "schema": "{\"type\":\"record\",\"name\":\"declaraciones\",\"fields\":[{\"name\":\"id_declaracion\",\"type\":\"int\"},{\"name\":\"nit\",\"type\":\"string\"},{\"name\":\"formulario\",\"type\":\"string\"},{\"name\":\"periodo\",\"type\":\"string\"},{\"name\":\"valor_pagado\",\"type\":{\"type\":\"bytes\",\"scale\":2,\"precision\":16,\"connect.version\":1,\"connect.parameters\":{\"scale\":\"2\",\"connect.decimal.precision\":\"16\"},\"connect.name\":\"org.apache.kafka.connect.data.Decimal\",\"logicalType\":\"decimal\"}},{\"name\":\"estado\",\"type\":\"string\"},{\"name\":\"actualizado_en\",\"type\":{\"type\":\"long\",\"connect.version\":1,\"connect.name\":\"org.apache.kafka.connect.data.Timestamp\",\"logicalType\":\"timestamp-millis\"}}],\"connect.name\":\"declaraciones\"}"
}
__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server broker1:19092   --topic jdbc.declaraciones --from-beginning --max-messages 3 --property print.key=true 2>/dev/null | head

## Paso 4

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s http://localhost:8083/connectors/declaraciones-s3-sink/status | python3 -m json.tool
__Output__
{
    "name": "declaraciones-s3-sink",
    "connector": {
        "state": "RUNNING",
        "worker_id": "connect:8083"
    },
    "tasks": [
        {
            "id": 0,
            "state": "RUNNING",
            "worker_id": "connect:8083"
        },
        {
            "id": 1,
            "state": "RUNNING",
            "worker_id": "connect:8083"
        }
    ],
    "type": "sink"
}

## Paso 5

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec -it postgres psql -U dian -c "UPDATE declaraciones SET valor_pagado = valor_pagado + 1, actualizado_en = now() WHERE id_declaracion = 1;"
__Output__
UPDATE 1

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec -it postgres psql -U dian -c "DELETE FROM declaraciones WHERE id_declaracion = 2;"
__Output__
DELETE 0

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-get-offsets.sh --bootstrap-server broker1:19092 --topic jdbc.declaraciones
__Output__
jdbc.declaraciones:0:599

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec -it postgres psql -U dian -c "UPDATE declaraciones SET estado='CORREGIDA', actualizado_en=now() WHERE id_declaracion = 3;"
__Output__
UPDATE 1

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec -it postgres psql -U dian -c "DELETE FROM declaraciones WHERE id_declaracion = 4;"
__Output__
DELETE 1

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server broker1:19092 \
  --topic cdc.public.declaraciones --from-beginning --timeout-ms 15000 --property print.key=true 2>/dev/null | tail -5

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec -it postgres psql -U dian -c "SELECT slot_name, active FROM pg_replication_slots;"

__Output__
 slot_name  | active 
------------+--------
 dian_lab09 | t
(1 row)

## Paso 6

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s localhost:8083/connectors | python3 -m json.tool
__Output__
[
    "declaraciones-s3-sink",
    "declaraciones-cdc-source",
    "declaraciones-jdbc-source"
]
