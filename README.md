# spec-driven-blog-builder

Prowadzi całą budowę bloga dla klienta od zera do wdrożenia metodą spec-driven (specyfikacja → plan → implementacja fazami → CMS przez MCP → QA → deploy → instrukcja dla klienta), zamiast vibe codingu.

## Co to jest

To jest **Claude Skill** dostarczany jako **plugin** przez marketplace `eryk-skills`. Zestaw instrukcji (`SKILL.md` + `assets/` + `references/`) rozszerza możliwości Claude'a o poprowadzenie użytkownika przez cały, ustrukturyzowany proces budowy bloga.

## Instalacja przez marketplace (zalecana)

W Claude Code uruchom po kolei:

```
/plugin marketplace add hretheum/spec-driven-blog-builder
/plugin install spec-driven-blog-builder@eryk-skills
```

Pierwsza komenda dodaje ten marketplace (`eryk-skills`) z repozytorium na GitHubie, druga instaluje z niego plugin `spec-driven-blog-builder`.

## Instalacja ręczna (alternatywnie)

Zamiast marketplace możesz skopiować sam katalog skilla do jednej z lokalizacji:

- **`~/.claude/skills/`** — instalacja globalna dla Claude Code, skill dostępny we wszystkich projektach.
- **`.claude/skills/`** w konkretnym projekcie — skill dostępny tylko w obrębie tego projektu.

Skopiuj do niej katalog `plugins/spec-driven-blog-builder/skills/spec-driven-blog-builder/`, tak aby docelowo wyglądało to np. globalnie:

```
~/.claude/skills/spec-driven-blog-builder/
├── SKILL.md
├── assets/
└── references/
```

## Struktura repozytorium

```
.
├── .claude-plugin/
│   └── marketplace.json              # marketplace "eryk-skills"
├── plugins/
│   └── spec-driven-blog-builder/
│       ├── .claude-plugin/
│       │   └── plugin.json           # manifest pluginu
│       └── skills/
│           └── spec-driven-blog-builder/
│               ├── SKILL.md
│               ├── assets/
│               └── references/
└── README.md
```
