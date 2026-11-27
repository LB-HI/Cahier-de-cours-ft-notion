# 📓 Cahier Malin × Notion

Fusion de **Cahier de cours** (l'éditeur en forme de feuille Seyès qui compile vers Notion) et de **[Cahier Malin](https://github.com/bradleyefrei/cahier-malin)** (enregistrement du cours, rédaction par l'IA, révisions, planning du soir), pour le BTS SIO.

Une seule page, `index.html`, qui marche de deux façons :

| | Dans claude.ai (navigateur ou app Claude) | Installée comme application Windows |
|---|---|---|
| Écrire le cours (éditeur complet) | ✅ | ✅ |
| Dicter | ✅ dictée Claude | — |
| **Enregistrer le cours au micro** | — | ✅ Whisper sur le PC, sans internet |
| Rédaction par l'IA | ✅ Claude | ✅ Claude ou **Ollama** (gratuit, local) |
| Fiches, flashcards, quiz, mnémotechniques, Feynman, carte mentale | ✅ | ✅ |
| Planning du soir 21:00 – 23:00 avec frise, export .ics | ✅ | ✅ |
| Rédaction automatique chaque soir dès 18:00 | ✅ si l'app est ouverte | ✅ + notification Windows |
| Compiler vers Notion | ✅ | ✅ |
| Rangement `Mes cours\<Matière>\<date - titre>\` (cours.md, fiches et quiz.md, transcription, audio Opus) | ⬇ téléchargement | ✅ automatique |
| Sauvegarde export / import (aussi depuis l'ancienne appli Cahier Malin) | ✅ | ✅ |

## Fonctionnalités

**Éditeur (Cahier de cours)**
- Titres T1 / T2 / T3 / T4 → violet, bleu, vert, orange (code couleur du bloc-notes Notion)
- Gras, italique, souligné, barré, `code dans la ligne` (Ctrl+E), liens (Ctrl+K), effacer la mise en forme
- 9 couleurs de texte, 9 surlignages et 9 fonds de paragraphe, identiques à la palette Notion ; citations
- Puces, numérotées, cases à cocher, indentation Tab / Maj+Tab
- Tableaux, 2 ou 3 colonnes, sections dépliantes, blocs de code avec langage, sommaire, séparateurs
- Équations LaTeX (KaTeX) et palette de symboles
- Encadrés : Correction, À retenir, Attention, Définition, Exemple, Méthode, Exercice, Astuce, Remarque
- Compiler vers Notion : à la suite, en sous-page ou dans une nouvelle matière ; « Voir le code »

**Cahier Malin**
1. **Enregistrer** le cours au micro (app Windows) : transcription Whisper (faster-whisper) sur le PC, langue détectée phrase par phrase, morceaux de 20 à 40 s coupés dans les silences, PC empêché de se mettre en veille. Chrono, niveau du micro et compteur de mots pendant le cours.
2. **Rédiger** : l'IA retire le bruit de la classe et produit un cours structuré (Claude, ou Ollama + `gemma3:4b` en local ; les cours très longs sont lus partie par partie avec l'IA locale).
3. **Réviser** : fiches, flashcards à 3 niveaux, quiz, moyens mnémotechniques, défis Feynman, carte mentale, écoute à voix haute.
4. **Planning du soir** : séances de 25 min entre 21:00 et 23:00, fin à 22:25 la veille d'un contrôle, frise de la soirée, export agenda.
5. **Automatique** : chaque soir à partir de 18:00 (heure réglable), tous les cours en attente sont rédigés avec leurs fiches et leurs quiz.
6. **Rangement par matière** : `Mes cours\<Matière>\<date - titre>\` contient l'audio (Opus), `transcription.txt`, `cours.md`, `fiches et quiz.md` et le texte d'origine. Un cours supprimé part dans `Mes cours\_Corbeille`.

Les cours, la progression (XP, série de jours) et les matières créées sont gardés avec le compte Claude et suivent sur tous les appareils.

## Installer l'application Windows

1. Ouvrir la page dans l'**app Claude pour Windows**, puis « Installer comme app ».
2. Ouvrir l'app installée → **Réglages** → « Autoriser l'accès à l'ordinateur ».
3. Prérequis : [Python 3.13](https://www.python.org/). Dans Réglages, « Installer les modules manquants » lance
   `py -3.13 -m pip install faster-whisper sounddevice numpy av`.
   On peut aussi indiquer le Python de l'environnement de Cahier Malin (`…\cahier-malin\.venv\Scripts\python.exe`).
4. Optionnel, pour l'IA locale gratuite : [Ollama](https://ollama.com), puis « Télécharger le modèle » dans Réglages.
5. « Reprendre les cours de l'ancienne appli Cahier Malin » importe les cours de `%LOCALAPPDATA%\Cahier Malin\donnees`.

Le modèle Whisper `small` (~460 Mo) se télécharge au premier enregistrement.

## Comment ça marche

- `index.html` est une page autonome (HTML + CSS + JS, KaTeX chargé depuis cdnjs). Elle utilise les capacités des artifacts claude.ai : `mcp` (connecteur Notion), `sample` (Claude), `dictation`, `db` + `user` (sauvegarde sur le compte), `downloads` et `desktop`.
- Installée, la page déclare la permission `full-access` (balise `claude-artifacts-manifest`) : elle écrit le moteur Python dans `%LOCALAPPDATA%\Cahier Malin\moteur\moteur.py` et le lance. Page et moteur dialoguent en JSON, une ligne par message (commandes sur l'entrée standard, événements sur la sortie).
- `moteur.py` (copie lisible ici, la page embarque la même) reprend le code de Cahier Malin : `Transcriber` (sounddevice 16 kHz + faster-whisper), compression Opus (PyAV), `LocalAI` (Ollama).
- À la compilation, le contenu est converti en **Markdown Notion** (`<h1 color="purple">`, `<callout>`, `<table>`, `$…$`…) puis envoyé avec `notion-update-page` ou `notion-create-pages`.

## Configuration

Les identifiants des pages Notion sont en haut du script dans `index.html` :

```js
const ROOT = '…';        // page « Léo — Efrei (BTS SIO 1) »
const DEFAULTS = [ … ];  // une entrée par matière : { id, name }
```

## Structure

```
├── index.html   # l'application complète (page + moteur Python embarqué)
├── moteur.py    # le moteur local, copie lisible de celui embarqué dans index.html
└── README.md
```
