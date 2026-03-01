# Patterns — Nano Banana MCP Server

> Dernière mise à jour : 2026-03-01 (session 2)

## Conventions de nommage

- **Fichiers** : kebab-case implicite (noms courts en minuscules : `gemini.ts`, `generate.ts`)
- **Tools MCP** : préfixe `nanobanana_` + verbe (`nanobanana_generate`, `nanobanana_edit`, `nanobanana_icon`, `nanobanana_diagram`)
- **Fonctions** : camelCase (`registerGenerateTool`, `buildDiagramPrompt`, `generateFilename`)
- **Types** : PascalCase (`GenerateOptions`, `GenerateResult`, `DiagramInput`)
- **Constants** : SCREAMING_SNAKE_CASE (`DEFAULT_MODEL`, `ASPECT_RATIOS`, `DIAGRAM_TYPES`)
- **Fichiers générés** : `{prefix}_{slug}_{timestamp}.png` (ex: `nanobanana_watercolor_fox_1709283600000.png`)

## Patterns architecturaux

- **Register pattern** : chaque tool exporte une fonction `register*Tool(server: McpServer)` appelée dans `index.ts`
- **Singleton client** : `getClient()` dans `gemini.ts` avec lazy init et variable module-level `_client`
- **Service layer** : séparation stricte entre la logique MCP (tools/) et l'API (services/)
- **Prompt engineering** : les tools `icon` et `diagram` enrichissent le prompt utilisateur avec des guidelines par type/style/complexité via des dictionnaires `Record<string, string>`
- **Skill pattern** : fichier `SKILL.md` avec frontmatter YAML (`name`, `description`, `user-invocable`) + documentation Markdown structurée par tool, placé dans `skills/{nom}/SKILL.md`

## Patterns de code récurrents

### Validation des inputs
- Tous les tools utilisent des schémas **Zod** avec `.describe()` pour la documentation MCP
- Les enums sont définis comme `as const` arrays dans `constants.ts` puis référencés par `z.enum()`

### Error handling
- Chaque tool handler wrappé dans un `try/catch`
- En cas d'erreur : retourne `{ isError: true, content: [{ type: "text", text: ... }] }`
- Le service Gemini lance des `Error` explicites (image manquante, réponse vide, filtre de sécurité)

### Réponses MCP
- Format consistant : emoji + label + valeur, joints par `\n`
- Les notes du modèle (`textResponse`) sont ajoutées conditionnellement

### Annotations MCP
- Toutes identiques sur les 4 tools : `readOnlyHint: false`, `destructiveHint: false`, `idempotentHint: false`, `openWorldHint: true`

## Style de code

- **Séparateurs visuels** : commentaires `// ─── Section Name ───...` pour délimiter les blocs
- **JSDoc** : header de fichier avec description courte
- **Imports** : extensions `.js` explicites (requis par Node16 module resolution)
- **Types** : TypeScript strict, types inférés via `z.infer<typeof Schema>`

## Tests

- Aucun framework de test configuré pour le moment
- Pas de tests unitaires ou d'intégration existants

## Scripts NPM

| Script | Commande |
|--------|----------|
| `dev` | `tsx watch src/index.ts` |
| `build` | `tsc` |
| `start` | `node dist/index.js` |
| `clean` | `rm -rf dist` |
