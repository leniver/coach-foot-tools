# coach-foot-tools

Boîte à outils web pour l'entraînement d'une équipe de foot. Des applications simples, **mobiles et tactiles**, qui fonctionnent **sans serveur** : il suffit d'ouvrir une page dans un navigateur.

## Outils

| Outil | Fichier | Description |
|-------|---------|-------------|
| Accueil | `index.html` | Sélecteur qui liste tous les outils. |
| Exercices | `exercices.html` | Exercices d'entraînement classés par catégorie (échauffement, passes, tir, jeu…). |
| Chronométrage | `chrono.html` | Chronométrer plusieurs enfants en même temps, puis créer 2 groupes équilibrés selon les temps. |

## Utilisation

Aucune installation, aucun serveur, aucune connexion nécessaire.

- **En local** : ouvrir `index.html` directement dans un navigateur.
- **En ligne** : déposer le dossier sur n'importe quel hébergement de fichiers statiques (GitHub Pages, etc.).

Les données du chronométrage sont sauvegardées automatiquement dans le navigateur (`localStorage`) ; rien n'est envoyé sur un serveur.

## Structure

- `styles.css` — design partagé par toutes les pages (couleurs, boutons, cartes…). Garder les fichiers dans le même dossier (chemins relatifs).
- Chaque outil est une page HTML autonome qui inclut `styles.css` et garde ses styles spécifiques dans son propre `<style>`.

## Ajouter du contenu

**Un exercice** — ajouter une entrée au tableau `exercices` dans `exercices.html` :

```js
{ categorie: 'Passes', titre: 'Passe et va', duree: '10 min', description: '…' }
```

Les catégories apparaissent dans l'ordre de leur première occurrence.

**Un nouvel outil** — créer une page (qui inclut `styles.css`) puis ajouter une entrée au tableau `tools` dans `index.html` :

```js
{ title: 'Mon outil', description: '…', icon: '⚽', href: 'mon-outil.html' }
```
