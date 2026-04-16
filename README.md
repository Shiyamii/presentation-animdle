# presentation-animdle

Slides de présentation du projet **Animdle**, réalisées avec [Slidev](https://sli.dev).

## Structure de la présentation (15 min)

### 1. Demo (5 min)
- Démonstration rapide de l'application
- Parties intéressantes + bonus éventuels

### 2. Architecture (5 min)
- Architecture globale et choix techniques
- Modèle de données

### 3. Action complète (5 min)
- Trace complète d'une action : un utilisateur dessine un pixel
- Méthode client → appel API → route → service → insertion BDD
- Code montré à chaque étape

## Développement

```sh
npm install
npm run dev     # lance Slidev sur http://localhost:3030
```

## Build

```sh
npm run build   # exporte en SPA statique dans dist/
npm run export  # exporte en PDF
```
