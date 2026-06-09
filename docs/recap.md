# Récap rapide

## Installation

```bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl status apache2
```

## Configurer un VirtualHost

```bash
sudo mkdir -p /var/www/monsite
sudo chown -R $USER:$USER /var/www/monsite
sudo nano /etc/apache2/sites-available/monsite.conf
```

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

<VirtualHost *:8080>
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

## Activer le site

```bash
sudo a2ensite monsite.conf
sudo a2dissite 000-default.conf
sudo systemctl reload apache2
```

## Tester la configuration

```bash
sudo apache2ctl configtest
```

## Corriger l'avertissement AH00558

```bash
echo "ServerName localhost" | sudo tee -a /etc/apache2/apache2.conf
```

## Activer les modules

```bash
sudo a2enmod rewrite
sudo a2enmod ssl
sudo systemctl restart apache2
```

## Configurer les ports

```bash
sudo nano /etc/apache2/ports.conf
# → ajouter : Listen 8080
```

## Lien symbolique vers le projet VS Code

```bash
sudo rm -r /var/www/monsite
sudo ln -s /home/administrateur/VScode/Training/Apache-training /var/www/monsite
```

## Permissions pour www-data

```bash
sudo usermod -aG administrateur www-data
sudo chmod g+x /home/administrateur
sudo systemctl restart apache2
```

## Vérifier que tout fonctionne

```bash
curl -I http://localhost
curl -I http://localhost:8080
```
