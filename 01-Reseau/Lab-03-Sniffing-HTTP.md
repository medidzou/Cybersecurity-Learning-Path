# Lab 03 : Analyse de Trafic avec Wireshark

**Date :** 14 Février 2026
**Sujet :** Interception de mot de passe (Sniffing HTTP)

## 1. Contexte
J'ai voulu vérifier pourquoi le protocole HTTP (Port 80) est considéré comme non sécurisé. Pour cela, j'ai utilisé **Wireshark** pour écouter le réseau local et voir si je pouvais lire les données en clair.

## 2. La Manipulation
1.  **Serveur :** J'ai lancé un serveur web Python sur le port 8000 (`python3 -m http.server 8000`).
2.  **Victime :** J'ai simulé une connexion avec un faux mot de passe dans l'URL.
3.  **Attaquant :** J'ai configuré Wireshark sur l'interface de boucle locale (`Loopback: lo`) pour intercepter le trafic HTTP.

## 3. La Preuve (Flagrant délit)
Voici ce que Wireshark a capturé. On voit clairement la requête **GET** qui contient le login et le mot de passe en toutes lettres.

<img width="1502" height="152" alt="image" src="https://github.com/user-attachments/assets/1a5088f8-832b-4739-8431-1dab05e21419" />



## 4. Conclusion
Le protocole HTTP n'est pas chiffré. N'importe qui sur le même réseau peut utiliser un sniffer pour voler des identifiants. C'est pour cela qu'il faut toujours utiliser **HTTPS** (Port 443), qui rend ces données illisibles (chiffrées).
