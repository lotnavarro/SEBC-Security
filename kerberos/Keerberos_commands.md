# Instalación y configuración de Kerberos (server y cliente) <br>

Para cloudera manager y salida de commandos, se comprueba con imagenes.<br>

Se instala el cliente en todos los servidores y el servicio de server en el equipo destinado para ello.

## Server

yum install -y krb5-server krb5-workstation pam_krb5<br>


cd  /var/kerberos/krb5kdc<br>

 kadm5.acl<br>
*/admin@LOT.COM *<br>

kdc.conf<br>

[kdcdefaults]
 kdc_ports = 88
 kdc_tcp_ports = 88
<br>
[realms]
 LOT.COM = {
  #master_key_type = aes256-cts
  acl_file = /var/kerberos/krb5kdc/kadm5.acl
  dict_file = /usr/share/dict/words
  admin_keytab = /var/kerberos/krb5kdc/kadm5.keytab
  supported_enctypes = aes256-cts:normal aes128-cts:normal des3-hmac-sha1:norma$
 }
<br>
krb5.conf
<br>
includedir /etc/krb5.conf.d/
<br>
[logging]
 default = FILE:/var/log/krb5libs.log
 kdc = FILE:/var/log/krb5kdc.log
 admin_server = FILE:/var/log/kadmind.log
<br>
[libdefaults]
 dns_lookup_realm = false
 ticket_lifetime = 24h
 renew_lifetime = 7d
 forwardable = true
 rdns = false
 pkinit_anchors = /etc/pki/tls/certs/ca-bundle.crt
default_realm = LOT.COM
# default_ccache_name = KEYRING:persistent:%{uid}
<br>
[realms]
 LOT.COM = {
  kdc = server.southcentralus.cloudapp.azure.com
  admin_server = server.southcentralus.cloudapp.azure.com
 }
<br>
[domain_realm]<br>

.lot.com=LOT.COM<br>
lot.com=LOT.COM<br>
<br>



kdb5_util create -s -r LOT.COM<br>
<br>passw0rd
<br>
systemctl start krb5kdc kadmin<br>
<br>systemctl enable krb5kdc kadmin
<br>

kadmin.local: addprinc admin<br>
kadmin.local: addprinc user1...4<br>

kadmin.local: quit<br>
## Cliente

KERBEROS CLIENT<br>

yum install -y krb5-workstation<br>

krb5.conf<br>

 
includedir /etc/krb5.conf.d/
<br>
[logging]
 default = FILE:/var/log/krb5libs.log
 kdc = FILE:/var/log/krb5kdc.log
 admin_server = FILE:/var/log/kadmind.log
<br>
[libdefaults]
 dns_lookup_realm = false
 ticket_lifetime = 24h
 renew_lifetime = 7d
 forwardable = true
 rdns = false
 pkinit_anchors = /etc/pki/tls/certs/ca-bundle.crt
default_realm = <b>LOT.COM</b>
# default_ccache_name = KEYRING:persistent:%{uid}
<br>
[realms]<br>
 LOT.COM = {<br>
  kdc = <b>server.southcentralus.cloudapp.azure.com<br></b>
  admin_server = server.southcentralus.cloudapp.azure.com<br>
 }<br>

[domain_realm]<br>
# .example.com = EXAMPLE.COM
# example.com = EXAMPLE.COM<br><b>
.lot.com=LOT.COM<br>
lot.com=LOT.COM<br></b>

resultado...<br>

[root@rakenlot-dn1 raken]# nano /etc/krb5.conf<br>
[root@rakenlot-dn1 raken]# kinit admin@LOT.COM<br>
Password for admin@LOT.COM:<br>
[root@rakenlot-dn1 raken]# klist<br>
Ticket cache: FILE:/tmp/krb5cc_0<br>
Default principal: admin@LOT.COM<br>
<br><b>
Valid starting       Expires              Service principal<br>
12/03/2018 23:30:19  12/04/2018 23:30:19  krbtgt/LOT.COM@LOT.COM<br>
[root@rakenlot-dn1 raken]#</b>





