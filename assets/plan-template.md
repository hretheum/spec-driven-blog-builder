# PLAN implementacji (na bazie SPEC.md)

## Faza 1 — Szkielet
Zakres: routing stron ze SPEC.md, mock data, bez realnej treści.
Definition of Done:
- [ ] Wszystkie strony ze SPEC.md renderują się lokalnie
- [ ] Nawigacja między nimi działa

## Faza 2 — Podłączenie CMS (MCP)
Zakres: fetch treści z [Notion | filesystem] w build-time wg content model.
Definition of Done:
- [ ] Realne posty z CMS pojawiają się na stronie
- [ ] Walidacja brakujących pól działa i daje czytelny błąd

## Faza 3 — Renderowanie treści
Zakres: MDX, obrazki, tagi, formatowanie.
Definition of Done:
- [ ] Treść z CMS renderuje się poprawnie ze wszystkimi elementami (obrazki, listy, linki)

## Faza 4 — Styling / design system
Zakres: układ wizualny zgodny z ustaleniami.
Definition of Done:
- [ ] Blog wygląda spójnie na desktopie i mobile

## Faza 5 — SEO
Zakres: meta tagi, sitemap, OG images.
Definition of Done:
- [ ] Sitemap generuje się poprawnie
- [ ] Podgląd linku w social media pokazuje właściwy tytuł/obrazek

## Faza 6 — Deploy + health check
Zakres: konfiguracja hostingu, zmienne środowiskowe, pierwszy deploy.
Definition of Done:
- [ ] Strona działa pod docelowym adresem
- [ ] Rebuild po zmianie treści w CMS działa
