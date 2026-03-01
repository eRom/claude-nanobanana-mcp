# Gotchas — Nano Banana MCP Server

> Dernière mise à jour : 2026-03-01

## Points d'attention

### 1. Double vérification de la clé API
L'entry point (`index.ts`) ET le service (`gemini.ts`) vérifient tous deux la présence de la clé API. La vérification dans `index.ts` fait un `process.exit(1)` avant même de connecter le serveur, tandis que `gemini.ts` lance une `Error`. Redondance intentionnelle pour couvrir les deux cas d'usage.

### 2. Variable d'environnement acceptée
Le serveur accepte `GEMINI_API_KEY` ou `GOOGLE_API_KEY` (dans cet ordre de priorité). Les deux noms fonctionnent — attention à ne pas les confondre.

### 3. Imports avec extensions `.js`
TypeScript est configuré en `Node16` module resolution — **tous les imports doivent avoir l'extension `.js`** même si les fichiers sources sont `.ts`. Oublier l'extension cause des erreurs de runtime non évidentes.

### 4. Tool `icon` : appels API multiples
Le tool `nanobanana_icon` fait **un appel API par taille demandée**. Avec `sizes: [16, 32, 64, 128, 256, 512]`, cela fait 6 appels séquentiels. Attention aux quotas API et à la latence.

### 5. `dist/` dans `.gitignore`
Le dossier `dist/` est ignoré par git. Après un clone, il faut exécuter `npm run build` avant de pouvoir utiliser `npm start`.

### 6. Modèles en preview
Les deux modèles (`gemini-3.1-flash-image-preview` et `gemini-3-pro-image-preview`) sont des versions **preview**. Les model IDs pourraient changer dans les versions futures de l'API.

### 7. Safety filters silencieux
L'API Gemini peut bloquer des requêtes via des safety filters. Le service détecte les réponses sans image et inclut le message texte du modèle si disponible, mais le refus peut sembler cryptique côté client.

## Aucun bug connu

Aucun bug identifié lors de cette session.
