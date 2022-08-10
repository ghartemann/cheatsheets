# IONOS

## Héberger un projet
1. Créer un sous-domaine
2. Changer la version de PHP -> 8.1
3. Créer une BDD
4. Créer un build
```bash
yarn encore dev
```
4. Uploader le projet via FileZilla (dont le build)
5. Installer composer
```bash
curl -sS https://getcomposer.org/installer | php8.1
```

6. Installer les dépendances
```bash
php8.1 composer.phar install
```

6. Configurer les fichiers .env (à retravailler, pas la bonne méthode)
```env
DATABASE_URL="mysql://USERNAME:PASSWORD@HOST/DBNAME?serverVersion=5.7&charset=utf8mb4"
```

7. Passer les migrations
```bash
php8.1-cli bin/console d:m:m
```

8. Passer les fixtures
```bash
php8.1-cli bin/console d:f:l
```

9. Créer un fichier htaccess dans public (sinon les routes renvoient toutes 404)
```htaccess
DirectoryIndex index.php
<IfModule mod_negotiation.c>
    Options -MultiViews
</IfModule>

<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_URI}::$0 ^(/.+)/(.*)::\2$
    RewriteRule .* - [E=BASE:%1]
    RewriteCond %{HTTP:Authorization} .+
    RewriteRule ^ - [E=HTTP_AUTHORIZATION:%0]
    RewriteCond %{ENV:REDIRECT_STATUS} =""
    RewriteRule ^index\.php(?:/(.*)|$) %{ENV:BASE}/$1 [R=301,L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ %{ENV:BASE}/index.php [L]
</IfModule>

<IfModule !mod_rewrite.c>
    <IfModule mod_alias.c>
        RedirectMatch 307 ^/$ /index.php/
    </IfModule>
</IfModule>
```