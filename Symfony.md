# Symfony

## Dans le cas d'un projet from scratch :
1. Initialiser le projet Symfony

        symfony new --webapp <nom>

2. Modifier le fichier .env ou .env.local

        DATABASE_URL="mysql://USER:PASSWORD@127.0.0.1:3306/DBNAME?serverVersion=8&charset=utf8mb4"

3. Créer la BDD

        symfony console doctrine:database:create

4. Installer les fixtures

        composer require --dev orm-fixtures

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
6. Lancer encore

        yarn encore dev-server
        yarn encore dev

## Bootstrap :
1. Installer bootstrap

        yarn add bootstrap --dev

2. Dans app.scss, ajouter la ligne

        @import "~bootstrap/scss/bootstrap";

7. Installer le js de bootstrap

        yarn add @popperjs/core --dev

8. Dans app.js, ajouter

        require('bootstrap');

## Pour charger les images
1. Dans webpack.config.js, ajouter le bloc suivant :

        .copyFiles({
            from: './assets/images',

            // optional target path, relative to the output dir
            to: 'images/[path][name].[ext]',

            // if versioning is enabled, add the file hash too
            //to: 'images/[path][name].[hash:8].[ext]',

            // only copy files matching this pattern
            //pattern: /\.(png|jpg|jpeg)$/
        })

2. Installer file loader

        composer install <MISSING>


