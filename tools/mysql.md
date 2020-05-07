# MySQL

## Connexion

Pour se connecter en root, il est parfois impossible depuis un simple compte utilisateur. Il faut tout d'abord utiliser le compte root (je crois que c'est à cause d'une "connexion par socket unix").

Par exemple, la commande suivante ne fonctionnera pas :

``` txt
user@server:~$ mysql -u root -p
```

Tandis que ces 2 exemples sont valides :

``` txt
user@server:~$ sudo mysql -u root
```

``` txt
root@server:~# mysql -u root
```

## Gestion des droits

### Créer un utilisateur et une base de données dédiée

Ici, le compte `munin` aura tous les droits sur la base de données `munin` et uniquement celle-ci.

``` mysql
create database munin;
create user 'munin'@'localhost' identified by 'P@$$w0rd';
grant all privileges on munin.* to 'munin'@'localhost';
```

Puis, pour supprimer tout ce qui a été ajouté :

``` mysql
revoke all privileges, grant option from 'munin'@'localhost';
# TODO suppression du user
drop database munin;
```

### Affichage des utilisateurs, droits, etc

Voir les utilisateurs, leur hash et leur hôte de connexion autorisé :

``` mysql
select host, user, password from mysql.user;
```

`show grants;` présente les droits de l'utilisateur courant par défaut.
Il est possible de voir les droits des autres utilisateurs à l'aide de :

``` mysql
show grants for 'munin'@'localhost';
```

Voir les bases de données, puis les tables d'une base en particulier :

``` mysql
show databases;
use mysql;
show tables;
```

## Sauvegarde et restauration

### Sauvegarde avec mysqldump

``` bash
mysqldump ---user [user name] ---password=[password] [database name] > [dump_file.sql]
```

Ou :

``` bash
mysqldump ---user [user name] ---password=[password] [database name] | gzip -9 > [dump_file.sql.gz]
```

### Restauration avec mysql

``` bash
# optionnal
gunzip dump_file.sql.gz
mysql -u munin -p muninDB < dump_file.sql
```
