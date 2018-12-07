# Sentry

## Utilización de beeline para acceder a tablas de hive	

Mediante kinit obtener el ticket de kerberos para cada usuario en cuestión<br>

Conectarse:<br>

!connect jdbc:hive2://rakenlot-mn0.southcentralus.cloudapp.azure.com:10000/default;principal=hive/rakenlot-mn0.southcentralus.cloudapp.azure.com@LOT.COM;ssl=true;<br>

Crear objetos:<br>
<br>
CREATE table tabla1(<br>
STATION STRING,<br>
STATION_NAME STRING,<br>
WDATE STRING,<br>
PRCP FLOAT,<br>
WIND INT,<br>
SNOW INT<br>
);<br>
<br>
CREATE table tabla2(<br>
STATION STRING,<br>
STATION_NAME STRING,<br>
WDATE STRING,<br>
PRCP FLOAT,<br>
WIND INT,<br>
SNOW INT<br>
);<br>
<br>


hdfs dfs -put /home/raken/tabla1.csv /tmp/tabla1.csv<br>
hdfs dfs -put /home/raken/tabla2.csv /tmp/tabla2.csv<br>
<br>
LOAD DATA INPATH '/tmp/tabla1.csv' OVERWRITE INTO TABLE tabla1;<br>
<br>

LOAD DATA INPATH '/tmp/tabla2.csv' OVERWRITE INTO TABLE tabla2;<br>

<b>Asignar seguridad</b><br>


grant all on table1 to role managers;<br>

grant select on table2 to role analysts;<br>

grant all on default to role admins;<br>

<b>Resultados en las imagenes</b>



