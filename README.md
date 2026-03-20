# ⚙️ Syntec Toolbox

**Zestaw narzędzi webowych do analizy, edycji i wizualizacji projektów CNC Syntec.**

Standalone HTML — zero zależności — działa offline w przeglądarce.

🌐 **Live:** [https://wujamiko1.github.io/SyntecToolbox/](https://wujamiko1.github.io/SyntecToolbox/)

---

## 🛠 Narzędzia

### ⛓ Ladder Viewer v4.0
Wizualny analizator skompilowanych programów PLC (.lad) dla sterowników Syntec CNC.
- Dekodowanie binarnych .lad z etykietami Big5 (chiński tradycyjny)
- 229+ tłumaczeń chiński → polski
- 12 zakładek: Mapa, Przegląd, Sekcje, Struktura, Diagram, Rejestry, Hex, Porównanie, I/O, System, Xref, Pomoc
- Import IoMap XML — mapowanie sprzętowe I/O
- Cross-reference rejestrów (Xref) — gdzie każdy rejestr jest używany
- Globalne wyszukiwanie (Ctrl+K), notatki do sekcji, eksport JSON
- Porównanie wielu plików .lad
- Mapa zależności systemowych CNC

### 🏭 Workshop v2.0
Zintegrowane środowisko z 7 modułami:
- **Screen Editor** — edycja .scr XML z pełnym CRUD elementów
- **Parametry CDB** — przeglądarka baz parametrów
- **Ladder Explorer** — szybki podgląd sekcji PLC
- **Alarm Viewer** — analiza plików .mir/.lkn
- **NC Files** — podgląd programów G-code
- **I/O Viewer** — mapowanie wejść/wyjść

### 🎨 Layout Editor v3.0
Wizualny edytor układów ekranów CNC (.scr):
- Drag & drop elementów na kanwie
- 9 typów elementów Syntec GUI
- Import/eksport XML zgodny z formatem .scr

---

## 📂 Struktura projektu

```
SyntecToolbox/
├── index.html              ← Landing page (GitHub Pages)
├── tools/
│   ├── SyntecLadderViewer.html   ← Ladder Viewer v4.0
│   ├── SyntecWorkshop.html       ← Workshop v2.0
│   └── SyntecLayoutEditor.html   ← Layout Editor v3.0
├── docs/
│   └── ladder-viewer.md         ← Dokumentacja Ladder Viewer
└── README.md
```

## 🚀 Jak używać

1. **Online:** Otwórz [stronę GitHub Pages](https://wujamiko1.github.io/SyntecToolbox/)
2. **Offline:** Sciągnij repozytorium i otwórz dowolny plik `.html` w przeglądarce

Każde narzędzie to samodzielny plik HTML. Brak potrzeby serwera, instalacji, Node.js ani internetu.

## 📁 Gdzie znaleźć pliki w projekcie Syntec

| Plik | Ścieżka | Opis |
|------|---------|------|
| `cnc.lad` | `Project/DiskC/OpenCNC/Ladder/` | Główny program PLC maszyny |
| `develop.lad` | `Project/DiskC/OpenCNC/Ladder/` | Wersja deweloperska |
| `cnc_server.lad` | `Project/DiskC/OpenCNC/Ladder/` | Program PLC serwera (multi-CPU) |
| `IoMap_*.xml` | `OpenCNC/Data/IO/` | Konfiguracja mapowania I/O |
| `*.scr` | `OpenCNC/Res/` | Pliki ekranów CNC (XML) |
| `*.cdb` | `cdb/CNC/` | Bazy parametrów CNC |
| `*.mir` / `*.lkn` | `mir/` / `lkn/` | Pliki alarmów i logów |

## 🔧 Technologia

- **HTML5 + CSS3 + Vanilla JS** — zero frameworków, zero zależności
- **Standalone** — każdy plik działa samodzielnie
- **Dark Theme** — profesjonalny ciemny motyw
- **Big5 Decoder** — dekodowanie chińskiego tradycyjnego w przeglądarce
- **localStorage** — notatki zapisywane lokalnie w przeglądarce

## 📝 Changelog

### v4.0 — Ladder Viewer (2026-03)
- ✅ Xref Tab — cross-reference rejestrów ↔ sekcji
- ✅ Pomoc Tab — referencja instrukcji PLC, przewodnik po plikach, typy rejestrów
- ✅ Globalne wyszukiwanie (Ctrl+K) — sekcje, rejestry, I/O, słownik
- ✅ Import IoMap XML — mapowanie sprzętowe I/O
- ✅ Notatki do sekcji (localStorage)
- ✅ Rejestry per sekcja w sidebarze

### v3.0 — Ladder Viewer
- Mapa wizualna z treemap
- 229+ tłumaczeń CN→PL
- 10 zakładek z pełną analizą
- Porównanie plików, diagram drabinkowy

### v2.0 — Workshop
- Screen Editor z pełnym CRUD elementów
- 7 modułów zintegrowanych

### v3.0 — Layout Editor
- Drag & drop, 9 typów elementów
- Eksport/import XML

---

**WujaMiko1** © 2025-2026
