# Formulaires

## Créer un formulaire

### Créer un Type
On utilise la commande

```bash
symfony console make:form
```

Elle a pour résultat de créer une classe :

```php
class CustomerType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('firstname', TextType::class)
            ->add('lastname', TextType::class)
            ->add('email', EmailType::class);
    }

    public function configurationOptions(OptionsResolver $resolver)
    {
        $resolver->setDefaults([
            'data_class' => Wilder::class,
        ]);
    }
}
```

### Afficher le formulaire
Pour afficher un formulaire via Twig, on intervient dans deux fichiers : un form et la page plus générale qui incluera ce dernier.

Dans customer/_form.html.twig :
```html
{{ form_start(form) }}
    {{ form_widget(form) }}
    <button class="btn">{{ button_label|default('Save') }}</button>
{{ form_end(form) }}
```

Dans customer/add.html.twig :
```html
{% extends 'base.html.twig' %}

{% block title %}New Customer{% endblock %}

{% block body %}
    <h1>Create a new customer</h1>
    {{ include('customer/_form.html.twig') }}
    <a href="{{ path('customer_index') }}">Back</a>
{% endblock %}
```

## Personnaliser le formulaire

### Types d'inputs

Quelques exemples :
- Champ à choix multiples

```php
ChoiceType::class
```

- Champ date

```php
DateType::class
```

- Champ téléphone

```php
TelType::class
```

[Voir la liste complète](https://symfony.com/doc/current/reference/forms/types.html)


## Passer un tableau d'options
Dans la classe Type, on peut ajouter des options à chaque champ

```php
$builder->add(‘language’, TextType::class, [
    ‘label’ => ‘language’,
    'attr' => [’placeholder' => 'Entrez le meilleur langage (PHP)'],
]);
```

## Valider le formulaire

### Ajouter des assertions
La validation s'effectue dans l'entité elle-même.

On ajoute tout d'abord un use :

```php
use Symfony\Component\Validator\Constraints as Assert;
```

Puis, en annotation de chaque propriété, on ajoute des assertions :
```php
#[ORM\Entity(repositoryClass: CustomerRepository::class)]
class Customer
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: ”integer”]
    private $id;
    
    #[ORM\Column(type: ”string”, length: 255)]
    #[Assert\NotBlank]
    private $firstname;

    #[ORM\Column(type: ”string”, length: 255)]
    #[Assert\NotBlank]
    private $lastname

    #[ORM\Column(type: 'string', length: 255)]
    #[Assert\NotBlank(message: 'This field can\'t be left empty.')]
    #[Assert\Length(
        max: 255,
        maxMessage: 'La catégorie saisie {{ value }} est trop longue, elle ne devrait pas dépasser {{ limit }} caractères',
    )]
    private $whatever;
```

Une contrainte ajoute automatiquement une à la vue, par exemple :
```php
#[Assert\Length(max: 10)]
```
ajoutera
```html
maxlength=10
```

### Types d'assertions
Le champ n'est pas vide :
```php
#[Assert\NotBlank]
```

Le champ contient un IBAN :
```php
#[Assert\Iban]
```

Le champ contient un email :
```php
#[Assert\Email]
```

Le champ contient une date :
```php
#[Assert\Date]
```

La saisie fait moins de 255 caractères :
```php
#[Assert\Length(max: 255)]
```

Le champ contient un tableau avec moins de 5 entrées :
```php
#[Assert\Count(max: 5)]
```

Le champ contient une URL :
```php
#[Assert\Url]
```

L’image fait entre 200 et 600 pixels de largeur :
```php
#[Assert\Image(minWidth: 200, maxWidth: 600)]
```

Pour empêcher un champ entré d'être soumis s'il existe déjà (ATTENTION : se met au niveau de la classe) :

```php
#[ORM\Entity]
#[UniqueEntity('email')]
class User
{
    // ...
}
```

[Voir la liste complète](https://symfony.com/doc/current/validation.html#constraints)

### Optionnel : neutraliser la vérification HTML
Ajouter, dans le type :

```php
{{ form_start(form, {'attr': {'novalidate': 'novalidate'}}) }}
    {{ form_widget(form) }}
{{ form_end(form) }}
```

### Gérer la partie controller
Ne pas oublier d'ajouter

```php
->isValid()
```

Par exemple :
```php
if ($form->isSubmitted() && $form->isValid()) {
    // handle data, in example, an insert into database
    // redirection
}
```

## Traiter le formulaire

Exemple d'un traitement de formulaire complet dans le controller :

```php
#[Route(“/new”, name: ”wilder_new”, methods: [”GET”, “POST”])]
public function new(
    Request             $request, 
    CustomerRepository  $customerRepository
): Response
{
    $customer = new Customer();
    $form = $this->createForm(WilderType::class, $customer);
    $form->handleRequest($request);

    if ($form->isSubmitted() && $form->isValid()) {
        $customerRepository->add($customer, true)
    
        return $this->redirectToRoute('customer_index');
    }

    return $this->renderForm(customer/new.html.twig’, [
        ‘wilder’ => $wilder,
        ‘form’ => $form,
   ]);
}
```