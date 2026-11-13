[← Chapitre précédent](05-Editer-avec-vi.md) · [Sommaire](Readme.md) · [Chapitre suivant →](07-Archivage-et-compression.md)

# Les permissions Unix

Chaque fichier a un **utilisateur propriétaire** et un **groupe
propriétaire**. Les permissions s'appliquent à trois catégories : le
propriétaire, le groupe, et **tous les autres**.

## 🎯 Objectifs du chapitre

- Lire les permissions d'un fichier ou d'un répertoire
- Passer de la notation symbolique à la notation octale
- Comprendre le masque `umask`
- Changer les propriétaires (`chown`, `chgrp`) et les droits (`chmod`)

---

## 1. Les trois droits

| Droit | Valeur octale | Sur un fichier | Sur un répertoire |
|---|---|---|---|
| `r` — lecture | 4 | lire le contenu | lister le contenu (`ls`) |
| `w` — écriture | 2 | modifier le contenu | créer, supprimer, renommer les fichiers qu'il contient |
| `x` — exécution | 1 | exécuter le fichier (programme, script) | le traverser : y entrer (`cd`), l'utiliser dans un chemin |

---

## 2. Lire les permissions

```bash
ls -l rapport.sh
```

```text
-rwxrw-r-- 1 nsahuc compta 2048 nov. 13 10:00 rapport.sh
│└┬┘└┬┘└┬┘   └──┬─┘ └──┬─┘
│ │  │  │       │      └ groupe propriétaire
│ │  │  │       └ utilisateur propriétaire
│ │  │  └ autres (other)
│ │  └ groupe (group)
│ └ propriétaire (user)
└ type : - fichier, d répertoire, l lien
```

Chaque bloc de trois positions se lit **r, w, x** ; un tiret signale un droit
absent.

| | Propriétaire | Groupe | Autres |
|---|---|---|---|
| Symbolique | `rwx` | `rw-` | `r--` |
| Binaire | 111 | 110 | 100 |
| Octal | 4+2+1 = **7** | 4+2 = **6** | **4** |

`rwxrw-r--` s'écrit donc **764**.

| Octal | Symbolique | Usage typique |
|---|---|---|
| 755 | `rwxr-xr-x` | répertoires, scripts partagés |
| 644 | `rw-r--r--` | fichiers ordinaires |
| 700 | `rwx------` | répertoire privé |
| 600 | `rw-------` | fichier secret (clé SSH privée) |

---

## 3. `umask` : les droits par défaut

À la création, un fichier reçoit des droits **maximaux** dont on retranche
le **masque** `umask` :

| | Droits maximaux | umask 022 | Résultat |
|---|---|---|---|
| Fichier | 666 (`rw-rw-rw-`) | − 022 | **644** (`rw-r--r--`) |
| Répertoire | 777 (`rwxrwxrwx`) | − 022 | **755** (`rwxr-xr-x`) |

Le droit d'exécution n'est jamais posé par défaut sur un fichier : il faut
l'ajouter explicitement avec `chmod`.

```bash
umask           # afficher le masque en octal : 0022
umask -S        # en symbolique : u=rwx,g=rx,o=rx
umask 027       # nouveaux fichiers en 640, répertoires en 750 (session courante)
```

> 💡 Pour un masque permanent, placez la commande `umask` dans `~/.bashrc`
> ou `~/.profile`.

---

## 4. Changer les propriétaires

### `chown` : utilisateur et groupe

Réservé à root.

```bash
sudo chown alice rapport.sh             # nouveau propriétaire
sudo chown alice:compta rapport.sh      # propriétaire et groupe
sudo chown -R alice:compta /srv/compta  # récursivement
```

### `chgrp` : groupe seulement

Le propriétaire d'un fichier peut en changer le groupe, à condition d'être
membre du groupe de destination ; root peut tout.

```bash
chgrp compta rapport.sh
chgrp -R compta /srv/compta
```

---

## 5. `chmod` : changer les droits

### En absolu (octal)

On remplace **tous** les droits d'un coup : à manier avec précaution.

```bash
chmod 764 rapport.sh
chmod -R 750 /srv/compta
```

### En relatif (symbolique)

`chmod` **qui** **comment** **quoi** :

| Qui | Comment | Quoi |
|---|---|---|
| `u` propriétaire | `+` ajouter | `r` lecture |
| `g` groupe | `-` retirer | `w` écriture |
| `o` autres | `=` fixer exactement | `x` exécution |
| `a` tous | | |

```bash
chmod u+x script.sh         # rendre exécutable pour le propriétaire
chmod go-w rapport.txt      # retirer l'écriture au groupe et aux autres
chmod o= secret.txt         # aucun droit pour les autres
chmod a+r,u+w public.txt    # plusieurs modifications à la fois
```

---

## ✅ Exercice

1. Quelle est la notation octale de `rw-r-----` ? Et la notation symbolique de `751` ?
2. Créez un fichier avec `umask 077` actif. Quels droits obtient-il ?
3. Créez un script `bonjour.sh` contenant `echo Bonjour`, rendez-le exécutable pour vous seul, puis lancez-le avec `./bonjour.sh`.

<details>
<summary>Voir la correction</summary>

1. `rw-r-----` = 640 ; 751 = `rwxr-x--x`.
2. 666 − 077 = 600 : `rw-------`.
3. ```bash
   echo 'echo Bonjour' > bonjour.sh
   chmod u+x bonjour.sh
   ./bonjour.sh
   ```

</details>

---

[← Chapitre précédent](05-Editer-avec-vi.md) · [Sommaire](Readme.md) · [Chapitre suivant →](07-Archivage-et-compression.md)
