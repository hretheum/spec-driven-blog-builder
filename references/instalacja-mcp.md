# Instalacja MCP dla wybranego wariantu CMS

Ten plik czytaj w Etapie 4 skilla (podłączenie MCP jako CMS), przed napisaniem kodu integracji. Instrukcje różnią się dla Claude Desktop i Claude Code — sprawdź w czym pracuje użytkownik.

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
