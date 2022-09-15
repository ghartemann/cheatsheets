# Apache sous macOS (Apple Silicon)

macOS vient avec sa propre version d'Apache, indépendante de la version installée par Homebrew. Il est important de paramétrer la bonne version et ce guide en indique les étapes minimales.

Ressources :
- [git-tower](https://www.git-tower.com/blog/apache-on-macos/#installation)
- [Setting Up Your Local Web Server on macOS Big Sur 11.0.1](https://tech-cookbook.com/2020/11/14/setting-up-your-local-web-server-on-macos-big-sur-11-0-1-2020-mamp-macos-apache-mysql-php/)
- [Add and install PHP to macOS Monterey 12 with Homebrew](https://wpbeaches.com/updating-to-php-versions-7-4-and-8-on-macos-12-monterey/)
- [macOS 12.0 Monterey Apache Setup: Multiple PHP Versions](https://getgrav.org/blog/macos-monterey-apache-multiple-php-versions)

## Installer Apache

1. Annihiler la version d'Apache préinstallée avec macOS

```bash
sudo apachectl stop 
sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```

2. Installer Apache avec homebrew
``` bash
brew install httpd
```

## Configurer Apache

ATTENTION: le fichier de config httpd.conf se trouve dans /opt/homebrew/etc/httpd/ et non /private/etc/apache2/ qui correspond à la version embarquée d'Apache !

1. Activer l'auto-start
``` bash
sudo brew services start httpd
```

2. Changer le répertoire
``` conf
DocumentRoot "/opt/homebrew/var/www"
<Directory "/opt/homebrew/var/www">
```

en

``` conf
DocumentRoot "/Library/WebServer/Documents"
<Directory "/Library/WebServer/Documents">
```

(il est probablement possible de spécifier n'importe quel dossier)

3. Changer le port d'écoute
```conf
Listen 8080
```

en

```conf
Listen 80
```

4. Décommenter les lignes
```conf
LoadModule authn_core_module libexec/apache2/mod_authn_core.so
LoadModule authz_host_module libexec/apache2/mod_authz_host.so
LoadModule userdir_module libexec/apache2/mod_userdir.so
LoadModule include_module libexec/apache2/mod_include.so
LoadModule rewrite_module libexec/apache2/mod_rewrite.so
```

5. À la fin du fichier, ajouter les lignes

```conf
# Added lines for PHP integration
<FilesMatch .php$>
    SetHandler application/x-httpd-php
</FilesMatch>

<IfModule php_module>
        AddType application/x-httpd-php .php
        AddType application/x-httpd-php-source .phps

        <IfModule dir_module>
                DirectoryIndex index.html index.php
        </IfModule>
</IfModule>
```

## Commandes

### Lancer Apache
```bash
brew services start httpd
```

### Arrêter Apache
```bash
brew services stop httpd
```

### Redémarrer Apache
```bash
brew services restart httpd
```