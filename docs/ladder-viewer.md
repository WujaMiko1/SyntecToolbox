# ⛓ Syntec Ladder Viewer v4.0 — Dokumentacja

## Przegląd

Ladder Viewer to wizualna przeglądarka skompilowanych programów PLC (`.lad`) dla sterowników Syntec CNC. Dekoduje binarny format pliku, tłumaczy chińskie etykiety na polski i prezentuje program w 12 zakładkach.

## Jak wczytać plik

1. **Przeciągnij i upuść** plik `.lad` na strefę drop
2. **Kliknij przycisk** "📂 Wczytaj .lad" i wybierz plik(i)
3. **Wczytaj folder** "📁 Wczytaj folder Ladder" — wczyta wszystkie `.lad` z katalogu

Można wczytać wiele plików jednocześnie — przełączaj między nimi w selektorze.

## Zakładki

| # | Zakładka | Skrót | Opis |
|---|----------|-------|------|
| 1 | 🗺 Mapa | Ctrl+1 | Wizualna mapa programu — treemap rozmiarów, sekcje wg kategorii |
| 2 | 📊 Przegląd | Ctrl+2 | Nagłówek pliku, statystyki, rozkład bajtów, kategorie |
| 3 | 📑 Sekcje | Ctrl+3 | Tabela wszystkich sekcji z tłumaczeniami, kopiowanie |
| 4 | 🔀 Struktura | Ctrl+4 | Sekcje pogrupowane wg funkcji, wizualny flow |
| 5 | ⛓ Diagram | Ctrl+5 | Diagram drabinkowy — próba dekodowania instrukcji |
| 6 | 🔧 Rejestry | Ctrl+6 | Wszystkie rejestry PLC pogrupowane wg typu |
| 7 | 🔍 Hex | Ctrl+7 | Widok heksadecymalny z kolorowaniem |
| 8 | ⚖ Porównanie | Ctrl+8 | Porównanie dwóch plików .lad |
| 9 | 🔌 I/O | Ctrl+9 | Mapa wejść/wyjść + import IoMap XML |
| 10 | 🏗 System | Ctrl+0 | Mapa zależności systemowych CNC |
| 11 | 🔗 Xref | — | Cross-reference: rejestr ↔ sekcje |
| 12 | ❓ Pomoc | — | Instrukcja PLC, typy rejestrów, skróty, ścieżki |

## Skróty klawiszowe

| Skrót | Akcja |
|-------|-------|
| `Ctrl+K` | Globalne wyszukiwanie |
| `Ctrl+E` | Eksport JSON |
| `Ctrl+1` do `Ctrl+0` | Przełączanie zakładek |
| `Esc` | Zamknij wyszukiwanie |
| `↑` `↓` | Nawigacja w wynikach |
| `Enter` | Wybierz wynik |

## Import IoMap XML

1. Wczytaj plik `.lad`
2. Kliknij przycisk "🔌 IoMap XML" w pasku narzędzi
3. Wybierz plik `IoMap_*.xml` z `OpenCNC/Data/IO/`
4. Dane sprzętowe pojawią się w zakładce I/O

## Notatki do sekcji

- Kliknij sekcję w sidebarze
- Wpisz notatkę w polu tekstowym
- Notatka zapisze się automatycznie (localStorage)
- Ikona 📝 widoczna przy sekcjach z notatkami

## Typy rejestrów PLC

| Typ | Nazwa | Zachowanie | Zastosowanie |
|-----|-------|-----------|-------------|
| R | Relay | Traci stan po wyłączeniu | Zmienne tymczasowe, flagi |
| K | Keep | Zachowuje stan | Stany trwałe, konfiguracja |
| D | Data | 16/32-bit wartość | Prędkości, czasy, pozycje |
| T | Timer | Odmierzanie czasu | Opóźnienia, timeouty |
| C | Counter | Zliczanie impulsów | Cykle, narzędzia, detale |
| M | Marker | Flagi systemowe | M-kody, statusy, alarmy |

## Instrukcje PLC

| Kod | Symbol | Opis |
|-----|--------|------|
| LD | `--[ ]--` | Kontakt normalnie otwarty (NO) |
| LDI | `--[/]--` | Kontakt normalnie zamknięty (NC) |
| OUT | `--( )--` | Cewka wyjściowa |
| SET | `--(S)--` | Ustaw bit (utrzymuje do RST) |
| RST | `--(R)--` | Kasuj bit |
| OR | `[OR]` | Logiczne LUB (równoległe) |
| AND | `[AND]` | Logiczne I (szeregowe) |
| MOV | `[MOV]` | Kopiuj wartość rejestru |
| CALL | `[CALL]` | Wywołanie podprogramu |
| END | `[END]` | Koniec programu/sekcji |
| TMR | `[TMR]` | Timer |
| CNT | `[CNT]` | Licznik |
| CMP | `[CMP]` | Porównanie wartości |
| ADD | `[ADD]` | Dodawanie |
| SUB | `[SUB]` | Odejmowanie |

## Format binarny .lad

```
[Nagłówek]     Magic bytes + typ maszyny + wersja + zakresy rejestrów
[Etykiety]     Big5 (chiński tradycyjny) — pary bajtów high+low
[Instrukcje]   Opkody 1-3 bajtowe (0x70=LD, 0x6E=OUT, 0x12=SET...)
[NOP]          Bajty wypełnienia 0x0D między instrukcjami
[Rejestry]     ASCII w strumieniu (R, 0x31, 0x30, 0x30 = "R100")
```
