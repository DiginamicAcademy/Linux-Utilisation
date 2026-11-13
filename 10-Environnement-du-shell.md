[← Chapitre précédent](09-Processus.md) · [Sommaire](Readme.md)

# L'environnement du shell

## 🎯 Objectifs du chapitre

- Garder ses sessions ouvertes avec le multiplexeur `screen`
- Créer, exporter et supprimer des variables
- Connaître les principales variables d'environnement
- Créer des alias

---

## 1. `screen` : multiplexeur de terminaux

`screen` permet :

- de garder une session **ouverte** même si la connexion SSH est coupée, et de la retrouver plus tard ;
- d'ouvrir **plusieurs terminaux** dans une seule console, et de passer de l'un à l'autre ;
- de **découper** l'écran pour en voir plusieurs à la fois.

```bash
screen              # ouvrir une session
screen -S maj       # ouvrir une session nommée « maj »
screen -ls          # lister les sessions
screen -r maj       # se rattacher à la session « maj »
```

Tous les raccourcis commencent par **`Ctrl+A`**, relâché, suivi d'une touche :

| Raccourci | Effet |
|---|---|
| `Ctrl+A` `c` | créer une fenêtre |
| `Ctrl+A` `k` | fermer la fenêtre courante |
| `Ctrl+A` `"` | lister les fenêtres |
| `Ctrl+A` `n` / `p` | fenêtre suivante / précédente |
| `Ctrl+A` `d` | **détacher** la session : elle continue de tourner |
| `Ctrl+A` `S` | découper l'écran horizontalement |
| `Ctrl+A` `Tab` | passer d'une zone à l'autre |
| `Ctrl+A` `X` | fermer la zone active |
| `Ctrl+A` `Échap` | mode copie, pour faire défiler l'historique ; `Échap` pour sortir |

La configuration se place dans `~/.screenrc`. Exemple :

```text
# ~/.screenrc
startup_message off        # pas d'écran d'accueil
vbell off                  # pas de clignotement
defscrollback 5000         # 5000 lignes d'historique par fenêtre
hardstatus alwayslastline  # barre d'état en bas de l'écran
hardstatus string "%{= kw}%-w%{= bw}%n %t%{-}%+w %=%H %c"
bindkey -k k8 prev         # F8 : fenêtre précédente
bindkey -k k9 next         # F9 : fenêtre suivante
```

> 💡 `tmux` est une alternative plus moderne à `screen`, sur le même principe
> (préfixe `Ctrl+B`).

---

## 2. Les variables

Deux sortes de variables :

- les **variables locales**, visibles seulement dans le shell courant ;
- les **variables d'environnement**, transmises aux programmes et sous-shells lancés depuis le shell, grâce à `export`.

```bash
prenom="Nicolas"        # affecter (pas d'espace autour du =)
echo "$prenom"          # utiliser : préfixer par $
export prenom           # rendre visible aux sous-shells
export LANG=fr_FR.UTF-8 # affecter et exporter en une fois
unset prenom            # supprimer
```

| Commande | Affiche |
|---|---|
| `set` | toutes les variables, locales et d'environnement (et les fonctions) |
| `env` | les variables d'environnement seulement |
| `echo $VAR` | la valeur d'une variable |

### Variables d'environnement courantes

Par convention, leur nom est en **majuscules**.

| Variable | Contenu |
|---|---|
| `HOME` (ou `~`) | répertoire personnel |
| `LOGNAME`, `USER` | nom de l'utilisateur connecté |
| `PATH` | répertoires où chercher les exécutables, séparés par `:` |
| `SHELL` | shell de connexion |
| `PS1` | invite de commande principale |
| `PS2` | invite de continuation |
| `TERM` | type de terminal |
| `MAIL` | chemin de la boîte aux lettres locale |
| `HISTFILE` | fichier de l'historique |
| `HISTSIZE` | nombre de commandes gardées en mémoire |
| `HISTFILESIZE` | nombre de commandes gardées dans le fichier |
| `HISTCONTROL` | options de l'historique (`ignoreboth` : ignorer les doublons et les commandes commençant par une espace) |
| `HISTTIMEFORMAT` | format d'horodatage de l'historique (absent par défaut) |

Pour horodater l'historique — utile pour la traçabilité, notamment du compte
root — ajouter dans `~/.bashrc` :

```bash
export HISTTIMEFORMAT="%Y/%m/%d @ %T  "
```

---

## 3. Les alias

Un **alias** associe un mot à une commande, avec ou sans arguments.

```bash
alias                       # lister les alias définis
alias ll='ls -l'            # créer un alias
alias rm='rm -i'            # demander confirmation à chaque suppression
unalias ll                  # supprimer un alias
```

Un alias créé en ligne de commande ne vaut que pour la session en cours.
Pour le rendre **permanent**, on l'ajoute à `~/.bashrc` (ou `~/.bash_aliases`
sur Debian et Ubuntu), puis on recharge le fichier :

```bash
echo "alias ll='ls -lh'" >> ~/.bashrc
source ~/.bashrc
```

---

## ✅ Exercice

1. Ouvrez une session `screen` nommée `tp`, lancez `top`, détachez-vous, puis rattachez-vous.
2. Affichez votre `PATH`. Ajoutez-y `~/bin` pour la session courante.
3. Activez l'horodatage de l'historique de façon permanente, et vérifiez avec `history`.
4. Créez un alias permanent `maj` qui lance `sudo apt update && sudo apt upgrade`.

<details>
<summary>Voir la correction</summary>

```bash
screen -S tp          # puis top, Ctrl+A d
screen -r tp
echo $PATH
export PATH="$PATH:$HOME/bin"
echo 'export HISTTIMEFORMAT="%Y/%m/%d @ %T  "' >> ~/.bashrc && source ~/.bashrc
history | tail -n 3
echo "alias maj='sudo apt update && sudo apt upgrade'" >> ~/.bashrc && source ~/.bashrc
```

</details>

---

[← Chapitre précédent](09-Processus.md) · [Sommaire](Readme.md)
