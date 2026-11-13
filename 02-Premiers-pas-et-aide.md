[← Chapitre précédent](01-Presentation-de-Linux.md) · [Sommaire](Readme.md) · [Chapitre suivant →](03-Fichiers-et-repertoires.md)

# Premiers pas et aide

## 🎯 Objectifs du chapitre

- Se connecter, changer d'utilisateur, se déconnecter, arrêter la machine
- Gagner du temps avec l'autocomplétion et l'historique
- Obtenir des informations sur sa session
- Trouver de l'aide : `--help`, `man`, Internet

> 💡 **Convention.** Dans ce cours, les commandes sont données sans invite.
> Celles qui exigent les droits d'administration sont précédées de `sudo`,
> ou à lancer en tant que root.

---

## 1. Connexion, déconnexion, arrêt

On se connecte à un serveur Linux en **SSH**, avec un client comme PuTTY,
MobaXterm ou la commande `ssh` : saisir l'identifiant, puis le mot de passe.

```bash
ssh utilisateur@192.168.1.50
```

| Action | Commande |
|---|---|
| Prendre l'identité d'un autre utilisateur | `su utilisateur` |
| Devenir root, avec son environnement | `su -` |
| Exécuter une seule commande en root | `sudo commande` |
| Changer son mot de passe | `passwd` |
| Changer le mot de passe d'un autre utilisateur (root) | `passwd utilisateur` |
| Changer des mots de passe en masse (scripts) | `echo "login:nouveau_mdp" \| sudo chpasswd` |
| Se déconnecter | `exit` |
| Éteindre la machine | `shutdown -h now` (ou `shutdown` : arrêt dans une minute) |
| Annuler un arrêt programmé | `shutdown -c` |
| Redémarrer | `reboot` |

---

## 2. Autocomplétion et historique

La touche **Tab** complète les chemins, noms de fichiers, commandes et
variables. Deux appuis successifs listent les possibilités.

| Raccourci | Effet |
|---|---|
| `history` | afficher l'historique |
| `↑` / `↓` | remonter / redescendre dans les commandes précédentes |
| `Ctrl+R` | rechercher dans l'historique, du plus récent au plus ancien |
| `!!` | rappeler la dernière commande |
| `!com` | rappeler la dernière commande commençant par « com » |
| `!?com` | rappeler la dernière commande contenant « com » |
| `!118` | rappeler la commande n° 118 de l'historique |

> 💡 `sudo !!` relance la dernière commande avec les droits root — l'oubli de
> `sudo` est le plus fréquent des oublis.

---

## 3. Informations de session

| Question | Commande |
|---|---|
| Quel est mon terminal ? | `tty` |
| Sous quel login me suis-je connecté ? | `logname` |
| Qui suis-je, maintenant ? | `whoami` |
| Qui est connecté ? | `who` (`-H` : en-têtes, `-q` : liste abrégée, `-u` : détails) ou `w` |
| Quels sont mes identifiants et mes groupes ? | `id` |

`id` renvoie l'**UID** et le login de l'utilisateur, le **GID** et le nom de
son groupe principal, puis ses autres groupes :

```text
uid=1000(nsahuc) gid=1000(nsahuc) groupes=1000(nsahuc),4(adm),24(cdrom),27(sudo),46(plugdev)
```

### Date et heure

```bash
date        # date et heure courantes
cal         # calendrier du mois
```

---

## 4. Obtenir de l'aide

Les commandes Linux ont beaucoup d'options, difficiles à retenir. Savoir
trouver l'aide est la compétence la plus utile de ce chapitre.

### L'aide rapide : `--help`

La plupart des commandes affichent un résumé de leurs options :

```bash
date --help
```

### Le manuel : `man`

`man` affiche le manuel d'une commande, mis en forme pour le terminal (en
anglais le plus souvent).

```bash
man ls          # manuel de la commande ls
man 5 passwd    # section 5 : le format du fichier /etc/passwd
man man         # le manuel du manuel
man -k réseau   # rechercher les pages qui parlent d'un sujet
```

Les pages sont réparties en **sections** :

| Section | Contenu |
|---|---|
| 1 | commandes utilisateur |
| 2 | appels système |
| 3 | fonctions de bibliothèque |
| 4 | fichiers spéciaux (périphériques) |
| 5 | formats de fichiers |
| 6 | jeux |
| 7 | divers |
| 8 | commandes d'administration système |
| 9 | interfaces du noyau |

Dans `man`, on navigue comme dans `less` : `/mot` pour chercher, `n` pour
l'occurrence suivante, `q` pour quitter.

### Internet

Forums, documentations des distributions, wikis (le wiki d'Arch Linux, la
documentation Ubuntu…) : Internet regorge d'informations sur Linux. Encore
faut-il **cibler** ses recherches, puis **analyser** et **adapter** ce que
l'on trouve : beaucoup d'informations en ligne sont erronées ou obsolètes, et
une commande copiée sans être comprise peut faire des dégâts.

---

## ✅ Exercice

1. Affichez votre UID et la liste de vos groupes.
2. Trouvez, avec `man`, l'option de `date` qui affiche la date au format ISO 8601.
3. Rappelez la commande précédente sans la retaper.

<details>
<summary>Voir la correction</summary>

1. `id`
2. `man date`, puis `/iso` : c'est l'option `-I` (ou `--iso-8601`). `date -I` affiche par exemple `2026-11-13`.
3. `!!` ou flèche `↑` puis Entrée.

</details>

---

[← Chapitre précédent](01-Presentation-de-Linux.md) · [Sommaire](Readme.md) · [Chapitre suivant →](03-Fichiers-et-repertoires.md)
