# Apache sur Ubuntu — Sommaire

Ce guide t'explique comment installer, configurer et gérer Apache sur Ubuntu.

---

## Minimum fonctionnel

1. [C'est quoi Apache ?](docs/presentation.md)
2. [Installation d'Apache](docs/installation.md)
3. [Les commandes de base pour gérer Apache](docs/commandes.md)
4. [Où sont les fichiers ?](docs/fichiers.md)
5. [Mettre ta propre page web](docs/page-web.md)
6. [Configurer un serveur de base](docs/configuration-serveur.md)
7. [Liens symboliques et liens physiques](docs/liens.md)
8. [Permissions](docs/permissions.md)

---

## Aller plus loin

### Site statique (HTML/CSS)

- Modifier `/etc/hosts` pour accéder via `monsite.local` au lieu de `localhost:8080`

### Sécuriser le serveur

- Configurer HTTPS avec un certificat SSL (`mod_ssl` est déjà activé)

### Site dynamique

- Installer et configurer PHP avec Apache (`mod_php` ou `php-fpm`)
- Installer une base de données (MySQL ou MariaDB)

---

## Référence

1. [Désinstaller Apache](docs/desinstallation.md)
2. [Récap rapide](docs/recap.md)
