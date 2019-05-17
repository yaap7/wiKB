## ncat

Options :
* `-m <n>` : Maximum `n` connexions simultanées
* `-k` : Accepte plusieurs connexions en mode écoute
* `-l` : Mode écoute

Lancer un mini IRC chiffré :
`openssl genrsa 8192 > ncat.key && openssl req -new -x509 -days 365 -key ncat.key -out ncat.crt && ncat -k -m 3 --chat -l 54321 --ssl --ssl-cert ncat.crt --ssl-key ncat.key`

Redirige les data d'un port local vers un port sur un serveur distant :
`ncat -l 4444 -k -m 3 -c "ncat 192.168.1.12 443"`




