Lab05

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 productor_facturacion.py --topic "$TOPIC" --n 2000 --ac
ks all
acks=all linger=0 compression=none idempotence=False -> 2000 eventos en 3.58s = 558 ev/s, errores=0
(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ 
...
Partition:4     949441825       {"id_evento": "752dab8e-7b3c-4ddb-87f9-fd9a11434e2b", "nit_emisor": "949441825", "cufe": "5416208905e64cb0acedcc98f6998472", "valor_total": 3024981.79, "estado": "VALIDADA"}
Partition:4     910376309       {"id_evento": "3b7933c5-b46f-4220-91aa-40c93f543f43", "nit_emisor": "910376309", "cufe": "61b65a45e15749a692d0527e11800008", "valor_total": 4628309.19, "estado": "VALIDADA"}
Partition:4     972444559       {"id_evento": "e1f2f80c-d20e-4e99-a1ad-f14739b17b90", "nit_emisor": "972444559", "cufe": "df86eed88964484bbe99334868a711e2", "valor_total": 3219688.7, "estado": "VALIDADA"}
Partition:4     942906373       {"id_evento": "a6ec3c74-19b0-4ae2-b421-012f12a13a53", "nit_emisor": "942906373", "cufe": "bfaf8d723ecc433c8df473d05eba26b9", "valor_total": 870601.84, "estado": "VALIDADA"}
Partition:4     953874049       {"id_evento": "5962d61b-de52-486a-9f45-9cac16bfdf74", "nit_emisor": "953874049", "cufe": "7aef596c97fa43b19d6e58a3b1074057", "valor_total": 146862.06, "estado": "VALIDADA"}
Partition:4     982110970       {"id_evento": "c875c33c-7da4-4906-a21e-03843774f05f", "nit_emisor": "982110970", "cufe": "32843d3f77eb4243b879df21b9d981eb", "valor_total": 3230032.89, "estado": "VALIDADA"}
Partition:4     920626653       {"id_evento": "9d373728-30e5-4c6a-afda-dec32e64f404", "nit_emisor": "920626653", "cufe": "f1d7ed65f12a45aaae22c086d2f5af49", "valor_total": 2072791.23, "estado": "VALIDADA"}
Partition:4     998923149       {"id_evento": "4f66758f-ba87-421d-962f-94aafc71bcf2", "nit_emisor": "998923149", "cufe": "818665eb8c844470be565935026f0192", "valor_total": 4795583.52, "estado": "VALIDADA"}
Partition:4     966755518       {"id_evento": "bb46e583-5745-474f-a836-e5f66733b960", "nit_emisor": "966755518", "cufe": "a315f3527eb04904a8f580f9c3919a5e", "valor_total": 2218081.23, "estado": "VALIDADA"}
Partition:4     983673184       {"id_evento": "a97a814c-86a9-4d17-9ae7-1665e641a886", "nit_emisor": "983673184", "cufe": "af2cd9179b3245418fb831dd6e7cc860", "valor_total": 1839292.21, "estado": "VALIDADA"}
^C(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ 

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ for a in 0 1 all; do python3 productor_facturacion.py --topic "
$TOPIC" --n 20000 --acks $a; done
acks=0 linger=0 compression=none idempotence=False -> 20000 eventos en 2.38s = 8409 ev/s, errores=0
acks=1 linger=0 compression=none idempotence=False -> 20000 eventos en 9.73s = 2056 ev/s, errores=0
acks=all linger=0 compression=none idempotence=False -> 20000 eventos en 22.33s = 896 ev/s, errores=0

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 sh -c 'du -sh /var/lib/kafka/data/"$1"-*' sh "$TOPIC"
8.6M    /var/lib/kafka/data/equipo08.lab05.facturacion.emitida-0
8.6M    /var/lib/kafka/data/equipo08.lab05.facturacion.emitida-1
8.6M    /var/lib/kafka/data/equipo08.lab05.facturacion.emitida-2
8.5M    /var/lib/kafka/data/equipo08.lab05.facturacion.emitida-3
8.6M    /var/lib/kafka/data/equipo08.lab05.facturacion.emitida-4
8.6M    /var/lib/kafka/data/equipo08.lab05.facturacion.emitida-5

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ docker exec broker1 /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server broker1:19092 --topic "$TOPIC" --from-beginning --timeout-ms 20000 2>/dev/null \
  | python3 -c "import sys,json; ids=[json.loads(l)['id_evento'] for l in sys.stdin if l.strip()]; print('total',len(ids),'unicos',len(set(i
ds)))"
total 502000 unicos 502000

(.venv) estudiante@ip-192-168-166-59:/home/est2/equipo08/kafka-dian/entorno$ python3 productor_facturacion.py --topic "$TOPIC" --n 200000 --acks all --linger 20
%3|1793288545.088|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
%3|1793288545.139|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
%3|1793288545.191|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
%3|1793288545.396|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
%3|1793288545.773|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
%3|1793288546.088|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
%3|1793288546.140|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
%3|1793288546.453|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
%3|1793288547.089|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
%3|1793288547.141|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
%3|1793288547.454|FAIL|lab05-productor#producer-1| [thrd:localhost:9092/bootstrap]: localhost:9092/bootstrap: Connect to ipv4#127.0.0.1:9092 failed: Connection refused (after 0ms in state CONNECT)
Traceback (most recent call last):
  File "/home/est2/equipo08/kafka-dian/entorno/productor_facturacion.py", line 60, in <module>
    main()
  File "/home/est2/equipo08/kafka-dian/entorno/productor_facturacion.py", line 52, in main
    p.produce(a.topic, key=k, value=json.dumps(v), callback=entregado)
BufferError: Local: Queue full
%4|1793288547.869|TERMINATE|lab05-productor#producer-1| [thrd:app]: Producer terminating with 100000 messages (17268386 bytes) still in queue or transit: use flush() to wait for outstanding message delivery





