# Ma Cheat Sheet - Réseau & Nmap

## Découverte 
| Commande | Description | Exemple |
| :--- | :--- | :--- |
| `ip a` | Afficher ma propre adresse IP (Kali). | |
| `nmap -sn [IP/24]` | **Ping Scan**. Scanne tout le réseau pour voir qui est en ligne (sans scanner les ports). | `nmap -sn 192.168.1.0/24` |

## Scan de Ports
| Commande | Description | Exemple |
| :--- | :--- | :--- |
| `nmap [IP]` | Scan simple (1000 ports les plus courants). | `nmap 192.168.1.50` |
| `nmap -p- [IP]` | **Scan complet**. Vérifie les 65 535 ports (long mais précis). | `nmap -p- 192.168.1.50` |
| `nmap -p [PORT] [IP]` | Scan chirurgical. Vérifie un ou plusieurs ports précis. | `nmap -p 80,443 192.168.1.50` |
| `nmap -A [IP]` | **Scan Agressif**. Détecte l'OS et les versions des services. | `nmap -A scanme.nmap.org` |

## Options Utiles
| Option | Effet |
| :--- | :--- |
| `-v` | **Verbose**. Nmap parle en temps réel (ne reste pas muet). |
| `-Pn` | Force le scan même si la cible bloque le ping (utile pour Windows). |
| `-oN [fichier]` | **Sauvegarde**. Enregistre le résultat dans un fichier texte. |

## Outils Python & Netcat

| Commande | Description | Exemple |
| :--- | :--- | :--- |
| `python3 -m http.server 8000` | Transforme le dossier actuel en serveur Web instantané. | `python3 -m http.server 8000` |
| `nc -v [IP] [PORT]` | **Netcat**. Se connecte manuellement à un port pour voir la bannière. | `nc -v 127.0.0.1 22` |
| `nc -lvp [port]` | **Mode Serveur**. Crée un "Listener" qui attend une connexion. | `nc -lvp 4444` |
| `nc -lvp [port] \| hd` | Écoute et affiche les données en Hexadécimal. | `nc -lvp 4444 \| hd` |
| `nc -lvp [port] > [fichier]` | **Recevoir un fichier**. Écoute et enregistre tout ce qui arrive dans un fichier. | `nc -lvp 4444 > butin.txt` |
| `nc [IP] [port] < [fichier]` | **Envoyer un fichier**. Connecte et expédie le contenu d'un fichier. | `nc 192.168.1.50 4444 < secret.txt` |
| `python3 -c "print(''.join([chr(int(x)) for x in open('file.txt').read().split()]))"` | **Décodeur ASCII**. Convertit un fichier de nombres décimaux en texte. |`python3 -c "print(''.join([chr(int(x)) for x in open('capture.txt').read().split()]))"`|
| `awk '{for(i=1;i<=NF;i++) printf "%c", $i; print ""}' [file]` | **Décodeur Rapide**. Alternative Linux pour transformer le décimal en texte. | `awk ... capture.txt` |
| `strings [fichier]` | **Nettoyeur**. Extrait uniquement le texte lisible d'un fichier binaire. | `strings capture.txt` |


## Gestion des Services (SSH)

| Commande | Description | Exemple |
| :--- | :--- | :--- |
| `sudo systemctl status ssh` | Vérifie si le serveur SSH est allumé ou éteint. | `sudo systemctl status ssh` |
| `sudo systemctl start ssh` | Allume le service SSH pour autoriser les connexions. | `sudo systemctl start ssh` |
| `sudo systemctl stop ssh` | Éteint le service SSH (Sécurité après utilisation). | `sudo systemctl stop ssh` |
| `ssh [user]@[IP]` | Se connecte à une machine distante via un tunnel chiffré. | `ssh kali@127.0.0.1` |
| `ssh [user]@[IP] -p [port]` | **Connexion Port Spécifique**. Se connecte si le port n'est pas le 22. | `ssh ctf-player@titan.picoctf.net -p 58704` |
| `who` | Affiche les utilisateurs connectés et leur provenance. | `who` |
| `sudo journalctl -t sshd -n 20` | **Logs SSH**. Affiche les 20 dernières tentatives de connexion au serveur. | `sudo journalctl -t sshd -n 20` |

## Pare-feu (UFW)

| Commande | Description | Exemple |
| :--- | :--- | :--- |
| `sudo apt install ufw` | Installe le pare-feu (si non présent). | `sudo apt install ufw` |
| `sudo ufw status` | Vérifie si le pare-feu est actif et liste les règles. | `sudo ufw status` |
| `sudo ufw allow [service]` | Autorise le trafic entrant pour un service ou un port. | `sudo ufw allow ssh` |
| `sudo ufw enable` | Active le pare-feu (Attention à ne pas s'enfermer dehors !). | `sudo ufw enable` |
| `sudo ufw disable` | Désactive le pare-feu (Laisse tout passer). | `sudo ufw disable` |

## Force Brute & Wordlists

| Commande | Description | Exemple |
| :--- | :--- | :--- |
| `gunzip [fichier.gz]` | **Décompression**. Extrait les listes de mots (ex: RockYou). | `sudo gunzip /usr/share/wordlists/rockyou.txt.gz` |
| `echo -e "p1\np2" > [file]` | **Création de liste**. Crée un fichier avec un mot de passe par ligne. | `echo -e "123\nmotdepasse" > pass.txt` |
| `hydra -l [user] -P [file] ssh://[IP]` | **Attaque SSH**. Tente de forcer l'accès avec une liste de mots de passe. | `hydra -l kali -P pass.txt ssh://127.0.0.1` |

