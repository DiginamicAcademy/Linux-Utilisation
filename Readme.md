<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/Logo-diginamic-color-blk.png">
    <img src="assets/Logo-diginamic-color.svg" alt="Diginamic Formation" width="358">
  </picture>
</p>

# Linux — utilisation

Présentation, découverte et utilisation d'un système GNU/Linux en ligne de
commande : son histoire et ses licences, son arborescence, puis les commandes
du quotidien — fichiers, recherche, édition avec vi, permissions, archives,
redirections, processus et environnement du shell.

**Auteur :** Nicolas SAHUC <sahuc.nic@gmail.com>

## 🎯 Objectifs pédagogiques

À la fin de ce cours, vous saurez :

1. Situer Linux dans l'histoire d'Unix, distinguer ses familles de distributions et ses licences.
2. Décrire les composants d'un système Linux et son arborescence de fichiers.
3. Vous connecter, vous repérer dans une session et trouver de l'aide (`--help`, `man`).
4. Naviguer dans l'arborescence et manipuler fichiers et répertoires.
5. Rechercher du texte avec `grep` et des fichiers avec `find`.
6. Éditer un fichier avec `vi`.
7. Lire et modifier les permissions Unix (`chmod`, `chown`, `chgrp`, `umask`).
8. Archiver et compresser avec `tar`, `gzip` et `bzip2`.
9. Rediriger les flux et enchaîner les commandes avec des pipes.
10. Gérer les processus, les variables d'environnement et les alias.

## ❔ Pourquoi ce cours ?

Linux fait tourner l'immense majorité des serveurs, du cloud, des conteneurs
et des équipements réseau. Un administrateur y travaille surtout à distance,
en SSH, sans interface graphique : la ligne de commande n'est pas une option,
c'est l'outil de travail.

## Prérequis

Une machine Linux (Debian ou Ubuntu de préférence) accessible en SSH ou en
console : machine virtuelle, WSL sous Windows, ou serveur de TP.

## Le parcours

| # | Étape | Durée | Ce qu'on y fait |
|---|---|---|---|
| [01](01-Presentation-de-Linux.md) | Présentation de Linux | 2 h | historique, distributions, licences, noyau, shells, arborescence |
| [02](02-Premiers-pas-et-aide.md) | Premiers pas et aide | 1 h 30 | connexion, arrêt, historique, informations de session, `man` |
| [03](03-Fichiers-et-repertoires.md) | Fichiers et répertoires | 2 h | chemins, navigation, création, copie, déplacement, affichage de texte |
| [04](04-Rechercher-grep-find.md) | Rechercher : grep et find | 1 h 30 | recherche de texte, recherche de fichiers, actions sur les résultats |
| [05](05-Editer-avec-vi.md) | Éditer avec vi | 2 h | modes, navigation, insertion, copier-coller, substitution, `.vimrc` |
| [06](06-Permissions.md) | Les permissions Unix | 2 h | lecture des droits, notation octale, `umask`, `chown`, `chgrp`, `chmod` |
| [07](07-Archivage-et-compression.md) | Archivage et compression | 1 h | `tar`, `gzip`, `bzip2` et décompression |
| [08](08-Redirections-et-pipes.md) | Redirections et pipes | 1 h 30 | flux standard, redirections, `/dev/null`, pipes, `tee` |
| [09](09-Processus.md) | Les processus | 1 h 30 | `ps`, `top`, `kill`, arrière-plan, `jobs` |
| [10](10-Environnement-du-shell.md) | L'environnement du shell | 1 h 30 | `screen`, variables, variables d'environnement, alias |
