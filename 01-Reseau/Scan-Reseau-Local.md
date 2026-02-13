# Lab 01 : Découverte réseau et premier contact (Nmap/Netcat)

**Date :** 13 Février 2026
**Environnement :** Kali Linux (VMware)

## Contexte
C'est mon premier laboratoire pratique. L'objectif est de comprendre l'environnement réseau dans lequel tourne ma machine virtuelle Kali et d'identifier les autres machines présentes, notamment mon PC hôte physique.

---

### Étape 1 : Cartographie du réseau 
Pour commencer, j'ai cherché mon propre IP avec `ip a` (je suis en `.128`). Ensuite, j'ai utilisé Nmap pour faire un "Ping Sweep" sur tout mon sous-réseau. L'option `-sn` permet de juste vérifier qui est en ligne sans lancer d'analyse lourde.

**Commande :** `sudo nmap -sn 192.168.81.0/24`

**Résultat :**
J'ai découvert 4 machines actives. L'IP `192.168.81.1` semble être ma passerelle vers mon PC physique.

<img width="530" height="302" alt="screennmap1" src="https://github.com/user-attachments/assets/a121bc1e-fd7d-4c22-9924-7f9f480b7673" />


### Étape 2 : Scan de la cible (Le PC Hôte)
J'ai décidé de scanner plus précisément l'IP `.1` (mon vrai PC). J'ai utilisé l'option `-Pn` car les pare-feux Windows bloquent souvent les pings classiques, ce qui fausse les résultats.

**Commande :** `sudo nmap -Pn 192.168.81.1`

**Résultat :**
Le scan est très intéressant : 999 ports sont "filtrés" (le pare-feu fait bien son travail), mais **un port est ouvert : le 3306 (MySQL)**.

<img width="529" height="188" alt="screennmap2" src="https://github.com/user-attachments/assets/b832cc19-384d-4a2c-8bd4-a3cad61058a3" />

### Étape 3 : Interaction avec le service 
Pour confirmer que le port 3306 n'était pas un faux positif, j'ai utilisé Netcat pour tenter une connexion brute et voir si le serveur répondait.

**Commande :** `nc -v 192.168.81.1 3306`

**Observation :**
La connexion s'établit (je l'ai aussi vérifié sur Wireshark avec le TCP Handshake), mais le serveur MySQL me rejette immédiatement avec un message d'erreur explicite : mon IP n'est pas dans sa whitelist.

### Note personnelle sur l'apprentissage
Ce lab m'a fait réaliser l'importance du pare-feu. Voir le résultat "999 filtered" est rassurant pour la sécurité de mon PC principal. J'ai aussi compris la différence entre un port "fermé" (le PC répond "non") et un port "filtré" (le pare-feu ignore la demande). Le service MySQL est exposé, mais sa propre configuration interne bloque l'intrusion pour l'instant.
