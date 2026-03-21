# ⚙️ Syntec Toolbox

Zestaw narzędzi do analizy i edycji projektów **Syntec CNC**.
Każde narzędzie to samodzielny plik HTML — zero zależności, działa offline w przeglądarce.

**WujaMiko1 © 2025–2026**

---

## 🛠️ Narzędzia

### 🔗 [Syntec Ladder Viewer v4.0](tools/SyntecLadderViewer.html)

Wizualna przeglądarka skompilowanych programów PLC (`.lad`) dla sterowników Syntec CNC.

**Funkcje:**
- Dekodowanie binarnych plików `.lad` z etykietami w kodowaniu Big5 (chiński tradycyjny)
- **260+ tłumaczeń** terminów chińskich na polski (E-STOP, wrzeciono, narzędzia, pneumatyka, próżnia…)
- **12 zakładek:** Mapa, Przegląd, Sekcje, Struktura, Diagram, Rejestry, Hex, Porównanie, I/O, System, Xref, Pomoc
- Kategoryzacja sekcji PLC (bezpieczeństwo, wrzeciono, narzędzia, osie, pneumatyka, próżnia…)
- Cross-reference rejestrów (Xref) — gdzie każdy rejestr jest używany
- Import `IoMap_*.xml` — mapowanie sprzętowe I/O z konfiguracji maszyny
- Porównanie wielu plików `.lad` naraz
- Globalne wyszukiwanie (`Ctrl+K`)
- Notatki/adnotacje do sekcji (localStorage)
- Eksport do JSON i etykiet tekstowych

---

## 🚀 Jak używać

1. Otwórz plik `index.html` lub bezpośrednio `tools/SyntecLadderViewer.html` w przeglądarce
   *(Chrome, Edge, Firefox — nowsze wersje)*
2. Kliknij **📂 Wczytaj .lad** lub przeciągnij plik `.lad` na okno aplikacji
3. Nawiguj po zakładkach aby analizować program PLC

> **Nie wymaga** serwera, instalacji ani dostępu do internetu.

---

## 📂 Gdzie znaleźć pliki .lad

```
Project/DiskC/OpenCNC/Ladder/
├── cnc.lad          ← główny program PLC maszyny
├── cnc_server.lad   ← program serwera (multi-CPU)
└── develop.lad      ← wersja deweloperska
```

Pliki konfiguracji I/O (IoMap XML):

```
Project/DiskC/OpenCNC/Data/IO/IoMap_*.xml
```

---

## ⌨️ Skróty klawiszowe

| Skrót        | Akcja                          |
|--------------|--------------------------------|
| `Ctrl+K`     | Globalne wyszukiwanie          |
| `Ctrl+O`     | Otwórz plik .lad               |
| `Ctrl+E`     | Eksport JSON                   |
| `Ctrl+1…9`   | Przełącz zakładkę              |
| `Esc`        | Zamknij wyszukiwanie           |
| `↑ ↓`        | Nawigacja wyników wyszukiwania |
| `Enter`      | Wybierz wynik                  |

---

## 📖 Dokumentacja

Pełna dokumentacja narzędzia: [docs/ladder-viewer.md](docs/ladder-viewer.md)

---

## 🔧 Technologia

- **Standalone HTML** — jeden plik, zero frameworków
- **Vanilla JavaScript** — bez zewnętrznych zależności
- **Offline-first** — działa bez dostępu do internetu
- **Big5 decoder** — wbudowane dekodowanie chińskich etykiet CNC
