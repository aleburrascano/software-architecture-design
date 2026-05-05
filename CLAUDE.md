# CLAUDE.md — software-architecture-design

You maintain this personal knowledge wiki. Read this at session start, then search-first — see Session start below.

## Layers
1. `raw/` — immutable source material. Read; never modify.
2. `wiki/` — your domain. Author and maintain pages here.

## Page conventions
- Frontmatter every page: `type`, `created`, `updated`, `sources`, `tags`
- Cross-references: Obsidian wikilinks `[[Page Name]]`
- Source pages in `wiki/sources/` with `source_path`, `source_date`, `source_author`
- Never invent facts. Use `> [!question] Unverified` for uncertain claims.

## Operations

### Ingest (adding a source)
1. Read raw source fully.
2. Discuss takeaways before writing pages.
3. Create source page in `wiki/sources/`.
4. Update or create pages in `wiki/topics/` (synthesis) and `wiki/concepts/` touched.
5. Update `index.md` (one line per page: `- [[Page]] — summary`). Append `log.md` entry (`## [YYYY-MM-DD] ingest | title`).

### Query
Use `search_notes` (folder: `wiki`) first → `get_note` on top 1–3 hits → synthesize.
`wiki/topics/` = synthesis pages (start here). `wiki/sources/` = per-source detail.

### Lint (on request)
Find: orphans, contradictions, missing cross-refs, index drift. Discuss before bulk edits.

## Session start
- **Queries**: read this → `search_notes` directly → respond.
- **Ingest / lint**: read this → read `index.md` → skim tail of `log.md` → proceed.
- **Always** scope `search_notes` to `folder: "wiki"` or `folder: "raw"` — unscoped searches can hit `.quartz` noise.

## You do NOT
- Modify `raw/` (immutable).
- Delete wiki pages without confirmation.
- Fabricate sources or citations.
- Skip the log.

<!-- vaultkit:wiki-style:start -->
## Wiki Style & Refresh Policy

### Voice and structure
<describe the wiki's voice, tone, page templates, what lives in concepts/ vs topics/, naming conventions, etc.>

### Refresh constraints (patch flow)
- When applying a freshness report, edit existing wiki pages surgically. Never regenerate a wiki page from sources.
- Scope edits to pages listed under "Wiki pages that cite this source" in the report.
- For sources in the "text-only compare" or "manual review" sections: use WebFetch to retrieve and compare against the corresponding raw/<file>.md. Patch only on meaningful semantic difference; ignore formatting noise.

### Workflow
For refresh sessions, cd into this vault directory and run `claude` there. The vault's `.claude/settings.json` will set recommended defaults (model, permissions). Don't rely on the MCP connection from another cwd for refresh work.

### Recommended Claude Code settings for refresh sessions
Model: <e.g. Sonnet 4.6 or higher>
Thinking: <enabled / disabled>
Effort: <low / medium / high>
<!-- vaultkit:wiki-style:end -->
