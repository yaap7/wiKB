# rsync

## Synchronization through SSH

``` bash
rsync -rv -e ssh ~/.ssh server:~/
```

## Local synchronization

To sync two directories: add trailling slash to both paths.

``` bash
rsync -avh --delete data/ /media/user/external_drive/data/
```
