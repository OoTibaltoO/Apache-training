# Permissions

## Pourquoi Apache a des problèmes de permissions ?

Apache tourne avec l'utilisateur `www-data`. Par défaut, cet utilisateur n'a pas le droit de lire les dossiers dans `/home/`. C'est pour ça qu'on obtient une erreur **Forbidden** quand le `DocumentRoot` pointe vers un dossier personnel.

---

## Donner accès à www-data sur ton dossier home

La solution la plus propre est d'ajouter `www-data` au groupe de ton utilisateur et de donner au groupe le droit de traverser ton dossier home.

```bash
sudo usermod -aG administrateur www-data
sudo chmod g+x /home/administrateur
sudo systemctl restart apache2
```

> `usermod -aG` = ajoute `www-data` au groupe `administrateur`
> `chmod g+x` = donne au groupe le droit de traverser le dossier (sans exposer son contenu)

C'est plus ciblé que `chmod o+x` — seul `www-data` gagne l'accès, pas tous les autres utilisateurs de la machine.

---

## Les trois alternatives

| Méthode | Sécurité | Usage |
| --- | --- | --- |
| `chmod o+x /home/user` | Faible | Dev local uniquement |
| `usermod -aG user www-data` + `chmod g+x` | Bonne | Dev local propre |
| Garder le `DocumentRoot` dans `/var/www/` | Optimale | Production |

---

## Vérifier les permissions d'un dossier

```bash
ls -la /home/administrateur
```

La colonne de gauche indique les permissions :

```text
drwxr-x--x  ...  administrateur  administrateur  /home/administrateur
```

> `d` = dossier
> `rwx` = droits du propriétaire (read, write, execute)
> `r-x` = droits du groupe
> `--x` = droits des autres (execute uniquement = traverser sans lire)

---

## Vérifier que www-data appartient bien au groupe

```bash
groups www-data
```

Tu dois voir `administrateur` dans la liste des groupes.
