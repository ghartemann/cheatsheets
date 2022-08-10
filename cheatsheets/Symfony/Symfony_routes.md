# Les routes avancées

Cette fiche est pas ouf, à retravailler.

## Les paramètres
### N'accepter que les chiffres en paramètre de route
```php
#[Route('/program/list/{page}', requirements: ['page'=>'\d+'], name: 'program_list')]
    public function list(int $page = 1): Response
    {
        // ...
    }
```

### Créer un nouvel item (affichage en GET et traitement du formulaire en POST)
```php
#[Route('/item/new', methods: ['GET', 'POST'], name: 'item_new')]
```

### Afficher une série en fonction de son identifiant, par un lien donc en GET
```php
#[Route('/program/{id}', methods: ['GET'], name: 'program_show')]
```

## Préfixer les routes
```php
#[Route('/program', name: 'program_')]
class ProgramController extends AbstractController
{
     // Correspond à la route /program et au name "program_index"
     #[Route('', name: 'index')]
     public function index(): Response
     {
         // ...
     }

     // Correspond à la route /program/new et au name "program_new"
     #[Route('/new', name: 'new')]
     public function new(): Response
     {
         // ...
     }
}
```

## Générer une route dans un controller
```php
public function show($id): Response
{
    return $this->redirectToRoute('program_show', ['id' => $id]);
}
```

## Rediriger dans la vue
```html
<a href="{{ path('program_list', {page: 5}) }}">Afficher la page 5</a>
```