# Authentification sous Symfony

Ces instructions song valables pour Symfony 6.1. Se référer à [la doc](https://symfony.com/doc/current/security.html#form-login) en cas de doute.

## Utilisateurs :
1. Créer un utilisateur

        symfony console make:user

2. Effectuer une migration

        php bin/console make:migration
        php bin/console doctrine:migrations:migrate

## Authentification :
1. Créer le controller

        symfony console make:controller Login

2. Dans le fichier config/packages/security.yaml, rajouter les lignes

        security:
            # ...
            firewalls:
                main:
                    # ...
                    form_login:
                        login_path: app_login
                        check_path: app_login

3. Modifier LoginController.php pour qu'il corresponde à

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

4. Modifier templates/login/index.html.twig pour qu'il corresponde à

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

## Protection CSRF :
1. Dans le fichier config/packages/security.yaml, rajouter la ligne

        security:
        # ...
        firewalls:
            secured_area:
                # ...
                form_login:
                    # ...
                    enable_csrf: true

2. Dans templates/login/index.html.twig, ajouter au dessus du bouton submit

        <input type="hidden" name="_csrf_token" value="{{ csrf_token('authenticate') }}">