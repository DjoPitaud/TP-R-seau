# TP1 : Les premiers pas de bébé B1

Dans ce premier TP de réseau, on va partir de ce que vous avez l'habitude de manipuler : votre PC.

On commence très doucement avec au menu, en vrac :

- des **commandes dans le terminal**
- faire joujou avec **les cartes réseau** de votre PC
- commencer à appréhender comment votre PC interagit avec le réseau
- se familiariser peu à peu avec des outils usuels du réseau, comme le `ping` et Wireshark

![Baby](./img/baby.png)

## Sommaire

- [TP1 : Les premiers pas de bébé B1](#tp1--les-premiers-pas-de-bébé-b1)
  - [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
- [I. Récolte d'informations](#i-récolte-dinformations)
- [II. Utiliser le réseau](#ii-utiliser-le-réseau)
- [III. Sniffer le réseau](#iii-sniffer-le-réseau)
- [IV. Network scanning et adresses IP](#iv-network-scanning-et-adresses-ip)

# 0. Prérequis

- ton PC
- connecté à un réseau local (celui de l'école, celui de chez vous, peu importe)
- ce réseau local permet un accès internet
- tu sais ouvrir un terminal sur ton PC

> *Petite précision pour les Windowsiens : le terminal s'appelle Powershell (n'ouvrez pas l'antiquité qu'est CMD ou "Invite de commandes", c'était en 2002 ça).*

# I. Récolte d'informations

> Pour rappel, tout est à faire depuis un terminal, sauf si je mentionne explicitement le contraire.

🌞 **Adresses IP de ta machine**

- J'utilise la commande `ipconfig /all`
- Adresse IP que ta machine a sur sa carte réseau WiFi: 10.33.73.247
- Adresse IP que ta machine a sur sa carte réseau ethernet: 172.25.64.1


🌞 **Si t'as un accès internet normal, d'autres infos sont forcément dispos...**

- Adresse IP de la passerelle du réseau local: 10.33.79.254

- Serveur DNS:  8.8.8.8
- Aresse IP du serveur DHCP: 10.33.79.254


🌟 **BONUS** : Détermine s'il y a un pare-feu actif sur ta machine

- J'utilise la commande `Get-NetFirewallProfile`
- Le pare-feu est présent et actif 
- Règles du pare-feu: 
`$rules=Get-NetFirewallRule`

`$DisplayGroups=foreach ($rule in $rules){$rule.displaygroup}` 
`$DisplayGroups | Select-Object Unique`


# II. Utiliser le réseau


🌞 **Envoie un `ping` vers...**

- **toi-même !**
  - >PS C:\Users\pitau> ping 10.33.73.247
Envoi d’une requête 'Ping'  10.33.73.247 avec 32 octets de données :
Réponse de 10.33.73.247 : octets=32 temps<1ms TTL=128
Réponse de 10.33.73.247 : octets=32 temps<1ms TTL=128
Réponse de 10.33.73.247 : octets=32 temps<1ms TTL=128
Réponse de 10.33.73.247 : octets=32 temps<1ms TTL=128
Statistiques Ping pour 10.33.73.247:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms

  
- **vers l'adresse IP `127.0.0.1`**
    - >PS C:\Users\pitau> ping 127.0.0.1
Envoi d’une requête 'Ping'  127.0.0.1 avec 32 octets de données :
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Réponse de 127.0.0.1 : octets=32 temps<1ms TTL=128
Statistiques Ping pour 127.0.0.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms

🌞 **On continue avec `ping`.** Envoie un `ping` vers...

- **ta passerelle**
  - >PS C:\Users\pitau> ping 10.33.79.254
Envoi d’une requête 'Ping'  10.33.79.254 avec 32 octets de données :
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Délai d’attente de la demande dépassé.
Statistiques Ping pour 10.33.79.254:
    Paquets : envoyés = 4, reçus = 0, perdus = 4 (perte 100%)

- **un(e) pote sur le réseau**
  - >Envoi d’une requête 'Ping'  10.33.72.215 avec 32 octets de données :
Réponse de 10.33.72.215 : octets=32 temps=7 ms TTL=128
Réponse de 10.33.72.215 : octets=32 temps=8 ms TTL=128
Réponse de 10.33.72.215 : octets=32 temps=11 ms TTL=128
Réponse de 10.33.72.215 : octets=32 temps=9 ms TTL=128
  Statistiques Ping pour 10.33.72.215:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 7ms, Maximum = 11ms, Moyenne = 8ms
- **un site internet**
  - >Envoi d’une requête 'ping' sur www.thinkerview.com [188.114.96.7] avec 32 octets de données :
Réponse de 188.114.96.7 : octets=32 temps=15 ms TTL=55
Réponse de 188.114.96.7 : octets=32 temps=15 ms TTL=55
Réponse de 188.114.96.7 : octets=32 temps=17 ms TTL=55
Réponse de 188.114.96.7 : octets=32 temps=19 ms TTL=55
Statistiques Ping pour 188.114.96.7:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 15ms, Maximum = 19ms, Moyenne = 16ms

---

Quand tu visites un site web comme `www.thinkerview.com`, ton PC va spontanément chercher à connaître l'adresse IP qui est associée au nom `www.thinkerview.com`. Une fois l'adresse IP obtenue, ton PC pourra établir une connexion vers cette adresse IP.

**Autrement dit, le nom n'est qu'un pointeur vers une adresse IP.**

Pour connaître à quelle adresse IP est associée un nom donné, une machine (comme ton PC) peut envoyer une **requête DNS** (*DNS lookup* en anglais) à un serveur DNS qu'elle connaît.

> Vous avez repéré dans la partie I que votre PC connaît déjà l'adresse d'un serveur DNS.

🌞 **Faire une requête DNS à la main**

- 
  - `nslookup www.thinkerview.com`
  - `nslookup www.wikileaks.org`
  - `nslookup www.torproject.org`
  

> J'ai choisi des noms de domaine complètement au hasard ~~ou pas~~. Vous remarquez que `https://` ne fait PAS partie du nom de domaine.

# III. Sniffer le réseau

![Wireshark](./img/wireshark.jpg)

> WTF keskidi sniffer le réseau. "Produire une analyse passive du trafic réseau" c'est plus joli formulé comme ça ? En anglais (et par abus de langage, bah en français aussi) on dit "network sniffing" ;)

Il existe des outils qui permettent de visualiser tous les paquets qui entrent et sortent d'une interface réseau.

Dans notre cours, on utilisera l'outil de référence : Wireshark.

Wireshark va afficher chaque paquet qui rentre et sort de l'interface réseau que vous ciblez.

➜ **Refais les commandes `ping` de la partie précédente**

- mais avec Wireshark ouvert
- tu peux saisir "icmp" dans la barre du haut pour filtrer le trafic et n'afficher que les paquets ICMP
- `ping` et `pong` sont des paquets de type ICMP ;)
- tu devrais **voir** les pings et les pongs passer
- pour chaque message, tu devrais voir
  - le type du paquet (ICMP pour le ping)
  - plus spécifiquement, si c'est...
    - un `ping` : tu verras `echo request` ou `ICMP type 8`
    - un `pong` : pour lui c'est `echo reply` ou `ICMP type 0`
  - l'adresse IP source (l'émetteur du message)
  - l'adresse IP de destination (le destinataire du message)
- fais l'effort d'identifier chaque ping envoyé, et chaque pong correspondant

> Hé, fais l'effort maintenant, parce que là c'est le niveau bac à sable. **N'hésitez pas à m'appeler s'il faut que je vous aide pour vos premiers pas avec Wireshark.**

🌞 **J'attends dans le dépôt git de rendu un fichier `ping.pcap`**

[Capture ping](ping.pcap)

- c'est une capture Wireshark
- elle ne doit contenir que les paquets demandés, absolument aucun autre

🌞 **Livrez un deuxième fichier : `dns.pcap`**

[Capture dns](dns.pcap)

- il contient une capture des 3 requêtes DNS demandées plus haut
- avec les réponses DNS associées

# IV. Network scanning et adresses IP

Le but de cette dernière partie est simple : **faire un scan réseau pour découvrir les autres machines connectées au même réseau que nous**.

Une fois que vous aurez la liste des adresses IP des autres machines du réseau, vous changerez à la main votre adresse IP, vous en choisirez une que vous savez libre (personne ne l'utilise) suite à votre scan.

L'idée est de vous forcer à **changer d'adresse IP manuellement**, sans pour autant perdre l'accès au réseau local et à internet.

> Vous êtes à tout moment libres de changer d'adresse IP sur votre PC ! Chaque PC/machine choisit sa propre adresse IP, c'est une règle élémentaire du réseau.

➜ **Commence par télécharger `nmap`**

- je vous laisse aller sur internet pour ça
- [le site officiel est là](https://nmap.org) (oui il est pas beau, et alors, on juge pas sur le physique :d)
- c'est un outil très réputé qu'on peut utiliser en ligne de commande pour faire du scan réseau

🌞 **Effectue un scan du réseau auquel tu es connecté**

- avec une seule commande `nmap`
- je vous recommande de faire un scan comme ça : `nmap -sn -PR <NETWORK_ADDRESS>`
- si vous ne savez pas quoi écrire comme `<NETWORK_ADDRESS>` ou êtes un peu perdus, appelez-moi !

- `nmap.exe -sn -PR 10.33.64.0/20` 
L'adresse du réseau se déduit grâce à son ip et son masque et le cacule en binaire.

🌞 **Changer d'adresse IP**

- Après avoir repéré une adresse IP non utilisé sur le réseau, j'ai changé mon adresse IP sur l'interface graphique de windows. Une fois celle-ci changé j'ai dans un premier temps vérifié avec la commande `ipconfig` puis testé de ping un site avec `ping www.thinkerview.com`
