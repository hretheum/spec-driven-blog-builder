# spec-driven-blog-builder

Prowadzi całą budowę bloga dla klienta od zera do wdrożenia metodą spec-driven (specyfikacja → plan → implementacja fazami → CMS przez MCP → QA → deploy → instrukcja dla klienta), zamiast vibe codingu.

## Co to jest

To jest **Claude Skill** — zestaw instrukcji (`SKILL.md` + `assets/` + `references/`), który rozszerza możliwości Claude'a o poprowadzenie użytkownika przez cały, ustrukturyzowany proces budowy bloga.

## Instalacja

Skill instalujesz przez rozpakowanie zawartości repozytorium do jednej z lokalizacji:

- **`~/.claude/skills/`** — instalacja globalna dla Claude Code, skill dostępny we wszystkich projektach.
- **`.claude/skills/`** w konkretnym projekcie — skill dostępny tylko w obrębie tego projektu.

Po instalacji katalog powinien wyglądać tak, np. globalnie:

```
~/.claude/skills/spec-driven-blog-builder/
├── SKILL.md
├── assets/
└── references/
```
