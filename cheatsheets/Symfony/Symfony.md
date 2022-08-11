# Symfony

## Dans le cas d'un projet from scratch
1. Initialiser le projet Symfony

```bash
symfony new --webapp <nom>
```

2. Modifier le fichier .env ou .env.local

```env
DATABASE_URL="mysql://USER:PASSWORD@127.0.0.1:3306/DBNAME?serverVersion=8&charset=utf8mb4"
```

3. Créer la BDD

```bash
symfony console doctrine:database:create
```

4. Installer les fixtures

```bash
composer require --dev orm-fixtures
```

## Dans le cas d'un git clone
1. Installer les dépendances composer

```bash
composer install
```

2. Installer les dépendances yarn

```bash
yarn install
```

3. Modifier le fichier .env ou .env.local

```env
DATABASE_URL="mysql://USER:PASSWORD@127.0.0.1:3306/DBNAME?serverVersion=8&charset=utf8mb4"
```

4. Créer la BDD

```bash
symfony console doctrine:database:create
```

5. Effectuer les ligrations

```bash
symfony console doctrine:migrations:migrate
```

6. Charger les fxtures

```bash
symfony console doctrine:fixtures:load
```

## Webpack Encore / SassLoader
1. Installer Webpack Encore

```bash
composer require symfony/webpack-encore-bundle
```

2. Installer SassLoader

```bash
yarn add sass-loader@^12.0.0 sass --dev
```

3. Dans webpack.config.js, décommenter la ligne 59

```js
//.enableSassLoader()
```

5. Modifier dans app.js -> app.css en app.scss
6. Lancer encore

```bash
yarn encore dev-server
yarn encore dev
```

## Bootstrap
1. Installer bootstrap

```bash
yarn add bootstrap --dev
```

2. Dans app.scss, ajouter la ligne

```scss
@import "~bootstrap/scss/bootstrap";
```

7. Installer le js de bootstrap

```bash
yarn add @popperjs/core --dev
```

8. Dans app.js, ajouter

```js
require('bootstrap');
```

## Pour charger les images
1. Dans webpack.config.js, ajouter le bloc suivant :

```js
.copyFiles({
    from: './assets/images',

    // optional target path, relative to the output dir
    to: 'images/[path][name].[ext]',

    // if versioning is enabled, add the file hash too
    //to: 'images/[path][name].[hash:8].[ext]',

    // only copy files matching this pattern
    //pattern: /\.(png|jpg|jpeg)$/
})
```

2. Installer file loader

```bash
yarn add file-loader@^6.0.0 --dev
```
