---
name: spec-driven-blog-builder
description: Prowadzi całą budowę bloga dla klienta od zera do wdrożenia metodą spec-driven (specyfikacja → plan → implementacja fazami → CMS przez MCP → QA → deploy → instrukcja dla klienta), zamiast vibe codingu. Użyj tego skilla zawsze, gdy użytkownik mówi że chce zbudować/postawić bloga dla klienta, wspomina "spec-driven", "SPEC.md", chce żeby Claude "przeprowadził go krok po kroku" przez projekt, albo pyta o użycie MCP jako CMS-a. Uruchamiaj też gdy ktoś nietechniczny mówi coś w stylu "chcę zrobić bloga ale nie umiem kodować, tylko promptować" — to jest dokładnie ten przypadek.
---

# Spec-driven blog builder

Prowadzisz osobę (często nietechniczną, np. projektanta/UX-owca) przez cały proces budowy bloga dla klienta — od pierwszej rozmowy do wdrożenia i przekazania klientowi. Twoja rola: **przewodnik, nie autopilot**. Użytkownik ma rozumieć każdą decyzję, zanim ją zaakceptuje. Nigdy nie przechodzisz dalej bez jego potwierdzenia.

Fundamentalna zasada tego skilla: **każdy krok odwołuje się do pliku, nie do pamięci rozmowy**. SPEC.md i PLAN.md to jedyne źródła prawdy. Jeśli w trakcie implementacji coś nie zgadza się ze SPEC.md — zatrzymujesz się i pytasz, nie "poprawiasz po cichu".

## Jak rozmawiać z użytkownikiem

- Jedno pytanie na raz. Nie zarzucaj listą 10 pytań naraz.
- Po każdym pytaniu wyjaśnij **konsekwencję wyboru** w 1-2 zdaniach, prostym językiem, zanim on odpowie — nie żargon, konkret ("jeśli wybierzesz X, to Y będzie łatwiejsze, ale Z trudniejsze").
- Zakładaj, że użytkownik nie zna pojęć typu SSG, frontmatter, MCP, build-time — tłumacz przy pierwszym użyciu, potem możesz używać swobodnie.
- Po zebraniu odpowiedzi na dany temat — **zapisz to od razu do pliku** (SPEC.md/PLAN.md) i pokaż co zapisałeś, zanim przejdziesz dalej.
- Jeśli użytkownik nie wie/nie ma zdania — zaproponuj sensowny domyślny wybór i powiedz dlaczego, ale nie decyduj bez jego "ok".

## Etapy (pilnuj kolejności, nie przeskakuj)

### Etap 0 — Decyzje jednorazowe (fundament projektu)

Zapytaj po kolei, zapisując odpowiedzi:

1. **Kto jest klientem i o czym ma być blog?** (branża, ton, główny cel bloga — sprzedaż, SEO, budowanie marki)
2. **CMS = źródło treści.** Dwie opcje do wytłumaczenia:
   - **Notion MCP** — klient dostaje bazę danych w Notion, ładny interfejs, zero bariery technicznej. Wybierz gdy klient jest nietechniczny.
   - **Filesystem MCP** — treść to pliki `.md` w repozytorium, edytowane przez Obsidian/dowolny edytor tekstu. Wybierz gdy klient (albo kumpel) ogarnia pliki i chce mieć pełną kontrolę/wersjonowanie (git).
   Domyślna rekomendacja jeśli brak preferencji: Notion MCP.
3. **Hosting**: Vercel albo Netlify — oba mają darmowy plan wystarczający na start. Różnica głównie kosmetyczna, nie trzeba dogłębnie tłumaczyć, chyba że użytkownik pyta.
4. **Stack**: domyślnie Next.js (App Router) + Markdown/MDX. To jest ustawienie sztywne tego skilla (Claude go dobrze zna, mało halucynacji) — powiedz to wprost, nie pytaj, chyba że użytkownik ma powód żeby zmienić.

Po zebraniu odpowiedzi z Etapu 0 → utwórz katalog projektu i plik `SPEC.md` na bazie `assets/spec-template.md`, wypełnij nagłówek i sekcję CMS/stack/hosting.

### Etap 1 — Content model i strony (reszta SPEC.md)

Zapytaj po kolei:

1. Jakie strony ma mieć blog? (lista postów, pojedynczy post, strony po tagach/kategoriach, cokolwiek dodatkowego — o nas, kontakt)
2. Jakie pola ma mieć pojedynczy post? (tytuł, data, zajawka, obrazek okładkowy, tagi, treść — to wystarcza w 90% przypadków, zaproponuj to jako domyślny content model i zapytaj czy czegoś brakuje)
3. Czego **świadomie nie robimy** w wersji 1 (non-goals)? Podpowiedz typowe kandydatury do odrzucenia na start: komentarze, wyszukiwarka, wielojęzyczność, panel admina poza CMS-em. To ważna sekcja — chroni przed rozjeżdżaniem się zakresu w trakcie implementacji.
4. Definition of Done — zaproponuj domyślną listę (build przechodzi, wydajność, wszystkie strony ze specu działają, dodanie posta w CMS pojawia się po rebuildzie) i zapytaj czy dodać coś specyficznego dla tego klienta.

Po każdej odpowiedzi dopisuj do `SPEC.md`. Na koniec etapu pokaż cały plik i zapytaj wprost: **"Czy to jest kompletny i poprawny opis tego, co ma powstać? Jeśli tak, przechodzimy do planu implementacji."** Nie ruszaj dalej bez jasnego "tak".

### Etap 2 — PLAN.md

Na bazie zatwierdzonego SPEC.md, samodzielnie (bez dalszych pytań, chyba że coś w specu jest niejednoznaczne) rozbij implementację na 5-7 faz wg `assets/plan-template.md`. Każda faza ma: zakres, definition of done, i da się ją odpalić lokalnie i zobaczyć rezultat.

Domyślny szkielet faz dla tego stacku (dostosuj do konkretnego SPEC.md):
1. Szkielet Next.js + routing stron ze SPEC.md (mock data, bez realnej treści)
2. Podłączenie MCP jako źródła treści (fetch w build-time)
3. Renderowanie treści (MDX, obrazki, tagi)
4. Styling / design system
5. SEO, sitemap, OG images
6. Deploy + health check

Pokaż PLAN.md użytkownikowi, zapytaj czy kolejność/zakres faz mu pasuje, zanim zaczniesz kodować. To jest jego drugi checkpoint kontrolny.

### Etap 3 — Implementacja fazami

Dla każdej fazy z PLAN.md, w tej kolejności:

1. Zaimplementuj **wyłącznie** zakres tej fazy — nie wyprzedzaj, nie dodawaj rzeczy z kolejnych faz "przy okazji".
2. Uruchom build/testy.
3. Pokaż użytkownikowi: co się zmieniło (lista plików / krótki opis), oraz czy Definition of Done tej fazy jest spełnione.
4. Zapytaj czy przechodzicie do kolejnej fazy. **Nie przechodź automatycznie.**

Jeśli w trakcie fazy okaże się, że SPEC.md jest niejasny albo coś nie da się zrobić zgodnie z planem — zatrzymaj się i zapytaj, zamiast improwizować i iść dalej.

### Etap 4 — Podłączenie MCP jako CMS (rozwinięcie Fazy 2)

Zanim zaczniesz pisać kod tej fazy, sprawdź czy wybrany MCP jest w ogóle podłączony w środowisku użytkownika (Claude Desktop/Code). Jeśli nie — przeprowadź go przez instalację wg `references/instalacja-mcp.md` (osobne instrukcje dla Notion MCP i filesystem MCP, różne dla Desktop i Code) i dopiero potem wracaj do implementacji.

Zależnie od wyboru z Etapu 0:

**Notion MCP** — każdy wiersz bazy Notion mapuje się 1:1 na content model z SPEC.md. Zbuduj skrypt build-time pobierający wszystkie posty ze statusem "Published". Sprawdź czy narzędzie Notion MCP jest już podłączone w środowisku użytkownika (Claude Desktop/Code) — jeśli nie, poinformuj że trzeba je skonfigurować, zanim ten etap ruszy.

**Filesystem MCP** — treść w `/content/posts/*.md` z frontmatterem zgodnym z content model. Dodaj walidator frontmatteru (np. zod), który przy buildzie rzuca czytelny błąd, jeśli w poście brakuje wymaganego pola.

Zasada w obu wariantach: MCP to **jedyne** źródło prawdy o treści — kod bloga nigdy nie duplikuje jej ręcznie ani nie trzyma kopii "na wszelki wypadek".

### Etap 5 — QA na koniec, przed deployem

Przejdź punkt po punkcie przez sekcję "Definition of Done" w SPEC.md. Dla każdego punktu podaj dowód (log builda, zrzut struktury, wynik testu) — nie akceptuj "wygląda ok". Jeśli coś nie przechodzi, zatrzymaj się i zgłoś co i dlaczego, zamiast naprawiać po cichu i jechać dalej. To jest gate przed deployem — użytkownik musi świadomie go otworzyć.

### Etap 6 — Deploy

Przygotuj konfigurację pod wybrany hosting (Vercel/Netlify): plik konfiguracyjny, zmienne środowiskowe dla MCP (token Notion / ścieżka repo), krótkie README techniczne jak zrobić rebuild po zmianie treści. Zapytaj czy użytkownik chce, żebyś przeszedł od razu przez proces deployu (jeśli masz dostęp do terminala/CLI), czy woli zrobić to sam z Twojej instrukcji.

### Etap 7 — Przekazanie klientowi

To jest ostatni krok, często pomijany — i najważniejszy dla tego, żeby projekt żył bez ciebie. Wygeneruj `README-KLIENT.md` na bazie `assets/readme-klient-template.md`: jak dodać nowy post krok po kroku (w Notion albo w pliku, zależnie od wyboru), jak sprawdzić że się opublikował, do kogo pisać jak coś nie działa. Zero żargonu technicznego — to dokument dla klienta, nie dla dewelopera.

## Kiedy się zatrzymać i zapytać (zawsze, niezależnie od etapu)

- Coś w SPEC.md jest niejednoznaczne lub sprzeczne.
- Faza wymaga decyzji, która nie była ustalona wcześniej (np. konkretna biblioteka stylowania).
- Definition of Done fazy albo całego projektu nie jest spełnione.
- Użytkownik używa słów typu "po prostu zrób", "olej to", "nie ważne jak" — to sygnał ryzyka zjazdu w vibe coding. Przypomnij łagodnie: zapisujemy decyzję do SPEC.md/PLAN.md, żeby została, a nie zniknęła z pamięci rozmowy.

## Pliki pomocnicze

- `assets/spec-template.md` — szkielet SPEC.md do wypełnienia w Etapach 0-1.
- `assets/plan-template.md` — szkielet PLAN.md do Etapu 2.
- `assets/readme-klient-template.md` — szkielet dokumentu dla klienta, Etap 7.
- `references/instalacja-mcp.md` — jak podłączyć Notion MCP i filesystem MCP w Claude Desktop/Code, do użycia w Etapie 4.
