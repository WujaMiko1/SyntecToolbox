
# SyntecToolbox

Zestaw narzędzi do analizy i diagnostyki sterowników CNC Syntec.

## Narzędzia

### [Syntec Ladder Viewer](https://wujamiko1.github.io/SyntecToolbox/tools/SyntecLadderViewer.html)
Przeglądarka skompilowanych programów PLC (.lad) dla sterowników Syntec CNC.

**Funkcje:**
- Dekodowanie binarnego formatu .lad (Big5 pair encoding, sekcje, rejestry)
- Tłumaczenie chińskich etykiet sekcji na polski
- 12 zakładek: Mapa, Przegląd, Sekcje, Struktura, Diagram drabinkowy, Rejestry, Hex, Porównanie, I/O, System, Xref, Pomoc
- Import IoMap XML → wizualizacja modułów sprzętowych (RIO, HK, M3IO, RFHHB, LIO)
- I/O Cabinet Tracer — mapowanie adresów logicznych na fizyczne porty w szafie sterowniczej
- Automatyczny cross-reference: Sekcje PLC → fizyczne I/O
- Globalne wyszukiwanie (Ctrl+K), adnotacje do sekcji, eksport JSON
- Porównanie dwóch plików .lad obok siebie

## Technologia
- Czysty HTML/CSS/JS — zero zależności, działa offline w przeglądarce
- Hosting: GitHub Pages

## Kontekst projektu
Narzędzia powstały do pracy z maszynami CNC sterowanymi kontrolerami Syntec (seria 6/7). Typowa struktura plików:
- `.lad` — skompilowany program drabinkowy PLC (binary)
- `IoMap_*.xml` — konfiguracja mapowania I/O (logiczne ↔ fizyczne adresy)
- Rejestry: R (relay), X (input), Y (output), T (timer), C (counter), D (data), K (constant), S (system), A (alarm), M (M-code)
- Moduły I/O: RIO (Remote I/O), HK (HandKit), M3IO (M3 Serial), RFHHB (RF Handwheel), LIO (Local I/O), MPG

## Dokumentacja
Szczegółowa dokumentacja: [docs/ladder-viewer.md](docs/ladder-viewer.md)

## Licencja
**WujaMiko1** © 2025-2026
