# Symfony

## Dans le cas d'un git clone :
1. Installer les dépendances composer
        composer install
2. Installer les dépendances yarn
        yarn install
3. Modifier le fichier .env ou .env.local
        DATABASE_URL="mysql://USER:PASSWORD@127.0.0.1:3306/DBNAME?serverVersion=8&charset=utf8mb4"
4. Créer la BDD
        symfony console doctrine:database:create
5. Effectuer les ligrations
        symfony console doctrine:migrations:migrate
6. Charger les fxtures
        symfony console doctrine:fixtures:load

## Webpack Encore / SassLoader :
1. Installer Webpack Encore
        composer require symfony/webpack-encore-bundle
2. Installer SassLoader
        yarn add sass-loader@^12.0.0 sass --dev
3. Dans webpack.config.js, décommenter la ligne 59
        //.enableSassLoader()
5. Modifier dans app.js -> app.css en app.scss

## Marche à suivre :


3. composer require --dev orm-fixtures
4. composer install
5. yarn install
6. Créer et modifier .env.local
7. symfony console doctrine:database:create
8. symfony console doctrine:migrations:migrate
9. symfony console doctrine:fixtures:load


## Ajouter bootstrap :
1. yarn add bootstrap --dev


4. yarn encore dev (ou yarn encore dev-server)

6. Dans app.scss -> ajouter la ligne @import "~bootstrap/scss/bootstrap";
7. yarn add @popperjs/core --dev
8. Dans app.js -> ajouter require('bootstrap');


## Pour charger les images
Dans webpack.config.js, ajouter le bloc suivant :

    .copyFiles({
        from: './assets/images',

        // optional target path, relative to the output dir
        to: 'images/[path][name].[ext]',

        // if versioning is enabled, add the file hash too
        //to: 'images/[path][name].[hash:8].[ext]',

        // only copy files matching this pattern
        //pattern: /\.(png|jpg|jpeg)$/
    })