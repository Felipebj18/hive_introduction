# Hive

## Introducción

- Conectarse a servidor de hive

 ```sh
ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
```

- Consultar tablas existentes

```sql
show tables;
```

- Mostrar el esquema de hivesampletable:

```sql
describe hivesampletable;
```

- Ejemplo de un conjunto de consultas

```sh
jdbc:hive2://headnodehost:10001/> CREATE TABLE x (a INT);

No rows affected (0.363 seconds)

jdbc:hive2://headnodehost:10001/> SELECT * FROM x;

+------+
| x.a  |
+------+
+------+
No rows selected (0.86 seconds)

jdbc:hive2://headnodehost:10001/> SELECT *
. . . . . . . . . . . . . . . . > FROM x;

+------+
| x.a  |
+------+
+------+
No rows selected (1.274 seconds)
```

## Comandos "One Shot"

Si se desean correr una o más consultas (separadas por punto y coma) y se desea que el cliente de hive termine inmediatamente después de completarlas. el cliente acepta el argumento **-e** para actuvar este feature.

```sh
$ beeline -e "SELECT clientid FROM hivesampletable LIMIT 10"

+-----------+
| clientid  |
+-----------+
| 8         |
| 23        |
| 23        |
| 23        |
| 28        |
| 28        |
| 28        |
| 28        |
| 28        |
| 28        |
+-----------+
10 rows selected (1.04 seconds)
```

- Una técnica rápida y "sucia" es usar esta función para enviar los resultados de la consulta a un archivo.  
Al agregar el parámetro `--silent=true` para el modo silencioso, se eliminan las líneas innecesarias y el Tiempo tomado.

```sh
$ beeline --silence=true -e "SELECT clientid FROM hivesampletable LIMIT 10" > resultado.txt
$ cat resultado.txt

+-----------+
| clientid  |
+-----------+
| 8         |
| 23        |
| 23        |
| 23        |
| 28        |
| 28        |
| 28        |
| 28        |
| 28        |
| 28        |
+-----------+
```

Nota:  
**Tenga en cuenta que beeline escribió la salida en la salida estándar del sistema operativo y el comando de shell ( > ) redirigió esa salida al sistema de archivos local, no al HDFS.**

- Usar archivos con el shell del cliente de hive

```sh
$ echo 'SELECT clientid FROM hivesampletable LIMIT 10' > clients.hql

cat clients.hql
SELECT clientid FROM hivesampletable LIMIT 10

$ beeline

0: jdbc:hive2://zk0-bigdat.1gwna0z40lrejmqu23>!run /path/to/file/clients.hql

+-----------+
| clientid  |
+-----------+
| 8         |
| 23        |
| 23        |
| 23        |
| 28        |
| 28        |
| 28        |
| 28        |
| 28        |
| 28        |
+-----------+
10 rows selected (0.912 seconds)
```

- Ejecutar el hql desde el shell como one shot command

```sh
$ beeline -i clients.hql

+-----------+
| clientid  |
+-----------+
| 8         |
| 23        |
| 23        |
| 23        |
| 28        |
| 28        |
| 28        |
| 28        |
| 28        |
| 28        |
+-----------+
10 rows selected (1.469 seconds)
```
