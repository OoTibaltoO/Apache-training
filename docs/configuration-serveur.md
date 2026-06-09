# Configurer un serveur de base avec Apache

## Les fichiers de configuration clés

| Fichier | Rôle |
| --- | --- |
| `/etc/apache2/apache2.conf` | Configuration principale |
| `/etc/apache2/ports.conf` | Ports d'écoute |
| `/etc/apache2/sites-available/` | VirtualHosts disponibles |
| `/etc/apache2/sites-enabled/` | VirtualHosts actifs (liens symboliques) |
| `/etc/apache2/mods-available/` | Modules disponibles |
| `/etc/apache2/mods-enabled/` | Modules activés |

---

## Créer un VirtualHost

Un VirtualHost permet de servir un site avec son propre domaine, dossier et configuration.

### 1. Créer le dossier racine du site

```bash
sudo mkdir -p /var/www/monsite
sudo chown -R $USER:$USER /var/www/monsite
```

> `chown` = change le propriétaire du dossier pour ton utilisateur courant

### 2. Créer le fichier de configuration

```bash
sudo nano /etc/apache2/sites-available/monsite.conf
```

Contenu minimal :

```apache
<VirtualHost *:80>
    ServerName monsite.local
    DocumentRoot /var/www/monsite

    <Directory /var/www/monsite>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/monsite_error.log
    CustomLog ${APACHE_LOG_DIR}/monsite_access.log combined
</VirtualHost>
```

> `ServerName` = domaine ou IP du serveur
> `DocumentRoot` = dossier racine des fichiers servis
> `AllowOverride All` = autorise les fichiers `.htaccess`
> `ErrorLog` / `CustomLog` = chemins des fichiers de log

### 3. Activer le site

```bash
sudo a2ensite monsite.conf
```

> `a2ensite` = crée un lien symbolique dans `sites-enabled/`

### 4. Désactiver le site par défaut (optionnel)

```bash
sudo a2dissite 000-default.conf
```

### 5. Tester la configuration

```bash
sudo apache2ctl configtest
```

Tu dois voir `Syntax OK` avant de redémarrer.

### 6. Redémarrer Apache

```bash
sudo systemctl restart apache2
```

---

## Activer des modules

Apache est modulaire. Certaines fonctionnalités nécessitent d'activer un module.

```bash
sudo a2enmod rewrite      # mod_rewrite (URLs propres, .htaccess)
sudo a2enmod ssl          # HTTPS
sudo a2enmod headers      # Manipulation des en-têtes HTTP
sudo systemctl restart apache2
```

> `a2enmod` = active un module
> `a2dismod` = désactive un module

---

## Configurer les ports d'écoute

Le fichier `/etc/apache2/ports.conf` définit sur quels ports Apache écoute :

```apache
Listen 80
Listen 8080
```

Pour ajouter un port, ajoute une ligne `Listen` et redémarre Apache.

---

## Exemple : servir sur un port personnalisé

Dans `/etc/apache2/sites-available/monsite.conf` :

```apache
<VirtualHost *:8080>
    ServerName localhost
    DocumentRoot /var/www/monsite
    ...
</VirtualHost>
```

Et dans `ports.conf` :

```apache
Listen 8080
```

---

## Récap des commandes de configuration

| Action | Commande |
| --- | --- |
| Activer un site | `sudo a2ensite monsite.conf` |
| Désactiver un site | `sudo a2dissite monsite.conf` |
| Activer un module | `sudo a2enmod nom_module` |
| Désactiver un module | `sudo a2dismod nom_module` |
| Tester la config | `sudo apache2ctl configtest` |
| Recharger sans couper | `sudo systemctl reload apache2` |
| Redémarrer | `sudo systemctl restart apache2` |
