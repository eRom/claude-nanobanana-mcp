# Fichiers clés — Nano Banana MCP Server

> Dernière mise à jour : 2026-03-01 (session 2)

## Core

| Fichier | Rôle |
|---------|------|
| `src/index.ts` | Entry point — crée le McpServer, enregistre les 4 tools, lance le transport stdio |
| `src/constants.ts` | Définit tous les enums typés : models, aspect ratios, resolutions, diagram/icon types & styles |

## Service

| Fichier | Rôle |
|---------|------|
| `src/services/gemini.ts` | Client singleton GoogleGenAI, fonctions `generateImage()` et `editImage()`, sauvegarde PNG, utilitaires filename/mime |

## Tools

| Fichier | Rôle |
|---------|------|
| `src/tools/generate.ts` | Tool `nanobanana_generate` — text-to-image avec style, aspect ratio, résolution |
| `src/tools/edit.ts` | Tool `nanobanana_edit` — édition d'image existante via prompt en langage naturel |
| `src/tools/icon.ts` | Tool `nanobanana_icon` — génération d'icônes multi-tailles avec prompt engineering spécialisé |
| `src/tools/diagram.ts` | Tool `nanobanana_diagram` — diagrammes techniques (flowchart, architecture, DB, sequence, mindmap) |

## Config

| Fichier | Rôle |
|---------|------|
| `package.json` | NPM config — scripts `build`, `dev`, `start`, `clean` |
| `tsconfig.json` | TypeScript strict, ES2022, Node16 modules, sourcemaps + declarations |
| `.gitignore` | Ignore standard Node.js (node_modules, dist, .env, logs) |
| `README.md` | Documentation utilisateur : features, setup, config MCP, exemples d'utilisation |

## Skill

| Fichier | Rôle |
|---------|------|
| `skills/nanobanana/SKILL.md` | Skill — guide complet d'utilisation des 4 tools avec paramètres, défauts, styles, exemples (222 lignes) |
