# CHALLENGE

CLOUDERA's Security Services Enablement Bootcamp (SEBC) - CDMX Diciembre 7, 2018


## Instrucciones

- Proof that no sssd, kerberos, tls, etc. are created<br>
- Send me FQDNs of hosts I will issue certs<br>
- Kerberize cluster<br>
- Enable TLS for CM, CM agents, Hive, HDFS (including DNs), and YARN!<br>
- You have an organization that consists of two lines of business:<br>
    - Finance<br>
    - HR<br>
- The organization has TWO types of roles PER line of business: managers and analysts<br>
- EACH line of business has a SINGLE managed database named after itself<br>
- Managers can create and write tables on their own database and read data from all databases<br>
- Analysts can only read data on their line of business database<br>
- The data for each line of business will be encrypted in HDFS<br>
- Create all the Sentry SQL you need to restrict access (you can name the groups, databases, roles, etc. whatever you want provided it is clear what each represents)<br>
- Describe what is required to ensure data is encrypted and ensure managers and analysts can read data from HDFS and Hive<br>
- Describe the capabilities of Navigator Audit, Lineage, and Metadata. What is it useful for?<br>
- Which technology would you use for encrypting Kafka, Kudu and Flume volumes?<br>

## TLS COMNMANDS, results evidence (one server) in images...
<br>
<b>MASTER</b>
<br>
export JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera/<br>
export PATH=$JAVA_HOME/bin:$PATH<br>
<br>
.bash_profile<br>
<br>

keytool -importkeystore -srckeystore lot-mn0.southcentralus.cloudapp.azure.com.p12 -destkeystore lot-mn0.southcentralus.cloudapp.azure.com.jks -srcstoretype pkcs12 -deststoretype JKS
<br>
openssl pkcs12 -in lot-mn0.southcentralus.cloudapp.azure.com.p12 -inkey lot-mn0.southcentralus.cloudapp.azure.com.key -out lot-mn0.southcentralus.cloudapp.azure.com.pem -nodes -clcerts
<br>
openssl req -new -newkey rsa:2048 -nodes -keyout $(hostname -f).key
<br>
/opt/cloudera/security/pki/
<br>
<b>DATANODE 1</b><br>
<br>
export JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera/<br>
export PATH=$JAVA_HOME/bin:$PATH<br>
<br>
.bash_profile<br>
<br>

keytool -importkeystore -srckeystore lot-dn0.southcentralus.cloudapp.azure.com.p12 -destkeystore lot-dn0.southcentralus.cloudapp.azure.com.jks -srcstoretype pkcs12 -deststoretype JKS
<br>
openssl pkcs12 -in lot-dn0.southcentralus.cloudapp.azure.com.p12 -inkey lot-dn0.southcentralus.cloudapp.azure.com.key -out lot-dn0.southcentralus.cloudapp.azure.com.pem -nodes -clcerts
<br>
openssl req -new -newkey rsa:2048 -nodes -keyout $(hostname -f).key
<br>
/opt/cloudera/security/pki/
<br>
<b>DATANODE 2</b><br>
export JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera/<br>
export PATH=$JAVA_HOME/bin:$PATH<br>
<br>
.bash_profile
<br>
	
keytool -importkeystore -srckeystore lot-dn1.southcentralus.cloudapp.azure.com.p12 -destkeystore lot-dn1.southcentralus.cloudapp.azure.com.jks -srcstoretype pkcs12 -deststoretype JKS
<br>
openssl pkcs12 -in lot-dn1.southcentralus.cloudapp.azure.com.p12 -inkey lot-dn1.southcentralus.cloudapp.azure.com.key -out lot-dn1.southcentralus.cloudapp.azure.com.pem -nodes -clcerts
<br>
openssl req -new -newkey rsa:2048 -nodes -keyout $(hostname -f).key
<br>
/opt/cloudera/security/pki/
<br>
<b>DATANODE 3</b>
export JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera/<br>
export PATH=$JAVA_HOME/bin:$PATH<br>
<br>
.bash_profile<br>

	
keytool -importkeystore -srckeystore lot-dn2.southcentralus.cloudapp.azure.com.p12 -destkeystore lot-dn2.southcentralus.cloudapp.azure.com.jks -srcstoretype pkcs12 -deststoretype JKS
<br>
openssl pkcs12 -in lot-dn2.southcentralus.cloudapp.azure.com.p12 -inkey lot-dn2.southcentralus.cloudapp.azure.com.key -out lot-dn2.southcentralus.cloudapp.azure.com.pem -nodes -clcerts
<br>
openssl req -new -newkey rsa:2048 -nodes -keyout $(hostname -f).key
<br>
/opt/cloudera/security/pki/
<br>
<b>DATANODE 4</b>
export JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera/<br>
export PATH=$JAVA_HOME/bin:$PATH<br>
<br>
.bash_profile<br>

	<br>
keytool -importkeystore -srckeystore lot-dn3.southcentralus.cloudapp.azure.com.p12 -destkeystore lot-dn3.southcentralus.cloudapp.azure.com.jks -srcstoretype pkcs12 -deststoretype JKS<br>
<br>
openssl pkcs12 -in lot-dn3.southcentralus.cloudapp.azure.com.p12 -inkey lot-dn3.southcentralus.cloudapp.azure.com.key -out lot-dn3.southcentralus.cloudapp.azure.com.pem -nodes -clcerts
<br>
openssl req -new -newkey rsa:2048 -nodes -keyout $(hostname -f).key
<br>
/opt/cloudera/security/pki/
<br>
<b> Todos los hosts </b><br>

keytool -importcert -alias rootca -keystore $JAVA_HOME/jre/lib/security/cacerts -file /opt/cloudera/security/pki/cacerts
<br>

## enlaces simbolicos

ln -s /opt/cloudera/security/pki/lot-mn0.southcentralus.cloudapp.azure.com.key /opt/cloudera/security/pki/cert.key
<br>
ln -s /opt/cloudera/security/pki/lot-mn0.southcentralus.cloudapp.azure.com.pem /opt/cloudera/security/pki/cert.pem
<br>
ln -s /opt/cloudera/security/pki/lot-mn0.southcentralus.cloudapp.azure.com.jks /opt/cloudera/security/pki/cert.jks
<br>
ln -s /opt/cloudera/security/pki/lot-dn0.southcentralus.cloudapp.azure.com.key /opt/cloudera/security/pki/cert.key
<br>
ln -s /opt/cloudera/security/pki/lot-dn0.southcentralus.cloudapp.azure.com.pem /opt/cloudera/security/pki/cert.pem
<br>
ln -s /opt/cloudera/security/pki/lot-dn0.southcentralus.cloudapp.azure.com.jks /opt/cloudera/security/pki/cert.jks
<br>
ln -s /opt/cloudera/security/pki/lot-dn1.southcentralus.cloudapp.azure.com.key /opt/cloudera/security/pki/cert.key
<br>
ln -s /opt/cloudera/security/pki/lot-dn1.southcentralus.cloudapp.azure.com.pem /opt/cloudera/security/pki/cert.pem
<br>
ln -s /opt/cloudera/security/pki/lot-dn1.southcentralus.cloudapp.azure.com.jks /opt/cloudera/security/pki/cert.jks
<br>
ln -s /opt/cloudera/security/pki/lot-dn2.southcentralus.cloudapp.azure.com.key /opt/cloudera/security/pki/cert.key
<br>
ln -s /opt/cloudera/security/pki/lot-dn2.southcentralus.cloudapp.azure.com.pem /opt/cloudera/security/pki/cert.pem
<br>
ln -s /opt/cloudera/security/pki/lot-dn2.southcentralus.cloudapp.azure.com.jks /opt/cloudera/security/pki/cert.jks
<br>
ln -s /opt/cloudera/security/pki/lot-dn3.southcentralus.cloudapp.azure.com.key /opt/cloudera/security/pki/cert.key
<br>
ln -s /opt/cloudera/security/pki/lot-dn3.southcentralus.cloudapp.azure.com.pem /opt/cloudera/security/pki/cert.pem
<br>
ln -s /opt/cloudera/security/pki/lot-dn3.southcentralus.cloudapp.azure.com.jks /opt/cloudera/security/pki/cert.jks
<br>



## Sentry Business Case

Los comandos para la creación de gobierno en sentry para dicho cliente son:<br>
<br>
*create role finance_managers;<br>
*create role finance_analysts;<br>create role HR_managers;<br>
*create role HR_analysts;<br>
*grant role managers to group managers;<br>
*grant role analysts to group analysts;<br>
*grant all on database managers to role managers;<br>
*grant select on database analysts to role managers;<br>
*grant select on database analysts to role analysts;<br>
<br>

## Questions
- Describe what is required to ensure data is encrypted and ensure managers and analysts can read data from HDFS and Hive<br>

*

- Describe the capabilities of Navigator Audit, Lineage, and Metadata. What is it useful for?<br>

*Navigator Audit, es un servicio que "visualiza" y graba cada acción que se realiza en el Cluster, en los nodos, a nivel servicio o usuario. <br>
De esta manera podemos revisar las acciones realizadas en el ciclo de vida del dato. 

<br>
*Navigator Lineage, es un servicio que permite revisar las relaciones jerarquicas de los artifactos de cloudera. Por lo que podemos ver como el dato se va transformando durante su ciclo de vida, estas jerarquias son entidad / relación. <br>

Se pueden visualizar los equemas de: hive, impala, sqoop, pig, hdfs, etc. 

<br>
*Navigator Metadata, es un servicio que permite visualizar y administrar el metadato interno de los objetos de cloudera. De manera nativa, se guarda la configuración de dicho metadato, objeto por objeto y se puede acceder a ella para facilitar la administración o gobierno de los objetos y del dato. 

<br>

De manera adicional se permite la configuración de metadatos por objeto de manera <b>AD-HOC</b>, es decir customizada, pera llevar un mejor control o llevar un control más intuitivo.
<br>

- Which technology would you use for encrypting Kafka, Kudu and Flume volumes?<br>

Con Navigator encrypt podemos encriptar los volumenes donde "vive" la información que se genera con Kafka, kudu y Flume. Es importante considerar que la recomendación es que cada servicio viva en su propio host, el hecho de que viva en diferentes discos en un mismo host, afecta el rendimiento de los workers por la relación contenedores / espacio en disco. <br>


