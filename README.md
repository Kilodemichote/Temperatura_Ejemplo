# Temperatura_Ejemplo

**_Ordenar números de Menor a Mayor (Temperaturas)_**

Cómo bajar un ejemplo de MapReduce
Vamos a usar un archivo `.jar` que ya trae todo listo para echar a andar un ejemplo con MapReduce en Hadoop. Así no nos quebramos la cabeza compilando cosas.

Abre este link:
https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/

Descarga el archivo que dice:
`hadoop-mapreduce-examples-2.7.1-sources.jar`

Ya que lo tengas, nomás mételo en la misma carpeta donde está tu proyecto de `docker-hadoop`.

**_Archivo de temperaturas_**
Hadoop normalmente se usa con archivos enormes (tipo varios gigas), pero para no complicarnos, vamos a usar uno chiquito, como de 10 KB, que trae puras temperaturas anotadas durante varios años.

Entra a este link:
https://github.com/Rkrahul04/Maximum_Temp_Calculation_mapreduce/blob/master/Dataset%20-%20Calculate%20Maximum%20Temperature/Temperature

Descarga el archivo Temperature.txt.
(Si viene en un zip o algo así, descomprímelo primero.)

Después, nomás pon ese archivo en la misma carpeta donde tienes el repo de `docker-hadoop`.

**_Pasar los archivos al contenedor_**
Ya que tienes el` .jar` con el ejemplo y el `.txt` con las temperaturas, ahora toca meterlos al contenedor de Hadoop, o sea, al lugar donde se va a hacer la chamba.

Abre tu terminal y escribe esto para copiar el archivo .jar al contenedor:
`docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp`
Con eso, el archivo se va al contenedor llamado` namenode`, directo a la carpeta` /tmp.`

Ahora haz lo mismo con el archivo de temperaturas:
`docker cp Temperature.txt namenode:/tmp`
¡Y listo! Ya tienes los dos archivos dentro del contenedor, todo listo para empezar a jugar con MapReduce.

**_Crear la carpeta en el contenedor_**
Primero entra al contenedor `namenode` ejecutando:
`docker exec -it namenode bash`

Luego, ejecuta lo siguiente:
`hdfs dfs -mkdir /user/root/input_temperaturas`

**_Copiar tu archivo .txt a HDFS_**
Primero vamos a ejecutar:
`cd /tmp`

Después:
`hdfs dfs -put Temperature.txt /user/root/input_temperaturas`

**_Ejecutar MapReduce_**
Ejecutamos el siguiente comando (todo en una sola línea):
`hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.SecondarySort input_temperaturas output_temperaturas`

V**_er los resultados_**
Ejecutamos:
`hdfs dfs -cat /user/root/output_temperaturas/*`

Puedes comprobar los resultados accediendo a la carpeta de salida con:
`hdfs dfs -ls /user/root/output_temperaturas`

Nos interesa el archivo part-r-00000, que contiene nuestro recuento de palabras. Podemos exportarlo, colocarlo en un archivo .txt y moverlo a nuestra carpeta base.

Para hacer lo anterior, debemos ejecutar:
h`dfs dfs -cat /user/root/output_temperaturas/part-r-00000 > /tmp/temperaturas_ordenadas.txt`

Después ejecutamos:
`exit`

Por último, ejecutamos:
`docker cp namenode:/tmp/temperaturas_ordenadas.txt .`
Ahora el archivo de texto está en la carpeta del repositorio` docker-hadoop.`
