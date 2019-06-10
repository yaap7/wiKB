# Dump réseau avec Wireshark en user

Pour capturer en tant qu'utilisateur avec wireshark il faut reconfigurer pour ajouter le groupe wireshark au système :
`sudo dpkg-reconfigure wireshark`
puis s'ajouter au groupe.

(si on veut pas relancer la session, un petit `sudo su - <username>` et le tour est joué.)
