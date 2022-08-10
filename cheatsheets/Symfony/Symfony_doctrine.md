# Doctrine

## Entités

Une entité est une simple classe, gérant les données :
* Dans le dossier src/Entity
* Une entité par table de BDD
* Le nom de l’entité est au singulier et en UpperCamelCase (ex : App\Entity\Product)
* Une propriété par champ de la table (ou presque). Les “clés étrangères” sont représentées par d’autres entités
* Les champs sont en camelCase (mais en snake_case côté SQL)
* La classe doit avoir les getters et setters correspondants à chaque propriété. */

## Initialiser la BDD
### Créer la base de donénes
```bash
symfony console d:d:c
```

### Créer une entité
```bash
symfony console make:entity
```
Si l'entité est créée manuellement, ne pas oublier
```php
use Doctrine\ORM\Mapping as ORM;
```

## Migrations
1. Génération d’une classe de migration
```bash
symfony console make:migration
```

2. Application des modifications en BDD
```bash
symfony console doctrine:migrations:migrate
symfony console d:m:m
```

3. Vérifie si la BDD est synchronisée avec l'application
```bash
symfony console doctrine:schema:validate
symfony console d:s:v
```

### Migrer sur l'état antérieur
```bash
symfony console doctrine:migrations:execute 'DoctrineMigrations\<nom_de_classe_de_ta_migration>' --down
```

## Commandes utiles

### Mettre à jour une entité
```bash
symfony console make:entity --regenerate
```

### Mapper les entités
```bash
symfony console doctrine:mapping:info
symfony console d:m:info
```

### Mais aussi
```bash
doctrine:database:create
doctrine:database:drop
doctrine:schema:create
doctrine:schema:drop
doctrine:schema:update
doctrine:schema:validate
doctrine:migrations:migrate
doctrine:migrations:status
```