[← Chapitre précédent](03-Fichiers-et-repertoires.md) · [Sommaire](Readme.md) · [Chapitre suivant →](05-Editer-avec-vi.md)

# Rechercher : grep et find

## 🎯 Objectifs du chapitre

- Rechercher une expression dans le contenu des fichiers avec `grep`
- Rechercher des fichiers selon leur nom, leur type, leur taille… avec `find`
- Exécuter une action sur chaque fichier trouvé

| Je cherche… | Commande |
|---|---|
| … du **texte dans** des fichiers | `grep` |
| … des **fichiers** dans l'arborescence | `find` |

---

## 1. `grep` : chercher dans le contenu

```bash
grep [options] 'expression' fichier(s)
```

```bash
grep 'root' /etc/passwd             # lignes contenant « root »
grep -i 'error' /var/log/syslog     # sans tenir compte de la casse
grep -rn 'PermitRootLogin' /etc/ssh # récursif, avec numéros de ligne
grep -v '^#' /etc/fstab             # lignes qui ne sont pas des commentaires
```

| Option | Effet |
|---|---|
| `-c` | afficher le **nombre** de lignes qui contiennent l'expression |
| `-h` | ne pas préfixer les résultats par le nom de fichier |
| `-i` | ignorer la casse |
| `-l` | afficher seulement le **nom** des fichiers qui contiennent l'expression |
| `-n` | préfixer chaque ligne par son numéro |
| `-v` | **inverser** : lignes qui ne contiennent **pas** l'expression |
| `-r` | rechercher récursivement dans un répertoire |

`grep` interprète l'expression comme une **expression régulière** : il faut
la protéger par des apostrophes `'…'` pour que le shell ne l'interprète pas
avant lui.

| Motif | Correspond à |
|---|---|
| `^abc` | « abc » en début de ligne |
| `abc$` | « abc » en fin de ligne |
| `a.c` | « a », un caractère quelconque, « c » |
| `[0-9]` | un chiffre |
| `ab*` | « a » suivi de zéro, un ou plusieurs « b » |

---

## 2. `find` : chercher des fichiers

```bash
find <chemin> <critères> [action]
```

```bash
find /etc -name '*.conf'                # par nom (motif entre apostrophes)
find /home -user nsahuc                 # par propriétaire
find /var/log -type f -size +10M        # fichiers de plus de 10 Mo
find /tmp -type f -mtime +7             # modifiés il y a plus de 7 jours
```

### Les critères

| Critère | Recherche selon |
|---|---|
| `-name motif` | le nom (`-iname` : sans tenir compte de la casse) |
| `-user nom` | l'utilisateur propriétaire |
| `-group nom` | le groupe propriétaire |
| `-type f` / `-type d` | le type : fichier ordinaire / répertoire |
| `-size n` | la taille : `+10M` (plus de 10 Mo), `-100k` (moins de 100 Ko), `n` suivi de `c` en octets |
| `-mtime n` | la date de dernière modification, en jours (`+n` : plus de n jours) |
| `-ctime n` | la date de dernier changement d'état (contenu ou métadonnées) |

### Combiner les critères

Les critères juxtaposés se combinent par un **ET** implicite. Pour le reste :

| Opérateur | Sens |
|---|---|
| `!` | négation |
| `-o` | OU |
| `-a` | ET (implicite) |
| `\( … \)` | regroupement |

```bash
# fichiers .log OU .gz, de plus de 7 jours
find /var/log \( -name '*.log' -o -name '*.gz' \) -mtime +7
```

### Les actions

| Action | Effet |
|---|---|
| `-print` | afficher les chemins trouvés (action par défaut) |
| `-exec cmd {} \;` | exécuter `cmd` sur chaque fichier trouvé ; `{}` est remplacé par son chemin |
| `-ok cmd {} \;` | comme `-exec`, avec demande de confirmation |
| `-delete` | supprimer les fichiers trouvés |

```bash
find /tmp -name '*.tmp' -ok rm {} \;          # supprimer, avec confirmation
find /etc -name '*.conf' -exec grep -l 'Listen' {} \;   # quels .conf contiennent « Listen » ?
```

> ⚠️ Testez toujours un `find … -exec rm` ou `-delete` **sans** l'action
> d'abord : vous verrez exactement ce qui sera supprimé.

---

## ✅ Exercice

1. Combien de comptes de `/etc/passwd` ont `/bin/bash` pour shell ?
2. Listez les lignes de `/etc/ssh/sshd_config` qui ne sont ni des commentaires ni vides.
3. Trouvez les répertoires nommés `log` sous `/var`.
4. Trouvez, sous `/etc`, les fichiers modifiés il y a moins de 2 jours.

<details>
<summary>Voir la correction</summary>

```bash
grep -c '/bin/bash$' /etc/passwd
grep -v '^#' /etc/ssh/sshd_config | grep -v '^$'
find /var -type d -name log
sudo find /etc -type f -mtime -2
```

</details>

---

[← Chapitre précédent](03-Fichiers-et-repertoires.md) · [Sommaire](Readme.md) · [Chapitre suivant →](05-Editer-avec-vi.md)
