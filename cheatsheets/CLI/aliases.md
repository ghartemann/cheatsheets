# Aliases

Cette fiche est adaptée à mes préférences (OhMyZsh sous macOS et édition sous VSCode). Un peu de travail d'adaptation sera nécessaire pour l'adapter à votre profil.

## Créer un alias
1. Ouvrir le fichier de config du CLI
```bash
code ~./zshrc
```

2. Ajouter une ligne par alias sous la section Example aliases, par exemple
```zsh
alias zshconfig="code ~/.zshrc"
```

## Mes aliases
```zsh
# Example aliases
alias zshconfig="code ~/.zshrc"
alias ohmyzsh="code ~/.oh-my-zsh"

# Encore
alias yed="yarn encore dev"
alias yeds="yarn encore dev-server"

# Symfony
alias sserve="symfony serve"
alias sc="symfony console"
alias mc="make:controller"
alias mf="make:form"
alias me="make:entity"
alias mm="make:migration"
alias mu="make:user"

# Git
alias ga="git add"
alias gc="git commit -m"
alias gpull="git pull"
alias gpush="git push -u origin"
alias gc="git checkout"
alias gcb="git checkout -b"

# Composer
alias cr="composer require"
alias ci="composer install"

# Yarn
alias ya="yarn add"
alias yi="yarn install"
```