# Android

## Usefull working directory

``` text
/data/local/tmp/
```

## List applications (packages)

``` text
pm list packages
```

See `pm` help for a full list of usages.

## Location of apps data

``` text
/data/data/<package_name>
```

* `databases/`: here go the app's databases
* `lib/`: libraries and helpers for the app
* `files/`: other related files
* `shared_prefs/`: preferences and settings
* `cache/`: well, caches

Perhaps something in:

* `/data/app/<package_name>/`
* `/data/app/<package_name>-1/` or another number

## Launch an Android application in a VM using Anbox

Notes about a test done on December 28th, 2019 using an Ubuntu 16.04.6 Xenial LTS (Backbox).

Note: `adb` was already installed.

### Install Anbox

``` bash
snap install --devmode --beta anbox
```

### Install the kernel modules

``` bash
sudo add-apt-repository ppa:morphis/anbox-support
sudo apt update
sudo apt install linux-headers-generic anbox-modules-dkms
```

Enable the modules (or reboot)

``` bash
sudo modprobe ashmem_linux
sudo modprobe binder_linux
```

Install `adb` if needed:

``` bash
sudo apt install android-tools-adb
```

The device should now be visible from `adb` using:

``` bash
adb devices
```

Or maybe it is needed to launch the session manager? (don't remember)

``` bash
anbox session-manager
```

### Install the application

``` bash
adb install example.apk
```

### Launch application

``` bash
anbox launch --package=com.contoso.superapp --component=com.contoso.superapp.MainActivity
```

### Play with it

It is now possible to listen traffic on the `anbox0` network interface.

Using `adb`, `frida`, `iptables`, Burp Suite, etc. it is possible to do almost everything!

#### Set a proxy

Using `adb` with:

``` bash
adb shell settings put global http_proxy <ip>:<port>
```

see: https://stackoverflow.com/questions/31807559/undo-setting-proxy-via-settings-global-in-android/47476009#47476009

## Mettre en place frida sur un téléphone Android

### 1. S'assurer que le téléphone est rooté

et si c'est le cas, ne pas oublier d'activer l'option "accès root" dans les options développeurs

### 2. Vérifier que adb fonctionne

Il est possible de connecter le téléphone en USB ou bien d'activer le débogage par réseau.
TODO : expliquer la partie pour activer le débogage par réseau à l'aide de la commande `adb tcpip 4444`
Note : avec Virtualbox et la kali en VM, les options usb sont restés en mode USB 2.0 et le cable utilisé était un câble USB 2.0 malgré qu'il soit sur un port USB 3.0.

### 3. Démarrer adb en mode root

sinon il n'est pas possible d'accéder aux autres applications : `adb root`

### 4. Envoyer et démarrer le démon frida

* Le récupérer sur [https://github.com/frida/frida/releases](https://github.com/frida/frida/releases) et prendre la version `frida-server-x.x.x-android-arm.xz` ou `frida-server-x.x.x-android-arm64.xz` en fonction de l'architecture du téléphone (ou même les versions x86 si besoin).
* L'envoyer sur le téléphone : `adb push frida-server-x.x.x-android-arm.xz /data/local/tmp/frida-server`
* Le lancer en root `adb shell /data/local/tmp/frida-server` (pour s'assurer que le shell adb a bien les permissions root, il est possible de faire `adb shell id`)

### 5. Installer le client sur kali

``` bash
sudo apt update
sudo apt install android-tools-adb android-tools-fastboot
sudo usermod -a -G plugdev joe # si l'utilisateur n'appartient pas encore au groupe plugdev
sudo su - joe # pour recharger les permissions
```

### 6. Profit

Il est maintenant possible de :

* lister les devices : `frida-ls-devices`
* lister tous les processus : `frida-ps -U`
* lister les applications : `frida-ps -Ua`
* etc…

(Le `-U` est utilisé car le téléphone est branché en USB)

## Outrepasser le SSL pinning dans une application Android

### 0. 0b10 façons de faire

* Nativement, on liste les applications qui tournent (`frida-ps -Ua`) puis on s'y attache (`frida-trace -U com.example.app`).
Mais parfois ça ne fonctionne pas, et ça necessite de lancer frida en root (pas toujours possible).

* Via frida-gadget. Cela demande d'insérer le "Gadget" dans l'application mais permet de l'utiliser sans root.
C'est cette méthode qui sera décrite ici.

### 1. S'assurer que frida est fonctionnel

Voir plus haut

### 2. Décompresser l'application

```bash
apktool d example.apk -o apktool_output
```

### 3. Insérer le gadget

* Télécharger le gadget adéquat : [https://github.com/frida/frida/releases](https://github.com/frida/frida/releases) et prendre la version `frida-gadget-12.0.8-android-arm64.so.xz` ou `frida-gadget-12.0.8-android-arm64.so.xz`.
