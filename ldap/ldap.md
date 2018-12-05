# Integración de Cloudera Manager sobre LDAP	

Se configura el cluster desde cloudera manager para que pueda utilizar los usuarios creados en LDAP, el de administración <b>admin</b> debe poder tener los más altos privilegios.

## Validación de creación de objetos en openldap

<br>

ldapsearch -h server -b "ou=machines,dc=southcentralus,dc=cloudapp,dc=azure,dc=com" -x <br>

ldapsearch -h server -b "ou=groups,dc=southcentralus,dc=cloudapp,dc=azure,dc=com" -x<br>

ldapsearch -h server.southcentralus.cloudapp.azure.com -b "ou=users,dc=southcentralus,dc=cloudapp,dc=azure,dc=com" -x<br>

ldapsearch -h server.southcentralus.cloudapp.azure.com -b "ou=people,dc=southcentralus,dc=cloudapp,dc=azure,dc=com" -x<br>

[raken@server ~]$ ldapsearch -h server.southcentralus.cloudapp.azure.com -b "ou=people,dc=southcentralus,dc=cloudapp,dc=azure,dc=com" -x<br>
-# extended LDIF<br>
-#
-# LDAPv3<br>
-# base <ou=people,dc=southcentralus,dc=cloudapp,dc=azure,dc=com> with scope subtree<br>
-# filter: (objectclass=*)<br>
-# requesting: ALL<br>
-#<br>
<br>
-# people, southcentralus.cloudapp.azure.com<br>
dn: ou=people,dc=southcentralus,dc=cloudapp,dc=azure,dc=com<br>
objectClass: organizationalUnit<br>
ou: people<br>
description: Generic people branch<br>

-# admin, people, southcentralus.cloudapp.azure.com<br>
dn: uid=admin,ou=people,dc=southcentralus,dc=cloudapp,dc=azure,dc=com<br>
displayName: Administrator<br>
cn: Administrator<br>
givenName: Administrator<br>
sn: None<br>
initials: ADM<br>
mail: admin@<br>
uid: admin<br>
uidNumber: 50000<br>
gidNumber: 10000<br>
homeDirectory: /home/admin<br>
loginShell: /bin/bash<br>
gecos: Administrator,,,,<br>
objectClass: inetOrgPerson<br>
objectClass: posixAccount<br>
objectClass: shadowAccount<br>
userPassword:: e1NTSEF9U21OekxlWTFZcFY4dVd1UlVldE5KYVplQi9FUnQxTWw=<br>
<br>
-# user1, people, southcentralus.cloudapp.azure.com<br>
dn: uid=user1,ou=people,dc=southcentralus,dc=cloudapp,dc=azure,dc=com<br>
displayName: User 1<br>
cn: User 1<br>
givenName: User<br>
sn: One<br>
initials: US1<br>
mail: user1@<br>
uid: user1<br>
uidNumber: 50001<br>
gidNumber: 20000<br>
homeDirectory: /home/user1<br>
loginShell: /bin/bash<br>
gecos: User 1,,,,<br>
objectClass: inetOrgPerson<br>
objectClass: posixAccount<br>
objectClass: shadowAccount<br>
userPassword:: e1NTSEF9U1VRUnAzcGNZMGJjQ0tIdWFTQ1dpY3hMc1J0cUorbzI=<br>

-# user2, people, southcentralus.cloudapp.azure.com<br>
dn: uid=user2,ou=people,dc=southcentralus,dc=cloudapp,dc=azure,dc=com<br>
displayName: User 2<br>
cn: User 2<br>
givenName: User<br>
sn: Two<br>
initials: US2<br>
mail: user2@<br>
uid: user2<br>
uidNumber: 50002<br>
gidNumber: 20000<br>
homeDirectory: /home/user2<br>
loginShell: /bin/bash<br>
gecos: User 2,,,,<br>
objectClass: inetOrgPerson<br>
objectClass: posixAccount<br>
objectClass: shadowAccount<br>
userPassword:: e1NTSEF9aFB0VGdMR3lDZXhQMG92QkhiZW41ZXFMWGlRK3VMU1c=<br>

-# user3, people, southcentralus.cloudapp.azure.com<br>
dn: uid=user3,ou=people,dc=southcentralus,dc=cloudapp,dc=azure,dc=com<br>
displayName: User 3<br>
cn: User 3<br>
givenName: User<br>
sn: Three<br>
initials: US3<br>
mail: user3@<br>
uid: user3<br>
uidNumber: 50003<br>
gidNumber: 20000<br>
homeDirectory: /home/user3<br>
loginShell: /bin/bash<br>
gecos: User 3,,,,<br>
objectClass: inetOrgPerson<br>
objectClass: posixAccount<br>
objectClass: shadowAccount<br>
userPassword:: e1NTSEF9SGVkSUpwTVJyZjFQV3k1ajJkWmtxYzZhRUR3UlBWNjk=<br>
<br>
-# user4, people, southcentralus.cloudapp.azure.com<br>
dn: uid=user4,ou=people,dc=southcentralus,dc=cloudapp,dc=azure,dc=com<br>
displayName: User 4<br>
cn: User 4<br>
givenName: User<br>
sn: Four<br>
initials: US4<br>
mail: user4@<br>
uid: user4<br>
uidNumber: 50004<br>
gidNumber: 20000<br>
homeDirectory: /home/user4<br>
loginShell: /bin/bash<br>
gecos: User 4,,,,<br>
objectClass: inetOrgPerson<br>
objectClass: posixAccount<br>
objectClass: shadowAccount<br>
userPassword:: e1NTSEF9R0pENDViSExQbXlGTW5IczA0OW1KYlV0NndTd3FoSC8=<br>
<br>
-# search result<br>
search: 2<br>
result: 0 Success<br>
<br>
-# numResponses: 7<br>
-# numEntries: 6<br>
[raken@server ~]$ <br>
