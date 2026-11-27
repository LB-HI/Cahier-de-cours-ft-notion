# 📓 Cahier de cours

Éditeur de prise de notes pour le BTS SIO, en forme de feuille de cahier Seyès. Quand le cours est terminé, le bouton **Compiler vers Notion** l'ajoute directement dans le bloc-notes Notion, avec la même mise en forme que le reste des cours.

## Fonctionnalités

**Texte**
- Titres T1 / T2 / T3 / T4 → violet, bleu, vert, orange (code couleur du bloc-notes Notion)
- Gras, italique, souligné, barré, `code dans la ligne` (Ctrl+E), liens (Ctrl+K), effacer la mise en forme
- 9 couleurs de texte, 9 surlignages et 9 fonds de paragraphe, identiques à la palette Notion
- Citations

**Listes**
- Puces, numérotées, cases à cocher (cliquer sur la case pour cocher)
- Indentation avec Tab / Maj+Tab

**Mise en page**
- Tableaux (ajout/suppression de lignes et colonnes, ligne d'en-tête)
- 2 ou 3 colonnes
- Sections dépliantes
- Blocs de code avec langage (bash, PowerShell, SQL, Python, HTML…)
- Sommaire automatique, séparateurs

**Maths**
- Équations LaTeX dans la ligne ou centrées, avec boutons ∀ ∃ ∈ ℝ ⇒ ⇔ ∧ ∨, fractions, puissances… et aperçu en direct (KaTeX)
- Palette de symboles à insérer dans le texte

**Encadrés**
- Correction ✅, À retenir 📌, Attention ⚠️, Définition 📘, Exemple 💡, Méthode 🛠️, Exercice ✏️, Astuce ✨, Remarque 📝

**Le reste**
- Rangement : à la suite dans la page de la matière, dans une nouvelle sous-page, ou dans une nouvelle matière
- Brouillon enregistré automatiquement dans le navigateur
- Voir le code : affiche le Markdown envoyé à Notion (copiable)
- Installable comme application depuis l'app Claude pour ordinateur

## Comment ça marche

`index.html` est une page autonome (HTML + CSS + JS, aucune dépendance à installer, KaTeX chargé depuis cdnjs).

1. L'éditeur est une zone `contenteditable`.
2. À la compilation, le contenu est converti en **Markdown Notion** (balises `<h1 color="purple">`, `<span color="red">`, `<callout>`, `<table>`, `$…$` pour les équations).
3. Le cours est envoyé via le **connecteur Notion de claude.ai** (`notion-update-page` ou `notion-create-pages`).

> ⚠️ L'envoi vers Notion passe par la capacité `mcp` des artifacts claude.ai : il ne fonctionne que lorsque la page est ouverte **depuis claude.ai** avec le connecteur Notion activé. Ouverte en local, la page fonctionne comme éditeur, et « Voir le code » permet de copier le résultat.

## Configuration

Les identifiants des pages Notion (page racine et matières) sont en haut du script dans `index.html` :

```js
const ROOT = '…';        // page « Léo — Efrei (BTS SIO 1) »
const DEFAULTS = [ … ];  // une entrée par matière : { id, name }
```

## Structure

```
cahier-de-cours/
├── index.html   # l'éditeur complet
└── README.md
```
