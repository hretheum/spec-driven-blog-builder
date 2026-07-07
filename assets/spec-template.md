# SPEC: Blog dla [Klient]

## Cel
[1-2 zdania: branża, ton, główny cel bloga]

## Stack i CMS
- Stack: Next.js (App Router) + Markdown/MDX
- CMS: [Notion MCP | Filesystem MCP]
- Hosting: [Vercel | Netlify]

## Strony
- / (lista postów[, paginacja co N])
- /blog/[slug] (pojedynczy post)
- /tag/[tag] (lista po tagu)
- [dodatkowe strony]

## Content model
- title
- slug
- date
- excerpt
- cover_image
- tags[]
- body (markdown/MDX)
[dodaj/usuń pola wg potrzeb]

## Non-goals (świadomie pomijamy w v1)
- Komentarze
- Wyszukiwarka
- Wielojęzyczność
- Panel admina poza CMS-em
[dostosuj do konkretnego projektu]

## Definition of Done
- [ ] Build przechodzi bez błędów
- [ ] Lighthouse Performance > 90
- [ ] Wszystkie strony z SPEC działają
- [ ] Dodanie nowego posta w CMS pojawia się na stronie po rebuildzie
[dodaj punkty specyficzne dla klienta]
