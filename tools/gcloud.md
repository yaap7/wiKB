# gcloud

## Gestion des projets

Récupérer l'ID du projet courant :

``` bash
gcloud config get-value project
```

Changer le projet courant :

``` bash
gcloud config set project <PROJECT_ID>
```

⚠️ Il faut bien mettre l'ID du projet et non pas simplement le nom du projet.

## Cloud Build

TODO

## Cloud Source Repositories

<https://source.cloud.google.com/>

Créer un dépôt :

``` bash
gcloud source repos create hello-world
```

ℹ️ Cela va simplement le créer dans GCP, mais ne va rien faire dans le répertoire courant. Il faut ensuite le cloner pour l'utiliser.

Cloner un dépôt :

``` bash
gcloud source repos clone hello-world
```

## App Engine

Initialiser une nouvelle application App Engine :

``` bash
gcloud app create
```


## Cloud Storage

See `gsutil` (TODO).
