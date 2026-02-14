# Lab 04 : Analyse de Trafic avec Wireshark (SSH)

**Date :** 14 Février 2026
**Sujet :** Chiffrement des données (Protocole SSH)

## 1. Contexte
Après avoir vu que le protocole HTTP (Port 80) est vulnérable car il transmet les données en clair, j'ai voulu tester le protocole **SSH (Port 22)**. L'objectif est de vérifier si le chiffrement rend effectivement les informations (comme le mot de passe) illisibles pour un attaquant utilisant un sniffer.

## 2. La Manipulation
1.  **Serveur :** J'ai activé le service SSH sur ma machine Kali Linux (`sudo systemctl start ssh`).

2.  **Client :** J'ai simulé une connexion sécurisée vers ma propre machine (`ssh kali@127.0.0.1`).

3.  **Attaquant :** J'ai configuré Wireshark sur l'interface de boucle locale (`Loopback: lo`) pour intercepter le trafic.

## 3. La Preuve (Chiffrement en action)
Voici ce que Wireshark a capturé. Contrairement au Lab 03, on ne peut plus lire le mot de passe. Le protocole indiqué est **SSHv2** et les données sont marquées comme chiffrées.

<img width="1504" height="185" alt="image" src="https://github.com/user-attachments/assets/d999c330-92e4-4e1b-bbea-0d8861542776" />



## 4. Conclusion
Le protocole SSH remplit son rôle de sécurité grâce au chiffrement. Même si un attaquant intercepte les paquets sur le réseau, il ne peut rien exploiter car les données sont illisibles. C'est pourquoi il faut privilégier SSH pour l'administration à distance.
