# Architecture — Nano Banana MCP Server

> Dernière mise à jour : 2026-03-01

## Vue d'ensemble

**Type** : Serveur MCP (Model Context Protocol) local  
**Objectif** : Génération et édition d'images via l'API Gemini (modèles Nano Banana 2 / Pro)  
**Transport** : stdio (intégration locale avec Claude Code / Opencode / Antigravity)  
**Langage** : TypeScript (ES2022, module Node16)  
**Runtime** : Node.js ≥ 18

## Stack technique

| Couche | Technologie | Version |
|--------|-------------|---------|
| SDK MCP | `@modelcontextprotocol/sdk` | ^1.6.1 |
| API IA | `@google/genai` | ^1.0.0 |
| Validation | `zod` | ^3.23.8 |
| Build | `typescript` | ^5.7.2 |
| Dev | `tsx` (watch) | ^4.19.2 |

## Arborescence

```
src/
├── index.ts              # Entry point — initialise McpServer + stdio transport
├── constants.ts          # Tous les enums/types (models, ratios, styles, etc.)
├── services/
│   └── gemini.ts         # Service Gemini — client singleton, generate & edit
└── tools/
    ├── generate.ts       # Tool: nanobanana_generate (text-to-image)
    ├── edit.ts           # Tool: nanobanana_edit (image editing)
    ├── icon.ts           # Tool: nanobanana_icon (multi-size icon generation)
    └── diagram.ts        # Tool: nanobanana_diagram (technical diagrams)
```

## Architecture en couches

```
┌─────────────────────────────────┐
│         MCP Client (Claude)     │
└────────────┬────────────────────┘
             │ stdio
┌────────────▼────────────────────┐
│   index.ts — McpServer          │
│   (register tools, connect)     │
└────────────┬────────────────────┘
             │
┌────────────▼────────────────────┐
│   tools/*.ts                    │
│   (validation Zod, prompt       │
│    engineering, formatting)     │
└────────────┬────────────────────┘
             │
┌────────────▼────────────────────┐
│   services/gemini.ts            │
│   (client singleton, API calls, │
│    save PNG to disk)            │
└────────────┬────────────────────┘
             │ HTTPS
┌────────────▼────────────────────┐
│   Google GenAI API              │
│   (Gemini 3.1 Flash / Pro)      │
└─────────────────────────────────┘
```

## Flux de données principal

1. Le client MCP envoie un appel d'outil via stdio
2. `index.ts` route vers le tool handler approprié
3. Le tool valide les inputs (Zod), construit le prompt enrichi
4. `services/gemini.ts` appelle l'API Gemini avec le prompt
5. La réponse contient des `parts` (text + inlineData/base64)
6. L'image est décodée et sauvegardée en PNG sur le disque
7. Le chemin du fichier est retourné au client MCP

## Modèles disponibles

| Alias | Model ID | Usage |
|-------|----------|-------|
| Nano Banana 2 (défaut) | `gemini-3.1-flash-image-preview` | Vitesse, usage haute fréquence |
| Nano Banana Pro | `gemini-3-pro-image-preview` | Production professionnelle |

## Dépendances externes critiques

- **GEMINI_API_KEY** (ou GOOGLE_API_KEY) : variable d'environnement obligatoire
- **Filesystem** : écriture directe sur disque (PNG), création automatique des répertoires
