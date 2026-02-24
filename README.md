# i18n-nextjs

A Claude Code skill that provides a comprehensive, production-ready guide for implementing internationalization (i18n) in Next.js App Router projects. Covers everything from translation file structure to multi-language SEO, hreflang, JSON-LD structured data, and sitemap generation.

---

## What This Skill Does

When active, this skill gives Claude deep, structured knowledge about i18n best practices for Next.js. Instead of giving generic advice, Claude will follow concrete patterns for:

- Structuring and using JSON translation dictionaries
- Locale routing via Next.js middleware (clean URLs for default locale)
- Generating locale-aware SEO metadata with full hreflang support
- Adding structured JSON-LD data with translated fields
- Building a correctly structured multi-language sitemap

---

## Installation

This is a [Claude Code](https://claude.ai/code) skill. To install it, copy the skill folder into your Claude skills directory:

```bash
# Clone the repository
git clone https://github.com/javainthinking/i18n-nextjs.git

# Move to your Claude skills directory
cp -r i18n-nextjs ~/.claude/skills/i18n-nextjs
```

Claude Code will automatically detect and load it on the next session.

---

## Skill Structure

```
i18n-nextjs/
├── SKILL.md                        ← Core guide: principles, structure, quick checklists
└── references/
    ├── translation-files.md        ← JSON key conventions, getDictionary, useDictionary hook
    ├── routing.md                  ← Middleware, URL patterns, LocalizedLink, useLocale
    ├── seo-metadata.md             ← generateMetadata, alternates, hreflang, OpenGraph
    ├── structured-data.md          ← JSON-LD schemas: WebApp, BlogPosting, FAQ, Breadcrumb
    └── sitemap.md                  ← i18n sitemap structure, XML format, Next.js sitemap.ts
```

The skill uses **progressive disclosure** — `SKILL.md` is a concise index that Claude always reads when triggered; detailed reference files are loaded only as needed, keeping context efficient.

---

## What's Covered

### Translation Files
- JSON key hierarchy: `page.section.key` convention
- Server-side `getDictionary(locale)` async helper
- Client-side `useDictionary()` hook with caching
- Template variable substitution (`{count}` pattern)
- Fallback strategy for missing keys

### Locale Routing & Middleware
- Next.js `middleware.ts` for locale detection via `Accept-Language` header
- Clean URL convention: default locale has no prefix (`/products`), others do (`/es/products`)
- Redirect `/en/*` → `/*` to enforce clean URLs
- `LocalizedLink` component that auto-prefixes locale
- `useLocale()` hook (URL → pathname → localStorage → default)
- `getLocalizedPath()` / `removeLocalePrefix()` utilities

### SEO Metadata
- `generateMetadata()` pattern per page/layout
- `generateAlternatesMetadata()` utility generating all locale URLs
- Full hreflang `<link rel="alternate">` output for all locales + `x-default`
- OpenGraph `locale` and `alternateLocale` fields
- Canonical URL rules (no `/en` prefix for default locale)
- `html lang` attribute with `LangSetter` client component for SPA navigation

### Structured JSON-LD Data
- **WebApplication** schema with translated `featureList`, `description`, `offers`
- **BlogPosting** schema with `inLanguage` field
- **FAQPage** schema with translated `acceptedAnswer` entries
- **BreadcrumbList** schema with localized item URLs
- **HowTo** schema with translated steps
- Placement patterns: `<Script>` in layouts, `<script>` in page components

### Multi-language Sitemap
- Core structure: one `<url>` entry per page, with `<xhtml:link>` hreflang alternates for every locale
- `<loc>` always uses the default-locale (clean) URL; `x-default` points there too
- Full XML example covering home, static pages, and dynamic pages (e.g. blog posts)
- Next.js App Router `sitemap.ts` implementation (recommended)
- Custom script approach for non-App Router or more control
- What to include vs. exclude (admin, API, auth routes excluded)
- IETF language tag format rules (`en`, `zh-CN`, `pt-BR`, `x-default`)

---

## Quick Reference

### Adding a new feature with i18n

1. Add translation keys to all locale JSON files — English first, then natural translations
2. **Server components**: `const dict = await getDictionary(locale)` → `dict?.page?.section?.key || 'fallback'`
3. **Client components**: `const dict = useDictionary()` → same fallback pattern
4. Add `generateMetadata()` to the page, calling `generateAlternatesMetadata()`
5. Add JSON-LD structured data with translated fields and `inLanguage`
6. Add the page to the sitemap source if it's new

### Adding a new locale

1. Add the locale code to `locales` in `src/lib/i18n/locales.ts`
2. Create `<code>.json` in the dictionaries folder (full translation of `en.json`)
3. Register it in the `dictionaries.ts` import map
4. Add the display name in the `LanguageSwitcher` component
5. Sync the sitemap locale list and regenerate

---

## Example Trigger Phrases

This skill activates when you ask Claude things like:

- "Add i18n support to this page"
- "Translate this component to Spanish"
- "Add SEO metadata for all locales"
- "How should I implement hreflang?"
- "Add a new language to the app"
- "Generate the sitemap for multiple languages"
- "What's the right way to structure translation files?"

---

## Design Principles

- **No hardcoded strings** — all user-facing text goes through the translation dictionary
- **Natural, idiomatic translations** — never machine-translate; quality matches English copywriting
- **Clean default-locale URLs** — `/products` not `/en/products`
- **Consistent alternates** — every locale page lists all the same hreflang entries (Google requirement)
- **Full SEO coverage** — metadata, OpenGraph, JSON-LD, and sitemap all locale-aware

---

## Requirements

- Next.js 13+ with App Router
- `@formatjs/intl-localematcher` and `negotiator` (for middleware locale detection)

---

## License

MIT
