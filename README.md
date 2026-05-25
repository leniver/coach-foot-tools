# coach-foot-tools

Boîte à outils web pour l'entraînement d'une équipe de foot. Des applications simples, **mobiles et tactiles**, qui fonctionnent **sans serveur** : il suffit d'ouvrir une page dans un navigateur.

## Outils

Toutes les pages servies sont dans le dossier `public/`.

| Outil | Fichier | Description |
|-------|---------|-------------|
| Accueil | `public/index.html` | Sélecteur qui liste tous les outils. |
| Exercices | `public/exercices.html` | Exercices d'entraînement classés par catégorie (échauffement, passes, tir, jeu…). |
| Chronométrage | `public/chrono.html` | Gérer des listes d'enfants, les chronométrer, puis créer 2 groupes équilibrés selon les temps. |

## Utilisation

Aucune installation, aucun serveur, aucune connexion nécessaire.

- **En local** : ouvrir `public/index.html` directement dans un navigateur.
- **En ligne** : servir le dossier `public/` sur n'importe quel hébergement de fichiers statiques (GitHub Pages, Apache `DocumentRoot`, etc.).

Les données du chronométrage sont sauvegardées automatiquement dans le navigateur (`localStorage`) ; rien n'est envoyé sur un serveur.

## Structure

- `public/styles.css` — design partagé par toutes les pages (couleurs, boutons, cartes…). Les fichiers servis restent ensemble dans `public/` (chemins relatifs).
- Chaque outil est une page HTML de `public/` qui inclut `styles.css` et garde ses styles spécifiques dans son propre `<style>`.

## Ajouter du contenu

**Un exercice** — ajouter une entrée au tableau `exercices` dans `public/exercices.html` :

```js
{ categorie: 'Passes', titre: 'Passe et va', duree: '10 min', description: '…' }
```

Les catégories apparaissent dans l'ordre de leur première occurrence.

**Un nouvel outil** — créer une page dans `public/` (qui inclut `styles.css`) puis ajouter une entrée au tableau `tools` dans `public/index.html` :

```js
{ title: 'Mon outil', description: '…', icon: '⚽', href: 'mon-outil.html' }
```
