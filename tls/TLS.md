# TLS

## Preconfiguración de certificados.	

Se adecuan los certificados al formato a utilizar (para todos los hosts)<br>

openssl pkcs12 -in rakenlot-mn0.southcentralus.cloudapp.azure.com.pfx -out rakenlot-mn0.southcentralus.cloudapp.azure.com.pem -nodes<br>
openssl pkcs12 -in rakenlot-dn0.southcentralus.cloudapp.azure.com.pfx -out rakenlot-dn0.southcentralus.cloudapp.azure.com.pem -nodes<br>
openssl pkcs12 -in rakenlot-dn1.southcentralus.cloudapp.azure.com.pfx -out rakenlot-dn1.southcentralus.cloudapp.azure.com.pem -nodes<br>
openssl pkcs12 -in rakenlot-dn2.southcentralus.cloudapp.azure.com.pfx -out rakenlot-dn2.southcentralus.cloudapp.azure.com.pem -nodes<br>
openssl pkcs12 -in rakenlot-dn3.southcentralus.cloudapp.azure.com.pfx -out rakenlot-dn3.southcentralus.cloudapp.azure.com.pem -nodes<br>

openssl pkcs12 -in rootca.pfx -out rootca.pem -nodes
openssl pkcs12 -in intermediateca.pfx -out intermediateca.pem -nodes

sudo cp *.pem /opt/cloudera/security/pki/


## Se concatenan los certificados y se valida la integridad del mismo

<br>
cat intermediateca.pem rootca.pem > rakenlot-dn3.pem<br>
openssl verify -verbose -CAfile rakenlot-dn3.pem rakenlot-dn3.southcentralus.cloudapp.azure.com.pem<br>

cat intermediateca.pem rootca.pem > rakenlot-dn2.pem<br>
openssl verify -verbose -CAfile rakenlot-dn2.pem rakenlot-dn2.southcentralus.cloudapp.azure.com.pem<br>

cat intermediateca.pem rootca.pem > rakenlot-dn1.pem<br>
openssl verify -verbose -CAfile rakenlot-dn1.pem rakenlot-dn1.southcentralus.cloudapp.azure.com.pem<br>

cat intermediateca.pem rootca.pem > rakenlot-dn0.pem<br>
openssl verify -verbose -CAfile rakenlot-dn0.pem rakenlot-dn0.southcentralus.cloudapp.azure.com.pem<br>


cat intermediateca.pem rootca.pem > rakenlot-mn0.pem<br>
openssl verify -verbose -CAfile rakenlot-mn0.pem rakenlot-mn0.southcentralus.cloudapp.azure.com.pem<br>

## Se crean los certificados JKS y se importan para que JAVA los use

<b>Master</b><br>

openssl pkcs12 -export -in intermediateca.pem -in rakenlot-mn0.southcentralus.cloudapp.azure.com.pem -inkey rakenlot-mn0.southcentralus.cloudapp.azure.com.key -out lotkeystore.p12
<br>
keytool -importkeystore -srckeystore lotkeystore.p12 -destkeystore rakenlot-mn0.southcentralus.cloudapp.azure.com.jks -srcstoretype pkcs12 -deststoretype JKS
<br>
keytool -importcert -alias rootca -keystore $JAVA_HOME/jre/lib/security/cacerts -file /opt/cloudera/security/pki/rootca.pem
<br>
<b>DN0</b>

openssl pkcs12 -export -in intermediateca.pem -in rakenlot-dn0.southcentralus.cloudapp.azure.com.pem -inkey rakenlot-dn0.southcentralus.cloudapp.azure.com.key -out lotkeystore.p12
<br>
keytool -importkeystore -srckeystore lotkeystore.p12 -destkeystore rakenlot-dn0.southcentralus.cloudapp.azure.com.jks -srcstoretype pkcs12 -deststoretype JKS
<br>
keytool -importcert -alias rootca -keystore $JAVA_HOME/jre/lib/security/cacerts -file /opt/cloudera/security/pki/rootca.pem
<br>
<b>DN1</b>

openssl pkcs12 -export -in intermediateca.pem -in rakenlot-dn1.southcentralus.cloudapp.azure.com.pem -inkey rakenlot-dn1.southcentralus.cloudapp.azure.com.key -out lotkeystore.p12
<br>
keytool -importkeystore -srckeystore lotkeystore.p12 -destkeystore rakenlot-dn1.southcentralus.cloudapp.azure.com.jks -srcstoretype pkcs12 -deststoretype JKS
<br>
keytool -importcert -alias rootca -keystore $JAVA_HOME/jre/lib/security/cacerts -file /opt/cloudera/security/pki/rootca.pem
<br>
<b>DN2</b>

openssl pkcs12 -export -in intermediateca.pem -in rakenlot-dn2.southcentralus.cloudapp.azure.com.pem -inkey rakenlot-dn2.southcentralus.cloudapp.azure.com.key -out lotkeystore.p12
<br>
keytool -importkeystore -srckeystore lotkeystore.p12 -destkeystore rakenlot-dn2.southcentralus.cloudapp.azure.com.jks -srcstoretype pkcs12 -deststoretype JKS
<br>
keytool -importcert -alias rootca -keystore $JAVA_HOME/jre/lib/security/cacerts -file /opt/cloudera/security/pki/rootca.pem
<br>
<b>DN3</b>

openssl pkcs12 -export -in intermediateca.pem -in rakenlot-dn3.southcentralus.cloudapp.azure.com.pem -inkey rakenlot-dn3.southcentralus.cloudapp.azure.com.key -out lotkeystore.p12
<br>
keytool -importkeystore -srckeystore lotkeystore.p12 -destkeystore rakenlot-dn3.southcentralus.cloudapp.azure.com.jks -srcstoretype pkcs12 -deststoretype JKS
<br>
keytool -importcert -alias rootca -keystore $JAVA_HOME/jre/lib/security/cacerts -file /opt/cloudera/security/pki/rootca.pem
<br>
sudo cp rakenlot-dn3.southcentralus.cloudapp.azure.com.jks /opt/cloudera/security/pki/
<br>
## Se prueban desde consola

java -cp /home/raken SSLPoke rakenlot-mn0.southcentralus.cloudapp.azure.com 7183

<br>



