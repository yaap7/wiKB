# Create a dm-crypt encrypted file

Utile pour créer un conteneur chiffré pour protéger les données PASSI par exemple.

## Création

Création d'un fichier de 3 Go formaté en ext4 :

``` bash
fallocate -l 3G conteneur.img
cryptsetup luksFormat conteneur.img
sudo cryptsetup luksOpen conteneur.img monImage
sudo mkfs.ext4 /dev/mapper/monImage
sudo cryptsetup luksClose monImage
```

## Utilisation

Montage :

``` bash
sudo cryptsetup luksOpen conteneur.img monImage
sudo mount /dev/mapper/monImage conteneur/
```

Démontage :

``` bash
sudo umount conteneur/
sudo cryptsetup luksClose monImage
```
