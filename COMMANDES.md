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
| Commande | Description |
| :--- | :--- |
| `python3 -m http.server 8000` | Transforme le dossier actuel en serveur Web instantané. |
| `nc -v [IP] [PORT]` | **Netcat**. Se connecte manuellement à un port pour voir la bannière. |
