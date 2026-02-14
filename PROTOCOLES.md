# Mes Protocoles de Base (Niveau 1)

Ce sont les seuls protocoles que j'ai rencontrés et manipulés pour l'instant.

## 1. Le Transport (Comment ça voyage ?)
| Nom | C'est quoi ? | Mon expérience |
| :--- | :--- | :--- |
| **TCP** | Le mode "Fiable". | C'est ce que Nmap utilise par défaut. La machine vérifie que le message est bien arrivé. C'est lent mais sûr. |

## 2. Les Services (Les portes que j'ai croisées)
| Port | Nom | À quoi ça sert ? | Où je l'ai vu ? |
| :--- | :--- | :--- | :--- |
| **80** | **HTTP** | Le Web normal (non sécurisé). | C'est le port qui s'est ouvert quand j'ai lancé ma commande Python (`python3 -m http.server`). |
| **3306** | **MySQL** | Une base de données (stockage d'infos). | C'est le port ouvert que j'ai découvert sur mon propre PC Windows en le scannant. |
| **22** | **SSH** | Prise de contrôle à distance (Ligne de commande). | (À venir) C'est le standard pour contrôler des serveurs Linux. |
|  | **ICMP**  |Sert à vérifier la connexion. | C'est ce qui s'est affiché dans **Wireshark** quand j'ai lancé la commande `ping 8.8.8.8` (Google). |
