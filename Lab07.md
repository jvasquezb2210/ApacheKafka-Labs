# Lab07

## Paso 1
__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 gen_eventos.py --topic "$TOPIC" --rate 500 --minutes 12

__Output__
enviados: 328652

## Paso 2
__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ SLEEP_MS=5 python3 consumidor_facturacion.py --topic "$TOPIC" --group "$GROUP" --upsert

## Paso 3
__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ for i in $(seq 1 6); do date +%T; kg --describe --group "$GROUP" | awk 'NR>1 {lag+=$6} END {print "LAG TOTAL:", lag}'; sleep 30; done
16:07:59

__Output__
14:38:58
LAG TOTAL: 1158155
14:39:32
LAG TOTAL: 1151960
14:40:05
LAG TOTAL: 1383605
14:40:40
LAG TOTAL: 1377028
14:41:13
LAG TOTAL: 1370662
14:41:46
LAG TOTAL: 1364261

### 3 minutos
__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ for i in $(seq 1 6); do date +%T; kg --describe --group "$GROUP" | awk 'NR>1 {lag+=$6} END {print "LAG TOTAL:", lag}'; sleep 180; done
__Output__
14:50:06
LAG TOTAL: 1268356
14:53:10
LAG TOTAL: 1233878
14:56:13
LAG TOTAL: 1197949
14:59:16
LAG TOTAL: 1161860
15:02:19
LAG TOTAL: 1126904
15:05:22
LAG TOTAL: 1091156
## Paso 5
'''
  c = Consumer({"bootstrap.servers": a.bootstrap, "group.id": a.group, "enable.auto.commit": False,
                  "auto.offset.reset": "earliest", "max.poll.interval.ms": 300000,
                  "partition.assignment.strategy": "range"})
'''
__Inpunt__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ for i in $(seq 1 6); do date +%T; kg --describe --group "$GROUP" | awk 'NR>1 {lag+=$6} END {print "LAG TOTAL:", lag}'; sleep 30; done
__Output__
14:12:38
LAG TOTAL: 596031
14:13:19
LAG TOTAL: 604559
14:13:57
LAG TOTAL: 613038

'''
    c = Consumer({"bootstrap.servers": a.bootstrap, "group.id": a.group, "enable.auto.commit": False,
                  "auto.offset.reset": "earliest", "max.poll.interval.ms": 300000,
                  "partition.assignment.strategy": "cooperative-sticky"})
'''
__Inpunt__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ for i in $(seq 1 6); do date +%T; kg --describe --group "$GROUP" | awk 'NR>1 {lag+=$6} END {print "LAG TOTAL:", lag}'; sleep 30; done
__Output__
14:15:56
LAG TOTAL: 650138
14:16:34
LAG TOTAL: 658116
14:17:11
LAG TOTAL: 668909
__Input__
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ grep -n "partition.assignment.strategy" consumidor_facturacion.py
__Output__
29:                  "partition.assignment.strategy": "cooperative-sticky"})