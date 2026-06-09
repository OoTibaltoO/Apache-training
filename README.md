# Apache sur Ubuntu

Ce guide t'explique comment installer et lancer Apache sur Ubuntu, étape par étape.

---

## C'est quoi Apache ?

Apache est un **serveur web**. Concrètement, c'est un programme qui tourne sur ta machine et qui répond aux requêtes HTTP — comme un navigateur qui demande une page web. Quand tu visites un site, c'est souvent Apache (ou un équivalent) qui envoie la page.

---

## 1. Mettre à jour le système avant d'installer

Avant d'installer quoi que ce soit, mets à jour la liste des paquets disponibles :

```bash
sudo apt update
```

> `sudo` = "je veux faire ça en tant qu'administrateur"
> `apt` = le gestionnaire de paquets d'Ubuntu (comme un App Store en ligne de commande)
> `update` = rafraîchit la liste des logiciels disponibles (ça n'installe rien encore)

---

## 2. Installer Apache

```bash
sudo apt install apache2 -y
```

> `install apache2` = installe le paquet Apache
> `-y` = répond "oui" automatiquement aux confirmations

L'installation prend quelques secondes. Une fois terminée, Apache est installé.

---

## 3. Vérifier qu'Apache tourne

```bash
sudo systemctl status apache2
```

Tu dois voir quelque chose comme :

```text
● apache2.service - The Apache HTTP Server
     Active: active (running) ...
```

La ligne `active (running)` en vert = Apache est lancé et fonctionne.

> `systemctl` = l'outil qui gère les services (programmes qui tournent en arrière-plan) sur Ubuntu

---

## 4. Tester dans le navigateur

Ouvre ton navigateur et va sur :

```text
http://localhost
```

Tu devrais voir la page par défaut d'Apache avec le message **"It works!"** (ou "Apache2 Ubuntu Default Page").

![alt text](<it works.png>)

Si tu vois cette page, Apache fonctionne correctement.

---

## 5. Les commandes de base pour gérer Apache

| Action | Commande |
| --- | --- |
| Démarrer Apache | `sudo systemctl start apache2` |
| Arrêter Apache | `sudo systemctl stop apache2` |
| Redémarrer Apache | `sudo systemctl restart apache2` |
| Voir le statut | `sudo systemctl status apache2` |
| Lancer au démarrage | `sudo systemctl enable apache2` |
| Ne plus lancer au démarrage | `sudo systemctl disable apache2` |

---

## 6. Où sont les fichiers ?

| Dossier / Fichier | Rôle |
| --- | --- |
| `/var/www/html/` | Dossier racine du site web (mets tes fichiers HTML ici) |
| `/etc/apache2/` | Dossier de configuration d'Apache |
| `/etc/apache2/apache2.conf` | Fichier de configuration principal |
| `/var/log/apache2/access.log` | Journal des requêtes reçues |
| `/var/log/apache2/error.log` | Journal des erreurs |

---

## 7. Mettre ta propre page web

Par défaut, Apache sert le fichier `/var/www/html/index.html`. Tu peux le modifier de trois façons :

**Option 1 — Avec nano (dans le terminal) :**

```bash
sudo nano /var/www/html/index.html
```

Écris ton HTML, sauvegarde avec `Ctrl+O` puis `Entrée`, et quitte avec `Ctrl+X`.

> `nano` = éditeur de texte simple directement dans le terminal

**Option 2 — Avec VS Code :**

Les fichiers dans `/var/www/html/` appartiennent à `root`, VS Code ne peut pas les modifier directement. La solution : copie le fichier, modifie-le, puis remplace-le.

```bash
cp /var/www/html/index.html ~/index.html
# modifie ~/index.html dans VS Code
sudo cp ~/index.html /var/www/html/index.html
```

> `cp` = copie un fichier
> `~/` = ton dossier personnel (ex. `/home/ton-user/`)

Pour copier la page Apache dans le dossier de projet courant (pour la garder en référence) :

```bash
cp /var/www/html/index.html .
```

> `.` = répertoire courant (là où tu te trouves dans le terminal)

**Option 3 — Remplacer entièrement avec une commande rapide :**

```bash
echo "<h1>Ma page</h1>" | sudo tee /var/www/html/index.html
```

> `echo` = affiche du texte
> `|` = envoie le résultat à la commande suivante
> `tee` = écrit dans un fichier (avec `sudo` pour les fichiers protégés)

---

Recharge `http://localhost` dans le navigateur — ta page s'affiche immédiatement, sans redémarrer Apache.

---

## 8. Trouver le fichier index.html

Pour localiser le fichier `index.html` d'Apache sur le système :

```bash
find /var/www/html -name "index.html"
```

> `find` = outil de recherche de fichiers
> `/var/www/html` = répertoire de départ (racine web Apache)
> `-name "index.html"` = filtre par nom de fichier

Le fichier se trouve à `/var/www/html/index.html`.

---

## 9. Désinstaller Apache (si besoin)

```bash
sudo apt remove apache2 -y
sudo apt autoremove -y
```

> `autoremove` = supprime les paquets installés automatiquement qui ne sont plus utiles

---

## Récap rapide

```bash
sudo apt update                    # 1. Mettre à jour la liste des paquets
sudo apt install apache2 -y        # 2. Installer Apache
sudo systemctl status apache2      # 3. Vérifier qu'il tourne
# Ouvrir http://localhost           # 4. Tester dans le navigateur
```

C'est tout — Apache est installé et opérationnel.
