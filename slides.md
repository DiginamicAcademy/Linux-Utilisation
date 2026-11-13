---
marp: true
theme: default
style: |
  section {background-color: #121114}
  h1,h2,h3 {color: #8393f0}
  p,ul,li,td,th {color: #ccc}
  section table {font-size: 0.85em}
  section table td {background-color: #121114}
  section table th {background-color: #272133; font-weight: bolder}
  pre {background-color: #17151a; color: #ccc}
  blockquote {border: 2px green solid; border-left-width: 15px; padding: 0.5em 15px;font-style: italic }
paginate: true
header: "Linux — utilisation"
footer: "![height:20px](https://raw.githubusercontent.com/DiginamicInternal/PublicAssets/refs/heads/main/Logo-diginamic-color-blk.png)"
---

# Linux — utilisation

### Présentation, découverte et utilisation du système

Nicolas SAHUC

---

## 🎯 Objectifs pédagogiques

1. Situer Linux, ses distributions, ses licences
2. Décrire le noyau, le shell, l'arborescence
3. Se connecter, trouver de l'aide
4. Manipuler fichiers et répertoires
5. Rechercher avec `grep` et `find`
6. Éditer avec `vi`
7. Gérer les permissions
8. Archiver et compresser
9. Rediriger et enchaîner les commandes
10. Gérer processus, variables et alias

---

## Le parcours

| #  | Étape                       | Durée  |
|----|-----------------------------|--------|
| 01 | Présentation de Linux       | 2 h    |
| 02 | Premiers pas et aide        | 1 h 30 |
| 03 | Fichiers et répertoires     | 2 h    |
| 04 | Rechercher : grep et find   | 1 h 30 |
| 05 | Éditer avec vi              | 2 h    |
| 06 | Les permissions Unix        | 2 h    |
| 07 | Archivage et compression    | 1 h    |
| 08 | Redirections et pipes       | 1 h 30 |
| 09 | Les processus               | 1 h 30 |
| 10 | L'environnement du shell    | 1 h 30 |

---

# 1. Présentation de Linux

---

## D'Unix à GNU/Linux

- **1971** : Unix aux Bell Labs · **1973** : réécrit en C
- **1983** : projet GNU (Richard Stallman) · **1985** : FSF
- **1991** : noyau Linux (Linus Torvalds)
- **1993** : Debian, Slackware · **2004** : Ubuntu

> Linux est le noyau ; GNU/Linux est le système complet.

---

## Distributions et familles

| Famille | Exemples | Paquets |
|---|---|---|
| Debian | Debian, Ubuntu, Mint | `.deb` — `dpkg`, `apt` |
| Red Hat | RHEL, Fedora, Rocky, Alma | `.rpm` — `rpm`, `dnf` |
| Autres | Slackware, Gentoo, Arch | … |

---

## Licences

- **GPL** : exécuter, étudier, modifier, redistribuer… **sous la même licence** (copyleft)
- **LGPL** : plus souple, pour les bibliothèques
- **BSD** : très permissive, réutilisable dans du propriétaire
- *Free* = **libre**, pas forcément gratuit

> Licence ≠ support : un logiciel libre peut avoir un support payant.

---

## Noyau, shell, arborescence

- **Noyau** : ressources, processus
- **Shell** (bash) : interprète les commandes, langage de script
- **Tout est fichier**, une seule racine `/`
- Sensible à la casse, fichiers cachés en `.`

`/etc` config · `/home` utilisateurs · `/var/log` journaux · `/tmp` temporaire · `/usr` programmes

---

# 2. Premiers pas et aide

---

## Se connecter, se repérer

| Commande | Effet |
|---|---|
| `ssh user@hôte` | se connecter |
| `su -` / `sudo cmd` | devenir root / une commande en root |
| `passwd` | changer de mot de passe |
| `whoami`, `id`, `who` | qui suis-je, qui est là |
| `shutdown -h now`, `reboot` | arrêter, redémarrer |

**Tab** complète · **Ctrl+R** cherche dans l'historique · `!!` rappelle

---

## Trouver de l'aide

```bash
date --help
man ls
man 5 passwd
man -k réseau
```

> Sur Internet : cibler, vérifier, adapter. Ne jamais coller sans comprendre.

---

# 3. Fichiers et répertoires

---

## Naviguer et manipuler

| Commande | Effet |
|---|---|
| `pwd`, `cd`, `ls -la` | se repérer, se déplacer, lister |
| `mkdir -p`, `rmdir` | créer, supprimer un répertoire |
| `touch`, `cp -r`, `mv`, `rm -r` | créer, copier, déplacer, supprimer |
| `cat`, `less`, `head`, `tail -f` | lire |
| `wc -l`, `diff`, `du -sh` | compter, comparer, mesurer |

> Absolu : depuis `/` · Relatif : depuis ici (`.`, `..`, `~`)

---

# 4. Rechercher

---

## grep et find

```bash
grep -rin 'erreur' /var/log        # du texte dans des fichiers
grep -v '^#' /etc/fstab            # tout sauf les commentaires
find /etc -name '*.conf'           # des fichiers par nom
find /var/log -size +10M -mtime +7 # par taille et date
find /tmp -name '*.tmp' -exec rm {} \;
```

---

# 5. Éditer avec vi

---

## Les modes de vi

- **Commande** (par défaut) · **Insertion** (`i`, `a`, `o`) · **Ligne** (`:`)
- **Échap** ramène toujours en mode commande

| | |
|---|---|
| `:wq` / `:q!` | enregistrer et quitter / quitter sans enregistrer |
| `dd`, `yy`, `p`, `u` | couper, copier, coller, annuler |
| `/texte`, `n` | chercher, suivant |
| `:%s/a/b/g` | remplacer partout |

---

# 6. Permissions

---

## Lire les droits

`-rwxrw-r--` → propriétaire `rwx` (7) · groupe `rw-` (6) · autres `r--` (4) → **764**

| | r = 4 | w = 2 | x = 1 |
|---|---|---|---|
| Fichier | lire | modifier | exécuter |
| Répertoire | lister | créer/supprimer dedans | traverser |

---

## Modifier les droits

```bash
chmod 640 fichier          # absolu
chmod u+x,go-w script.sh   # relatif
sudo chown alice:compta f  # propriétaire et groupe
chgrp compta f             # groupe
umask 022                  # fichiers 644, répertoires 755
```

---

# 7. Archivage et compression

---

## tar, gzip, bzip2

```bash
tar -czvf archive.tar.gz dossier/   # créer + gzip
tar -tzvf archive.tar.gz            # lister
tar -xzvf archive.tar.gz -C /dest   # extraire
gzip f / gunzip f.gz / zcat f.gz
bzip2 f / bunzip2 f.bz2 / bzcat f.bz2
```

**c**rée · e**x**trait · **t**able · **z** gzip · **j** bzip2 · **v** voir · **f** fichier

---

# 8. Redirections et pipes

---

## Les flux

| N° | Flux | Redirection |
|---|---|---|
| 0 | stdin | `< fichier`, `<< FIN` |
| 1 | stdout | `> fichier`, `>> fichier` |
| 2 | stderr | `2> fichier`, `2>/dev/null` |

```bash
cmd > tout.txt 2>&1
cut -d: -f7 /etc/passwd | sort | uniq -c
ls -l | tee liste.txt
```

---

# 9. Processus

---

## Observer et contrôler

| Commande | Effet |
|---|---|
| `ps -ef`, `pstree -p`, `top` | observer |
| `pgrep -l nom` | trouver un PID |
| `kill PID` / `kill -9 PID` | arrêter poliment / brutalement |
| `cmd &`, `jobs`, `fg`, `bg`, `Ctrl+Z` | arrière-plan |
| `time cmd` | mesurer |

---

# 10. Environnement du shell

---

## screen, variables, alias

```bash
screen -S tp       # Ctrl+A d pour détacher
screen -r tp       # se rattacher
export VAR=valeur  # variable d'environnement
echo $PATH
alias ll='ls -lh'  # à mettre dans ~/.bashrc
```

---

# Des questions ?

Pour toute question [adressez-vous au service pédagogie](mailto:pedagogie@diginamic.fr)
