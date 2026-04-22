# Nivel medio (fallos reales y quórum)

He utilizado estos comandos para crear el usuario que vamos a usar.  

```sql
CREATE USER 'sbtest'@'192.168.100.155' IDENTIFIED BY 'sbpass';
GRANT ALL PRIVILEGES ON galera_test.* TO 'sbtest'@'192.168.100.155';
FLUSH PRIVILEGES;
```

despues he echo las pruebas con estos comandos.  

Con este comando he creado las tablas que vamos a usar para la prueba. Antes he instalado sysbench para facilitar las pruebas.

```bash
sysbench /usr/share/sysbench/oltp_read_write.lua \
  --mysql-host=192.168.100.153 \
  --mysql-user=sbtest \
  --mysql-password=sbpass \
  --mysql-db=galera_test \
  --tables=4 \
  --table-size=100000 \
  prepare
```

ahora he usado este comando para que haya una constante escitura y lectura en la tablas de la base de datos.

```bash
sysbench /usr/share/sysbench/oltp_read_write.lua \
  --mysql-host=192.168.100.153 \
  --mysql-user=sbtest \
  --mysql-password=sbpass \
  --mysql-db=galera_test \
  --threads=16 \
  --time=300 \
  --report-interval=5 \
  run
```

y esta ha sido la salida cuando he cerrado el nodo.

```bash
Initializing worker threads...

Threads started!

[ 5s ] thds: 16 tps: 260.12 qps: 5246.13 (r/w/o: 3679.43/896.91/669.79) lat (ms,95%): 108.68 err/s: 0.20 reconn/s: 0.00
[ 10s ] thds: 16 tps: 215.83 qps: 4332.59 (r/w/o: 3034.02/786.51/512.07) lat (ms,95%): 161.51 err/s: 0.40 reconn/s: 0.00
[ 15s ] thds: 16 tps: 253.01 qps: 5065.77 (r/w/o: 3545.52/920.63/599.62) lat (ms,95%): 110.66 err/s: 0.20 reconn/s: 0.00
[ 20s ] thds: 16 tps: 267.34 qps: 5345.17 (r/w/o: 3741.54/978.18/625.46) lat (ms,95%): 97.55 err/s: 0.40 reconn/s: 0.00
[ 25s ] thds: 16 tps: 278.84 qps: 5584.30 (r/w/o: 3910.03/1028.57/645.70) lat (ms,95%): 92.42 err/s: 0.60 reconn/s: 0.00
[ 30s ] thds: 16 tps: 276.98 qps: 5538.95 (r/w/o: 3878.48/1034.52/625.95) lat (ms,95%): 95.81 err/s: 0.20 reconn/s: 0.00
[ 35s ] thds: 16 tps: 253.83 qps: 5077.02 (r/w/o: 3551.63/952.52/572.87) lat (ms,95%): 104.84 err/s: 0.00 reconn/s: 0.00
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'COMMIT'
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'SELECT c FROM sbtest1 WHERE id BETWEEN ? AND ?'
FATAL: `thread_run' function failed: /usr/share/sysbench/oltp_common.lua:432: SQL error, errno = 1047, state = '08S01': WSREP has not yet prepared node for application use
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'UPDATE sbtest1 SET c=? WHERE id=?'
FATAL: `thread_run' function failed: /usr/share/sysbench/oltp_common.lua:469: SQL error, errno = 1047, state = '08S01': WSREP has not yet prepared node for application use
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'SELECT c FROM sbtest1 WHERE id=?'
FATAL: `thread_run' function failed: /usr/share/sysbench/oltp_common.lua:419: SQL error, errno = 1047, state = '08S01': WSREP has not yet prepared node for application use
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'UPDATE sbtest1 SET k=k+1 WHERE id=?'
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'UPDATE sbtest1 SET c=? WHERE id=?'
(last message repeated 1 times)
FATAL: `thread_run' function failed: /usr/share/sysbench/oltp_common.lua:469: SQL error, errno = 1047, state = '08S01': WSREP has not yet prepared node for application use
(last message repeated 1 times)
FATAL: `thread_run' function failed: /usr/share/sysbench/oltp_common.lua:458: SQL error, errno = 1047, state = '08S01': WSREP has not yet prepared node for application use
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'UPDATE sbtest1 SET c=? WHERE id=?'
FATAL: `thread_run' function failed: /usr/share/sysbench/oltp_common.lua:469: SQL error, errno = 1047, state = '08S01': WSREP has not yet prepared node for application use
FATAL: `thread_run' function failed: /usr/share/sysbench/oltp_common.lua:409: SQL error, errno = 1047, state = '08S01': WSREP has not yet prepared node for application use
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'SELECT c FROM sbtest1 WHERE id=?'
FATAL: `thread_run' function failed: /usr/share/sysbench/oltp_common.lua:419: SQL error, errno = 1047, state = '08S01': WSREP has not yet prepared node for application use
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'SELECT c FROM sbtest1 WHERE id=?'
(last message repeated 1 times)
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'SELECT c FROM sbtest1 WHERE id=?'
FATAL: `thread_run' function failed: /usr/share/sysbench/oltp_common.lua:419: SQL error, errno = 1047, state = '08S01': WSREP has not yet prepared node for application use
FATAL: mysql_stmt_execute() returned error 1047 (WSREP has not yet prepared node for application use) for query 'UPDATE sbtest1 SET k=k+1 WHERE id=?'
FATAL: `thread_run' function failed: /usr/share/sysbench/oltp_common.lua:458: SQL error, errno = 1047, state = '08S01': WSREP has not yet prepared node for application use
```

antes de tirar el nodo tenia esto parametros

```sql
MariaDB [(none)]> SHOW STATUS LIKE 'wsrep_cluster_size';
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| wsrep_cluster_size | 3     |
+--------------------+-------+
1 row in set (0,003 sec)

MariaDB [(none)]> SHOW STATUS LIKE 'wsrep_local_state_comment';
+---------------------------+--------+
| Variable_name             | Value  |
+---------------------------+--------+
| wsrep_local_state_comment | Synced |
+---------------------------+--------+
1 row in set (0,001 sec)

MariaDB [(none)]> SHOW STATUS LIKE 'wsrep_ready';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| wsrep_ready   | ON    |
+---------------+-------+
1 row in set (0,001 sec)
```

despues de tirar el nodo tenia estos

```sql
MariaDB [(none)]> SHOW STATUS LIKE 'wsrep_cluster_size';
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| wsrep_cluster_size | 2     |
+--------------------+-------+
1 row in set (0,015 sec)

MariaDB [(none)]> SHOW STATUS LIKE 'wsrep_ready';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| wsrep_ready   | ON    |
+---------------+-------+
1 row in set (0,001 sec)

MariaDB [(none)]> SHOW STATUS LIKE 'wsrep_cluster_status';
+----------------------+---------+
| Variable_name        | Value   |
+----------------------+---------+
| wsrep_cluster_status | Primary |
+----------------------+---------+
1 row in set (0,001 sec)
```

Al iniciar el nodo funciona todo correctamente.

# Nivel alto (consistencia, recuperación y estrés)

vamos a usar 2 clientes para escribir silmutaneamente en la base de datos.

Cliente 1

```bash
sysbench /usr/share/sysbench/oltp_write_only.lua \
  --mysql-host=192.168.100.153 \
  --mysql-user=sbtest \
  --mysql-password=sbpass \
  --mysql-db=galera_test \
  --tables=1 \
  --threads=8 \
  --time=120 \
  --rand-type=uniform \
  --rand-range=1-50 \
  run
```

Cliente 2

```bash
sysbench /usr/share/sysbench/oltp_write_only.lua \
  --mysql-host=192.168.100.154 \
  --mysql-user=sbtest \
  --mysql-password=sbpass \
  --mysql-db=galera_test \
  --tables=1 \
  --threads=8 \
  --time=120 \
  --rand-type=uniform \
  --rand-range=1-50 \
  run
```

Lo que pasa es que el galera aborta operacion que modifique la misma tupla.

ahora vamos a forzar la desactualización de la base de datos.

primero detenemos la base de datos en un nodo,`systemctl stop mariadb`, Mientras tanto, ejecuta carga en el cluster con este comonado `sysbench oltp_write_only.lua ... --time=600 run` ahora iniciamos el nodo de nuevo `systemctl start mariadb` y podemos mirar los logs `journalctl -u mariadb | grep -i sst`.

despues podemos medir la carga del sst primero vamos a hacer una carga sostenida en el nodo.

```bash
sysbench oltp_read_write.lua \
  --threads=16 \
  --time=600 \
  --report-interval=10 \
  run
```

podemos observar con el siguiente comando `SHOW STATUS LIKE 'wsrep_flow_control_paused';` que aumenta la latencia y hay una caida de TPS.
Observaciones:

- Rendimiento disminuye temporalmente
- El cluster sigue operativo
- El impacto depende del método SST

Durante el sst tambien podemos ver con `top` como el consumo de la CPU elevado, Disco muy activo, Memoria estable, Nodo sigue operativo pero degradado.

despues de las pruebas la conclusión es que no hubo:

- datos divergentes
- transacciones fantasma
- corrupción

El clúster Galera mantuvo consistencia total frente a conflictos de escritura, caídas abruptas, reinicios consecutivos y procesos de sincronización completa. Durante dichos eventos se observaron abortos de transacción y degradación temporal del rendimiento, lo cual corresponde al funcionamiento normal del mecanismo de certificación de Galera. En ningún caso se produjo pérdida o divergencia de datos, confirmando la robustez del clúster ante fallos encadenados y carga concurrente.
