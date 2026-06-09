# Liens symboliques et liens physiques

## Lien physique (hard link)

Un lien physique est un **deuxième nom** pour le même fichier. Les deux noms pointent vers les mêmes données sur le disque.

```bash
ln fichier.txt lien-physique.txt
```

- Modifier l'un modifie l'autre (c'est le même fichier)
- Supprimer l'un ne supprime pas les données — elles restent accessibles via l'autre nom
- Ne fonctionne pas entre deux systèmes de fichiers différents
- Ne fonctionne pas sur les dossiers

---

## Lien symbolique (symlink)

Un lien symbolique est un **raccourci** qui pointe vers un autre fichier ou dossier. C'est une référence, pas une copie.

```bash
ln -s /chemin/source /chemin/lien
```

> `-s` = symbolique

Exemple concret — faire pointer `/var/www/monsite` vers ton projet VS Code :

```bash
sudo ln -s /home/administrateur/VScode/Training/Apache-training /var/www/monsite
```

Vérifier qu'il existe :

```bash
ls -la /var/www/monsite
# lrwxrwxrwx ... /var/www/monsite -> /home/administrateur/VScode/Training/Apache-training
```

> `l` en début de ligne = lien symbolique
> `->` = pointe vers

---

## Différences clés

| | Lien physique | Lien symbolique |
| --- | --- | --- |
| Pointe vers | Les données directement | Un chemin |
| Si la source est supprimée | Les données restent | Le lien est cassé |
| Fonctionne sur les dossiers | Non | Oui |
| Entre systèmes de fichiers | Non | Oui |
| Commande | `ln` | `ln -s` |

---

## Utilisation avec Apache

Le lien symbolique est utile pour faire pointer le `DocumentRoot` d'Apache vers ton dossier de projet, sans déplacer les fichiers :

```bash
sudo rm -r /var/www/monsite
sudo ln -s /home/administrateur/VScode/Training/Apache-training /var/www/monsite
```

Apache lit `/var/www/monsite` (qu'il a le droit d'ouvrir) et suit le lien vers ton projet. Tu modifies dans VS Code, les changements sont visibles immédiatement dans le navigateur.

> Pour qu'Apache suive les liens symboliques, la directive `FollowSymLinks` doit être présente dans le bloc `<Directory>` du VirtualHost.

```apache
<Directory /var/www/monsite>
    Options Indexes FollowSymLinks
    ...
</Directory>
```
