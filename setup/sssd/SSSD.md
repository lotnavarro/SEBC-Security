# Instalación y configuración de SSSD	

Se instala el cliente en todos los servidores y se configura

## Instalación


yum -y install sssd-client sssd-tools sssd oddjob-mkhomedir authconfig

## Configuración e inicialización de servicios



-export LDAP_SERVER=<b>server.southcentralus.cloudapp.azure.com</b>&nbsp;
-export KERBEROS_REALM=<b>LOT.COM</b>&nbsp;
-export LDAP_SEARCH_BASE=dc=<b>southcentralus,dc=cloudapp,dc=azure,dc=com</b>&nbsp;


-authconfig --enableshadow --passalgo=sha512 --enablesssd --enablesssdauth --enablemkhomedir --updateall&nbsp;

echo "[sssd]
config_file_version = 2
services = nss, pam, autofs
domains = default
debug_level = 0
&nbsp;
[nss]
filter_users = root,ldap,named,avahi,haldaemon,dbus,radiusd,news,nscd
&nbsp;
[pam]
&nbsp;
[domain/default]
debug_level = 0
id_provider = ldap
auth_provider = ldap
autofs_provider = ldap
chpass_provider = ldap
cache_credentials = False
entry_cache_timeout = 600
#ldap_schema = rfc2307bis
ldap_tls_reqcert = never
ldap_search_base = ${LDAP_SEARCH_BASE}
ldap_user_object_class = posixAccount
ldap_group_object_class = posixGroup
ldap_user_name = uid
ldap_user_home_directory = homeDirectory
ldap_group_member = memberUid
ldap_id_use_start_tls = False
ldap_uri = ldap://${LDAP_SERVER}:389
ldap_chpass_uri = ldap://${LDAP_SERVER}:389
ldap_network_timeout = 3
ldap_tls_cacertdir = ${CERTS}
krb5_kdcip = ${LDAP_SERVER}
krb5_server = ${LDAP_SERVER}
krb5_realm = ${KERBEROS_REALM}
&nbsp;
[autofs]" > /etc/sssd/sssd.conf
&nbsp;
chmod 600 /etc/sssd/sssd.conf
&nbsp;
-<b>systemctl start sssd.service</b>&nbsp;
-<b>systemctl enable sssd.service</b>&nbsp;

centos 6x commands:
-

#service sssd start
#chkconfig sssd on

