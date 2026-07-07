# Instalacja MCP dla wybranego wariantu CMS

Ten plik czytaj w Etapie wstępnym (zapis plików) i w Etapie 4 skilla (podłączenie MCP jako CMS), przed napisaniem kodu integracji. Instrukcje różnią się dla Claude Desktop i Claude Code — sprawdź w czym pracuje użytkownik.

## Desktop Commander (wymagany do zapisu plików w Claude Desktop)

Bez tego MCP-a Claude Desktop nie zapisuje plików na dysku — pokazuje tylko podgląd (artifact) w oknie rozmowy, który trzeba ręcznie eksportować. Claude Code tego nie potrzebuje (ma natywny zapis).

### Claude Desktop — najprostsza ścieżka (one-click)
1. W oknie czatu kliknij "+" → **Connectors** → **Browse Connectors**.
2. Wyszukaj "Desktop Commander".
3. Zainstaluj jednym kliknięciem.
4. Zrestartuj Claude Desktop.

### Claude Desktop — ścieżka alternatywna (terminal)
```
npx @wonderwhy-er/desktop-commander@latest setup
```
Ten komenda sama zapisuje wpis do configu Claude Desktop (macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`, Windows: `%APPDATA%\Claude\claude_desktop_config.json`, Linux: `~/.config/Claude/claude_desktop_config.json`) — nie trzeba edytować pliku ręcznie. Zrestartuj aplikację po zakończeniu.

### Claude Code
Zwykle niepotrzebne — Code ma natywne narzędzia do zapisu plików (Write/Edit) i nie wymaga Desktop Commandera do tego celu. Warto go dodać tylko jeśli potrzebujesz dodatkowych możliwości, których Code nie ma natywnie (długo działające procesy interaktywne, odczyt PDF/Excel przez `read_file`):
```
claude mcp add --scope user desktop-commander -- npx -y @wonderwhy-er/desktop-commander@latest
```

### Test po instalacji
Poproś Claude o utworzenie pustego pliku testowego w katalogu roboczym (np. `test.md`) i potwierdzenie że plik faktycznie istnieje na dysku (nie tylko w podglądzie rozmowy). Dopiero po pozytywnym teście przechodź do dalszych etapów.

## Wariant: Notion MCP

Notion MCP to serwer **zdalny** (hostowany przez Notion) — instalacja to głównie autoryzacja, nie edycja plików konfiguracyjnych.

### Claude Desktop
1. Ustawienia → **Connectors**.
2. Dodaj Notion (jeśli jest na liście gotowych) albo "Add custom connector" z URL: `https://mcp.notion.com/mcp`.
3. Przejdź przez proces logowania (OAuth) — autoryzujesz konkretny workspace Notion.
4. Wymaga planu Pro/Max/Team/Enterprise.

### Claude Code
1. W terminalu w katalogu projektu:
   ```
   claude mcp add --transport http notion https://mcp.notion.com/mcp
   ```
2. Uruchom `/mcp` wewnątrz sesji Claude Code, żeby przejść proces logowania (OAuth).
3. Alternatywa: oficjalny plugin Notion (`claude plugin install notion@claude-plugins-official` — nazwa może się zmienić, sprawdź w marketplace), który dodaje serwer razem z gotowymi Skillami do typowych operacji w Notion.

### Po połączeniu
- Sprawdź w Claude czy narzędzia Notion faktycznie się pojawiły (np. komenda `/mcp` w Code, albo ikonka narzędzi w Desktop), zanim zaczniesz pisać kod, który na nich polega.
- Uwaga: dostęp działa tylko do stron/baz, które użytkownik jawnie udostępnił integracji w Notion (Share → dodaj integrację do konkretnej strony/bazy).

## Wariant: Filesystem MCP

Filesystem MCP to serwer **lokalny** (stdio, uruchamiany na tej samej maszynie).

### Claude Code
Zwykle **niepotrzebny** — Claude Code ma natywny dostęp do plików w bieżącym katalogu projektu i tak działa. Sięgaj po dedykowany filesystem MCP tylko jeśli chcesz dać dostęp do folderu *poza* katalogiem projektu (np. treści trzymane gdzie indziej na dysku).

### Claude Desktop
Wymaga edycji pliku konfiguracyjnego (nie ma UI jak przy Connectors, bo to serwer lokalny):
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

Przykładowy wpis (dostosuj ścieżkę do faktycznego folderu z treścią):
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/pełna/ścieżka/do/content"]
    }
  }
}
```
Zapisz plik, zamknij i uruchom ponownie Claude Desktop.

## Checklist przed przejściem do dalszej implementacji

- [ ] Wybrany MCP widoczny jako podłączony/aktywny w Claude
- [ ] Zrobiony test: proste zapytanie do MCP (np. "pokaż listę stron/plików") działa
- [ ] Uprawnienia ograniczone do potrzebnego zakresu (Notion: tylko udostępnione strony; filesystem: tylko folder treści, nie cały dysk)

Jeśli którykolwiek punkt nie jest spełniony — zatrzymaj się i pomóż użytkownikowi to naprawić, zanim zaczniesz pisać kod Fazy 2 z PLAN.md.
