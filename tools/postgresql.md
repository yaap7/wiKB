# MySQL

## Connexion

``` txt
user@server:~$ sudo -u postgres psql
```

## Gestion des droits

### Créer un utilisateur et une base de données dédiée

Ici, le compte `youruser` aura tous les droits sur la base de données `yourdbname` et uniquement celle-ci.

``` postgresql
CREATE DATABASE yourdbname;
CREATE USER youruser WITH ENCRYPTED PASSWORD 'yourpass';
GRANT ALL PRIVILEGES ON DATABASE yourdbname TO youruser;
```

Puis, pour supprimer tout ce qui a été ajouté :

``` postgresql
drop database yourdbname;
drop user youruser;
```

Pour changer le mot de passe d'un utilisateur :

``` sql
ALTER USER youruser WITH PASSWORD 'yourpass';
```

### Affiche l'algorithme de hashage des mots de passe

``` sql
SHOW password_encryption;
```

[Source](https://documentation.tricentis.com/qtest/10400/en/content/qtest_onpremise/server_administration/change_postgresql_password_authentication_scram-sha-256.htm)
