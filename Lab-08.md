# Lab-08

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s localhost:8081/subjects
__Output__
[]

## Paso 2

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ JSON=$(python3 -c "import json;print(json.dumps({'schema':open('declaracion-presentada-v1.avsc').read()}))")
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s -X POST -H "Content-Type: application/vnd.schemaregistry.v1+json" \
  --data "$JSON" http://localhost:8081/subjects/dian.declaracion.presentada-value/versions
__Output__
{"id":1}

__Input__
{"id":1}(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/ecurl -s http://localhost:8081/subjects/dian.declaracion.presentada-value/versionsversions
__Output__
[1]

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s http://localhost:8081/config/dian.declaracion.presentada-value
__Output__
{"error_code":40408,"message":"Subject 'dian.declaracion.presentada-value' does not have subject-level compatibility configured"}

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s "http://localhost:8081/config/dian.declaracion.presentada-value?defaultToGlobal=true"
__Output__
{"compatibilityLevel":"BACKWARD"}

## Paso 3

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ EQUIPO=equipo08
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 productor_avro.py --n 50
__Output__
%4|1794927869.727|GETPID|rdkafka#producer-1| [thrd:main]: Failed to acquire idempotence PID from broker localhost:9092/1: Broker: Coordinator load in progress: retrying
enviados: 50

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 consumidor_avro.py --group "${EQUIPO}-lab08"
__Output__
p=2 o=0 nit=823078574 form=490 valor=28225272.00 estado=PRESENTADA
p=2 o=1 nit=811434606 form=350 valor=26154468.00 estado=PRESENTADA
p=2 o=2 nit=880847182 form=110 valor=29083881.00 estado=PRESENTADA
p=2 o=3 nit=854533527 form=350 valor=25210356.00 estado=PRESENTADA
p=2 o=4 nit=831542444 form=490 valor=21396525.00 estado=CORREGIDA
sin más mensajes (50 leídos)

## Paso 4

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server broker1:19092 \
  --topic dian.declaracion.presentada --from-beginning --max-messages 1 | xxd | head -4
__Output__
Processed a total of 1 messages
00000000: 0000 0000 0148 3332 3439 6563 3862 2d34  .....H3249ec8b-4
00000010: 6332 342d 3439 3764 2d61 3338 392d 3865  c24-497d-a389-8e
00000020: 3962 3566 3561 3166 3336 1238 3734 3438  9b5f5a1f36.87448
00000030: 3330 3237 0634 3930 0e32 3032 362d 3036  3027.490.2026-06

__Input
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 productor_avro.py --n 5 --invalido
__Output__
[RECHAZADO POR EL ESQUEMA] ValueError: 'EN_TRAMITE' is not in list
enviados: 0

__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ curl -s http://localhost:8081/schemas/ids/1 | python3 -m json.tool | head -20
__Output__
{
    "schema": "{\"type\":\"record\",\"name\":\"DeclaracionPresentada\",\"namespace\":\"gov.dian.tributaria\",\"doc\":\"Evento emitido cuando un contribuyente presenta una declaraci\u00f3n. Datos sint\u00e9ticos para formaci\u00f3n.\",\"fields\":[{\"name\":\"id_evento\",\"type\":\"string\",\"doc\":\"Identificador \u00fanico del evento (UUID). Clave de idempotencia en los consumidores.\"},{\"name\":\"nit\",\"type\":\"string\",\"doc\":\"NIT del declarante, sin d\u00edgito de verificaci\u00f3n. Clave de partici\u00f3n: conserva el orden por contribuyente.\"},{\"name\":\"formulario\",\"type\":\"string\",\"doc\":\"C\u00f3digo del formulario presentado (110, 300, 350, 490).\"},{\"name\":\"periodo\",\"type\":\"string\",\"doc\":\"Periodo gravable en formato AAAA-MM.\"},{\"name\":\"fecha_presentacion\",\"type\":{\"type\":\"long\",\"logicalType\":\"timestamp-millis\"},\"doc\":\"Momento de la presentaci\u00f3n, en UTC.\"},{\"name\":\"valor_pagado\",\"type\":{\"type\":\"bytes\",\"logicalType\":\"decimal\",\"precision\":16,\"scale\":2},\"doc\":\"Valor pagado en pesos colombianos.\"},{\"name\":\"estado\",\"type\":{\"type\":\"enum\",\"name\":\"EstadoDeclaracion\",\"symbols\":[\"PRESENTADA\",\"ANULADA\",\"CORREGIDA\"]},\"doc\":\"Estado de la declaraci\u00f3n al momento del evento.\"}]}"
}
