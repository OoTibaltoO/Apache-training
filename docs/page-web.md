# Mettre ta propre page web

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

## Trouver le fichier index.html

Pour localiser le fichier `index.html` d'Apache sur le système :

```bash
find /var/www/html -name "index.html"
```

> `find` = outil de recherche de fichiers
> `/var/www/html` = répertoire de départ (racine web Apache)
> `-name "index.html"` = filtre par nom de fichier

Le fichier se trouve à `/var/www/html/index.html`.
