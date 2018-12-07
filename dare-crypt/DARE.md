# DARE

Ambiguación: Drug Abuse Resistance Education (D.A.R.E.) Perdón Benjamín no lo pude evitar, desde que lo leí. <br>

<b>DARE</b> Data-at-Rest Encryption o lo que es lo mismo, cifrado de datos en reposo. <br>




## Introducción

En Cloudera además de poder cifrar los datos que viajan por la red, se pueden cifrar los datos que están en reposo, digamos estáticos en nuestros discos duros. <br>

Esto permite que los datos que se almacenan en medios persistentes, no puedan ser leídos; aún cuando la media se extraiga y se pueda acceder físicamente a ella<br>

Este tipo de encriptación se divide en 2:<br>
<br>
*HDFS Transparent Encryption<br>
*Navigator Encrypt<br>
<br>

## HDFS Transparent Encryption

Como su nombre lo indica, este tipo de encriptación es transparante, es decir sin intervención humana, una vez que el ambiente de cloudera esté configurado.<br>

Si bien el cifrado de datos consiste en la generación o aplicación de algortimo criptográfico para cifrar el contenido de un medio digital, en el caso de <b>HDFS</b> es necesario recordar que se trata de un sistema de archivos "montado" sobre el ya existente del sistema operativo, por lo que el sistema ademas de cifrar la región de datos en donde se va almacenar la información; encriptará con una segunda llave el bloque de cada dato.<br>

<b>Implementación del cifrado</b> <br>

Independientemente de la definición de una politica de seguridad que definan por ejemplo la creación estandarizada de contraseñas y asegurarse que estas se cumplan. Se deben implementar mecanismos para la creación y almacenamiento de llaves, por lo que es necesario considerar servicios de KMS, HSM, y KTS. <br>

<b>Usos</b> <br>

La implementación de esta solución permite utilizar la bondad de sentry de permitir acceder información no solo de manera granular, sino de poder segmentar las regiones de datos para ser utilizadas por los usuarios y por los servicios que requieran. <br>

Esta solución es la que más se adapta a las necesidades de negocio de un cliente. <br>

## Navigator Encrypt

Este tipo de solución permite la encriptación transparente de los volumenes de disco duro de los hosts en el cluster; por lo que no solo se puede encriptar la información derivada de la ingesta y manipulación de información con las herramientas de Cloudera, tambien otro tipos de datos, como por ejemplo: Bases de datos, información temporal (yarn y spark), log files, achivos planos (configuración, y fuente externa de datos: csv, txt)<br>

Se debe implementar junto con un servidor de KTS y hace uso de cifrado GPG para encriptar la llave maestra.<br>

## Pregunta
Imagine you have a customer with 40 TB of data in /user/hive/warehouse. HDFS Encryption was not enabled and a customer is now interested in encrypting all data under /user/hive/warehouse. Describe what steps would you take to encrypt the data. What would you and/or the customer need to do to ensure that encryption of all data is posslbe?<br>
<br>
1.- Implementar los certificados de seguridad<br>
2.- Copiar los datos de /user/hive/warehouse a una zona temporal: /user/temporal/data <br>
3.- Crear la estrategia de HDFS Transparent Encryption (llaves y la zona de datos que el cliente quiere) /user/hive/warehouse<br>
4.- Copiar los datos de la zona temporal (/user/temporal/data ) a la zona deseada por el cliente, ya encriptada (/user/hive/warehouse)<br>
5.- Seguramente por temas de gobierno el cliente queria que se respetara el ya creado directorio con datos /user/hive/warehouse<br>





