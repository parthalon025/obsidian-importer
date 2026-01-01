# Claude Code Project Instructions

## Project Overview

Obsidian Importer - A community plugin for importing notes from various formats into Obsidian.

## Build Commands

```bash
npm install          # Install dependencies
npm run build        # Build the plugin (TypeScript + esbuild)
npm run dev          # Development build with watch mode
```

## Key Files

| File | Purpose |
|------|---------|
| `src/main.ts` | Plugin entry point, ImportContext, UI |
| `src/format-importer.ts` | Base class for all importers |
| `src/formats/notion-api.ts` | Notion API importer with delta sync |
| `src/formats/notion-api/api-helpers.ts` | API utilities, metadataCache helpers |

## Notion API Importer Features

### Delta Sync (Incremental Import)
- Tracks `notion-id`, `notion-created`, `notion-updated` in frontmatter
- Uses Obsidian's `metadataCache` for efficient frontmatter access
- Skips unchanged pages, updates only modified ones
- Fallback to regex parsing when cache unavailable

### Key Functions
- `extractNotionTimestamps()` - Hybrid cache+regex timestamp extraction
- `shouldSkipOrUpdateFile()` - Determines skip/update/create action
- `extractFrontMatter()` - Conditional timestamp writing based on incrementalImport flag

## Code Standards

- Follow [Obsidian Plugin Guidelines](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines)
- TypeScript only
- Minimize dependencies
- Performance-minded (supports 100k+ note vaults)
- Avoid concurrency to prevent memory issues
- Use `metadataCache` for frontmatter access per Obsidian best practices

## Testing

Manual testing in Obsidian:
1. Fresh import with `incrementalImport=true` → timestamps added
2. Fresh import with `incrementalImport=false` → no timestamps
3. Re-import unchanged pages → skipped
4. Re-import modified pages → updated
5. Legacy files (no timestamps) → skipped gracefully

## References

- [Obsidian Developer Docs](https://docs.obsidian.md/)
- [MetadataCache API](https://docs.obsidian.md/Reference/TypeScript+API/MetadataCache)
- [Notion API Reference](https://developers.notion.com/reference)
