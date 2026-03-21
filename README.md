
# SyntecToolbox

Zestaw narzędzi do analizy i diagnostyki sterowników CNC Syntec.
Cel: pełne zrozumienie programów PLC (.lad) — od binarnego kodu po fizyczne okablowanie w szafie sterowniczej.

**Live:** https://wujamiko1.github.io/SyntecToolbox/

---

## Narzędzia

### [Syntec Ladder Viewer](https://wujamiko1.github.io/SyntecToolbox/tools/SyntecLadderViewer.html)

Przeglądarka skompilowanych programów PLC (.lad) dla sterowników Syntec CNC (serie 6/7).

### Co potrafi teraz (v4.0)

| Funkcja | Status | Opis |
|---------|--------|------|
| Parser binarny .lad | ✅ Działa | Nagłówek, sekcje, etykiety (Big5), rejestry, statystyki instrukcji |
| Tłumaczenie CN→PL | ✅ 229 tłumaczeń | Automatyczne tłumaczenie chińskich nazw sekcji na polski |
| Kategoryzacja sekcji | ⚠️ Częściowa | 14 kategorii, ~200 słów kluczowych. **281 sekcji jako "Inne"** — wymaga rozbudowy |
| 12 zakładek UI | ✅ Działa | Mapa, Przegląd, Sekcje, Struktura, Diagram, Rejestry, Hex, Porównanie, I/O, System, Xref, Pomoc |
| IoMap XML import | ✅ Działa | Wizualne moduły (RIO, HK, M3IO, RFHHB, LIO), adresy fizyczne |
| Cabinet Tracer | ✅ Działa | Mapowanie adresów logicznych → fizyczne porty/piny w szafie |
| Sekcja→I/O cross-ref | ✅ Działa | Automatyczne: które sekcje sterują którymi portami |
| Dekodowanie instrukcji | ⚠️ 8 z ~50 opkodów | LD, OUT, OR, SET, RST, CALL, MOV, END — reszta nierozpoznana |
| Diagram drabinkowy | ⚠️ Podstawowy | Pseudo-wizualizacja, brak prawdziwej siatki kontaktów/cewek |
| Analiza rejestrów | ⚠️ Skan bajtowy | Wykrywa nazwy R/K/D/T/C/M, ale nie rozróżnia READ vs WRITE |
| Globalne wyszukiwanie | ✅ Ctrl+K | Przeszukuje sekcje, rejestry, tłumaczenia |
| Porównanie plików | ✅ Działa | Dwa pliki .lad obok siebie, diff bajtowy |
| Eksport JSON | ✅ Ctrl+E | Pełna struktura pliku jako JSON |
| Adnotacje/notatki | ✅ localStorage | Notatki do sekcji z ikoną 📝 |

### Co NIE działa / wymaga pracy

| Problem | Wpływ | Priorytet |
|---------|-------|-----------|
| 281 sekcji "Inne" bez kategorii | Nie wiadomo co sekcja robi | 🔴 Krytyczny |
| Tylko 8 rozpoznanych opkodów | 50-60% instrukcji to "nieznane" | 🔴 Krytyczny |
| Brak prawdziwego diagramu drabinkowego | Nie widać logiki AND/OR/kontakty/cewki | 🔴 Krytyczny |
| Rejestry bez kierunku (R/W) | Xref nie pokazuje kto pisze a kto czyta | 🟡 Ważny |
| Brak wartości timerów/counterów | T1 istnieje, ale nie wiadomo ile czasu | 🟡 Ważny |
| Brak grafu wywołań sekcji | Nie widać CALL sekcja→sekcja | 🟡 Ważny |
| Brak analizy przepływu logiki | "co aktywuje Y1.5?" — brak odpowiedzi | 🟡 Ważny |
| Brak wykrywania maszyn stanów | Sekwencje T-code, ATC nie odtworzone | 🟠 Średni |

---

## Architektura techniczna

### Format binarny .lad
```
Offset  Zawartość
0x0000  Nagłówek: magic bytes, typ maszyny (PT/5A/NA), wersja, data
0x0040  Definicje zakresów rejestrów (R0-R999, K0-K999, D0-D999...)
~0x0100 Etykiety sekcji: Big5-encoded pary bajtów (00 00 XX YY)
~0x1A00 Instrukcje binarne: opkody 1-3B + operandy, oddzielone NOP (0x0D)
~0xEOF  Koniec pliku
```

### Typy rejestrów Syntec PLC
| Typ | Nazwa | Trwałość | Rozmiar | Zastosowanie |
|-----|-------|---------|---------|-------------|
| R | Relay | Volatile | 1 bit | Flagi tymczasowe, stany logiczne |
| K | Keep Relay | Non-volatile | 1 bit | Stany trwałe (przeżywają restart) |
| D | Data | Volatile | 16/32-bit | Wartości numeryczne, pozycje, prędkości |
| T | Timer | Volatile | 16-bit + bit | Odmierzanie czasu (preset + aktualne) |
| C | Counter | Volatile | 16-bit + bit | Zliczanie impulsów/zdarzeń |
| X | Input | Read-only | 1 bit | Fizyczne wejścia cyfrowe |
| Y | Output | Read/write | 1 bit | Fizyczne wyjścia cyfrowe |
| S | System | Read-only | 1 bit | Statusy systemowe CNC |
| A | Alarm | Read/write | 1 bit | Kody alarmów |
| M | M-code | Special | Varies | Kody maszynowe (M03=wrzeciono, M06=narzędzie) |

### Mapowanie rejestrów → moduły fizyczne
| Zakres R | Moduł | Typ |
|----------|-------|-----|
| R0-R63 | RIO (Remote I/O) | Cyfrowe wejścia/wyjścia |
| R64-R119 | HK (HandKit/pendant) | Panel operatora |
| R120-R127 | MPG | Kółko ręczne |
| R128-R191 | HK2 (HandKit rozszerzony) | Drugi panel |
| R192-R217 | RFHHB (RF Handwheel) | Bezprzewodowe kółko |
| R320-R511 | M3IO (M3 Serial I/O) | Rozszerzenie szeregowe |
| R800-R832 | MLC System Parameters | Parametry systemowe #3421-3433 |
| R3000-R3499 | User-defined registers | Rejestry użytkownika |
| R10000-R10020 | Communication/timing | Komunikacja RS485, zegary |
| R40000-R40001 | User variables | Zmienne globalne |

### Rozpoznane opkody instrukcji (aktualnie 8)
| Hex | Mnemonik | Symbol | Opis |
|-----|----------|--------|------|
| 0x70 | LD | `--[ ]--` | Załaduj kontakt (NO) |
| 0x6E | OUT | `--( )--` | Cewka wyjściowa |
| 0x0E | OR | `[OR]` | Logiczne LUB (gałąź równoległa) |
| 0x12 | SET | `--(S)--` | Ustaw bit (latch) |
| 0x14 | RST | `--(R)--` | Kasuj bit |
| 0xC8 | CALL | `[CALL]` | Wywołanie podprogramu/sekcji |
| 0xC6 | MOV | `[MOV]` | Kopiowanie danych |
| 0xC5 | END | `[END]` | Koniec sekcji/bloku |

### Brakujące opkody (do odkrycia)
LDI (NC contact), AND, ANI, ORI, MPS/MRD/MPP (stos), TMR, CNT, CMP, ADD, SUB, MUL, DIV, DMOV, DCMP, JMP, RET, NOP, oraz ~30 innych z rodziny Syntec MLC.

---

## Roadmap rozwoju

### Faza 1: Pełna klasyfikacja sekcji (281 "Inne" → 0)
- [ ] Rozbudowa słownika CN→PL (229 → 500+ tłumaczeń)
- [ ] Dodanie brakujących kategorii: Diagnostyka, Timing, Wersja, EEPROM/Backup, Parametry maszynowe
- [ ] Rozszerzenie słów kluczowych w istniejących kategoriach (+200 keywords)
- [ ] System auto-kategoryzacji na podstawie rejestrów (sekcja używa R411-R418 → Osie/Jog)
- [ ] Tryb "nauki" — kliknij sekcję "Inne" i przypisz kategorię ręcznie
- [ ] Eksport/import bazy kategoryzacji (JSON) dla różnych maszyn

### Faza 2: Pełne dekodowanie instrukcji
- [ ] Reverse-engineering wszystkich opkodów (analiza par: znany opkod + bajty po nim)
- [ ] Parsowanie operandów: LD R100.3 → kontakt R100 bit 3
- [ ] Wykrywanie typów kontaktów: NO vs NC (LDI, ANI, ORI)
- [ ] Dekodowanie MOV/CMP/ADD/SUB z źródłem i celem
- [ ] Dekodowanie TMR z wartością presetu i jednostką czasu
- [ ] Dekodowanie CNT z wartością presetu
- [ ] Rozpoznawanie CALL z numerem docelowej sekcji
- [ ] Tabela wszystkich opkodów z przykładami

### Faza 3: Prawdziwy diagram drabinkowy
- [ ] Wykrywanie granic szczebli (rung boundaries) przez END/NOP markery
- [ ] Budowa siatki: kontakty (kolumny) → cewki (prawa strona)
- [ ] Wizualizacja AND (szeregowe) i OR (równoległe gałęzie)
- [ ] Kolorowanie: zielony=NO, czerwony=NC, niebieski=cewka, żółty=timer
- [ ] Interaktywność: kliknij kontakt → pokaż rejestr, sekcje, fizyczne I/O
- [ ] Numerowanie szczebli (Rung #1, #2, ...)
- [ ] Eksport diagramu jako SVG/PNG

### Faza 4: Analiza przepływu danych i logiki
- [ ] Graf wywołań: CALL sekcja→sekcja (wizualny directed graph)
- [ ] Analiza kierunku rejestrów: READ (LD/AND/OR) vs WRITE (OUT/SET/RST/MOV)
- [ ] Łańcuchy zależności: R100 (write w sekcji #3) → R100 (read w sekcji #7)
- [ ] "Co aktywuje Y1.5?" — śledzenie wstecz przez wszystkie warunki
- [ ] "Co robi R261.0?" — pokaz wszystkie sekcje + fizyczne I/O + opis
- [ ] Wykrywanie nieużywanych rejestrów (write-only, never read)
- [ ] Wykrywanie konfliktów (dwie sekcje piszą do tego samego rejestru)

### Faza 5: Wykrywanie wzorców i maszyn stanów
- [ ] Automatyczna detekcja: sekwencja T-code (narzędzie), ATC cycle
- [ ] Identyfikacja maszyn stanów: stan1→warunek→stan2→...
- [ ] Wizualizacja cyklu jako state diagram
- [ ] Wykrywanie interlockow (safety chains: E-STOP → wszystkie wyjścia OFF)
- [ ] Analiza czasowa: "jak długo trwa pełen cykl wymiany narzędzia?"
- [ ] Generowanie opisu tekstowego: "Sekcja #23 steruje załadunkiem materiału: czeka na X2.0, aktywuje Y2.3 na 2s, potem Y2.4"

### Faza 6: Wsparcie wielu maszyn i porównywanie
- [ ] Profile maszyn: zapamiętaj kategoryzację, notatki, nazwy I/O per maszyna
- [ ] Porównanie programów PLC dwóch maszyn (sekcja po sekcji)
- [ ] Diff instrukcji (nie tylko bajtów): "dodano 3 szczeble w sekcji Spindle"
- [ ] Import/eksport profilu maszyny (share z innymi technikami)
- [ ] Raport diagnostyczny: "ta maszyna vs standardowa — 12 różnic"

### Faza 7: Dokumentacja i raporty
- [ ] Auto-generowanie dokumentacji PLC: PDF/HTML z opisem każdej sekcji
- [ ] Lista I/O z opisami: "X0.3 = czujnik narzędzia, kabel z modulu RIO1 port M1 pin 3"
- [ ] Raport rejestrów: tabela R0-R511 z opisem, kierunkiem, sekcjami
- [ ] Schemat szafy sterowniczej z zaznaczonymi I/O

---

## Struktura plików projektu

```
SyntecToolbox/
├── index.html                    # Landing page
├── tools/
│   └── SyntecLadderViewer.html   # Główna aplikacja (single-file, ~2200 linii)
├── docs/
│   └── ladder-viewer.md          # Dokumentacja użytkownika
└── README.md                     # Ten plik
```

## Technologia
- Czysty HTML/CSS/JS w jednym pliku — zero zależności, działa offline
- Dark theme, ES5-compatible, polski interfejs
- Hosting: GitHub Pages
- IoMap XML import bezpośrednio w przeglądarce

## Kontekst: sterowniki Syntec CNC
Narzędzia do pracy z maszynami CNC (frezarki, tokarki, centra obróbcze) sterowanymi kontrolerami Syntec seria 6/7.
- `.lad` — skompilowany program PLC (format binarny, etykiety w Big5 Traditional Chinese)
- `IoMap_*.xml` — konfiguracja mapowania I/O (ścieżka: `OpenCNC/Data/IO/`)
- `SriModule.xml` — definicja modułów I/O w szafie
- `M3_SerialParam.txt` — parametry portów szeregowych M3IO

## Licencja
**WujaMiko1** © 2025-2026
