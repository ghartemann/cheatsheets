# Authentification sous Symfony

Ces instructions sont valables pour Symfony 6.1. Se référer à [la doc](https://symfony.com/doc/current/security.html#form-login) en cas de doute.

## Utilisateurs
1. Créer un utilisateur

```bash
symfony console make:user
```

2. Effectuer une migration

```bash
php bin/console make:migration
php bin/console doctrine:migrations:migrate
```

## Authentification
1. Créer le controller

```bash
symfony console make:controller Login
```

2. Dans le fichier config/packages/security.yaml, rajouter les lignes

```yaml
security:
    firewalls:
        main:
            form_login:
                login_path: app_login
                check_path: app_login
```

3. Modifier LoginController.php pour qu'il corresponde à

```php
use Symfony\Component\Security\Http\Authentication\AuthenticationUtils;

class LoginController extends AbstractController
{
    #[Route('/login', name: 'app_login')]
    public function index(AuthenticationUtils $authenticationUtils): Response
    {
        // get the login error if there is one
        $error = $authenticationUtils->getLastAuthenticationError();

        // last username entered by the user
        $lastUsername = $authenticationUtils->getLastUsername();

        return $this->render('login/index.html.twig', [
            'last_username' => $lastUsername,
            'error'         => $error,
        ]);
    }
}
```

4. Modifier templates/login/index.html.twig pour qu'il corresponde à

```html
{% extends 'base.html.twig' %}

{% block body %}
    {% if error %}
        <div>{{ error.messageKey|trans(error.messageData, 'security') }}</div>
    {% endif %}

    <form action="{{ path('app_login') }}" method="post">
        <label for="username">Email:</label>
        <input type="text" id="username" name="_username" value="{{ last_username }}"/>

        <label for="password">Password:</label>
        <input type="password" id="password" name="_password"/>

        {# If you want to control the URL the user is redirected to on success
        <input type="hidden" name="_target_path" value="/account"/> #}

        <button type="submit">login</button>
    </form>
{% endblock %}
```

## Déconnexion

1. Ajouter les lignes suivantes dans config/packages/security.yaml
```yaml
security:
    firewalls:
        main:
            logout:
                path: app_logout

                # where to redirect after logout
                # target: app_any_route
```

2. Créer une route spécifique dans le controller (ici SecurityController.php mais probablement plus dans LoginController.php ? À vérifier)
```php
namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Routing\Annotation\Route;

class SecurityController extends AbstractController
{
    #[Route('/logout', name: 'app_logout', methods: ['GET'])]
    public function logout()
    {
        // controller can be blank: it will never be called!
        throw new \Exception('Don\'t forget to activate logout in security.yaml');
    }
}
```

## Remember me
TODO : https://symfony.com/doc/current/security/remember_me.html

## Création de compte
TODO
```bash
symfony console make:registration
```

## Sécurité
### Protection CSRF
1. Dans le fichier config/packages/security.yaml, rajouter la ligne

```yaml
security:
    firewalls:
        secured_area:
            form_login:
                enable_csrf: true
```

2. Dans templates/login/index.html.twig, ajouter au dessus du bouton submit

```html
<input type="hidden" name="_csrf_token" value="{{ csrf_token('authenticate') }}">
```

### Limiter les tentatives de connexion
Pour éviter le bruteforcing par exemple, ajouter les lignes suivantes à config/packages/security.yaml (selon celles qui vous intéressent)

```yaml
security:
    firewalls:
        main:
            # by default, the feature allows 5 login attempts per minute
            login_throttling: null

            # configure the maximum login attempts (per minute)
            login_throttling:
                max_attempts: 3

            # configure the maximum login attempts in a custom period of time
            login_throttling:
                max_attempts: 3
                interval: '15 minutes'
```