# jesenlaplume.ca

Blogue technique (sécurité Microsoft 365 / Azure) : write-ups, projets de lab, CV.
Site statique généré avec [Hugo](https://gohugo.io/) (extended) et le thème
[PaperMod](https://github.com/adityatelange/hugo-PaperMod), publié sur GitHub Pages.

- Bilingue : anglais à la racine (`/`), français sous `/fr/`.
- Aucun traceur, aucune police ni script externe. Mode sombre automatique.
- Flux RSS : `/index.xml` (EN), `/fr/index.xml` (FR), et un flux par tag.

## Structure

```
.github/workflows/hugo.yml   Build + déploiement sur Pages à chaque push sur main
archetypes/posts.md          Gabarit de write-up (sections + checklist d'anonymisation)
assets/css/extended/         Petits ajustements CSS (chargés automatiquement par PaperMod)
content/en/                  Contenu anglais (posts/, projects.md, about.md, contact.md)
content/fr/                  Contenu français (posts/, projets.md, a-propos.md, contact.md)
data/projects.yaml           Liste des projets (une source pour les deux langues)
layouts/_markup/             Render hooks : coloration des blocs ```kql
layouts/_shortcodes/         Shortcode {{< projects >}}
themes/PaperMod/             Thème (submodule git, figé sur un commit)
hugo.toml                    Configuration (commentée)
```

## Installation (Fedora)

Utiliser **la même version de Hugo que la CI** (`HUGO_VERSION` dans
`.github/workflows/hugo.yml`, actuellement 0.170.1) pour éviter les surprises :

```bash
V=0.170.1
cd /tmp
curl -fsSLO https://github.com/gohugoio/hugo/releases/download/v$V/hugo_extended_${V}_linux-amd64.tar.gz
curl -fsSLO https://github.com/gohugoio/hugo/releases/download/v$V/hugo_${V}_checksums.txt
grep " hugo_extended_${V}_linux-amd64.tar.gz\$" hugo_${V}_checksums.txt | sha256sum -c -
tar -xzf hugo_extended_${V}_linux-amd64.tar.gz hugo
sudo install -m 0755 hugo /usr/local/bin/hugo
hugo version   # doit afficher « +extended » ou « extended »
```

Cloner le dépôt **avec le submodule** (sinon le site se construit sans thème) :

```bash
git clone --recurse-submodules git@github.com:le6009/blog.git
cd blog
# Déjà cloné sans --recurse-submodules ?
git submodule update --init --recursive
```

## Lancer en local

```bash
hugo server --buildDrafts
```

- http://localhost:1313/ (EN) et http://localhost:1313/fr/ (FR)
- `--buildDrafts` (ou `-D`) affiche aussi les brouillons (`draft: true`).
- La page se recharge à chaque sauvegarde. `Ctrl+C` pour arrêter.

## Créer un article

Chaque article est un dossier (« page bundle ») : `index.md` + ses captures.

```bash
# Français
hugo new content content/fr/posts/mon-article/index.md
# Anglais
hugo new content content/en/posts/my-post/index.md
```

L'archetype `archetypes/posts.md` remplit automatiquement le front matter, les
sections dans la bonne langue et la checklist d'anonymisation (commentaire HTML
en bas du fichier, retiré du site publié).

- **Tags permis :** `entra`, `pim`, `intune`, `defender`, `azure`, `homelab`, `offensive`, `ai-security`.
- **Traduction :** mettre la même valeur de `translationKey` dans les deux versions
  pour que le sélecteur de langue les relie. Un article peut n'exister que dans une langue.
- **Images :** déposer `capture-01.png` dans le dossier de l'article, puis
  `![Description](capture-01.png)`. Retirer les métadonnées : `exiftool -all= *.png`.
- **Code :** blocs ```` ```powershell ````, ```` ```kql ````, ```` ```bash ````,
  ```` ```bicep ````, ```` ```yaml ````. KQL est coloré avec le lexer SQL, faute de
  lexer KQL dans Hugo ; les commentaires `//` ne sont donc pas colorés.

## Publier

1. Valider la checklist d'anonymisation en bas de l'article.
2. Retirer toutes les notes `[À COMPLÉTER]`. Le workflow **refuse de déployer** un article publié qui en contient encore.
3. Passer `draft: false` dans le front matter.
4. Vérifier le rendu final sans les brouillons : `hugo server` (sans `-D`).
5. Commit et push sur `main` :

   ```bash
   git add content/
   git commit -m "post: mon article"
   git push
   ```

GitHub Actions construit et déploie le site en une à deux minutes (onglet **Actions** du dépôt).

## Ajouter un projet

Ajouter un bloc dans `data/projects.yaml` (modèle en commentaire dans le fichier).
Il apparaît dans les deux langues sur `/projects/` et `/fr/projets/`.

## Mettre à jour

```bash
# Thème PaperMod : avancer au dernier commit, tester, puis commiter le nouveau pointeur
git submodule update --remote themes/PaperMod
hugo server -D
git add themes/PaperMod && git commit -m "chore: update PaperMod"
```

Hugo : changer `HUGO_VERSION` dans le workflow **et** réinstaller la même version en local.
