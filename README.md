# ⛓ Syntec Ladder Viewer

**Wizualny analizator programów PLC (.lad) dla sterowników Syntec CNC.**

Standalone HTML — zero zależności — działa offline w przeglądarce.
---

## 🛠 Funkcje

- Dekodowanie binarnych .lad z etykietami Big5 (chiński tradycyjny)
- 229+ tłumaczeń chiński → polski
- 12 zakładek: Mapa, Przegląd, Sekcje, Struktura, Diagram, Rejestry, Hex, Porównanie, I/O, System, Xref, Pomoc
- Import IoMap XML — mapowanie sprzętowe I/O
- Cross-reference rejestrów (Xref) — gdzie każdy rejestr jest używany
- Globalne wyszukiwanie (Ctrl+K), notatki do sekcji, eksport JSON
- Porównanie wielu plików .lad
- Mapa zależności systemowych CNC

---

## 📂 Struktura projektu

```
SyntecToolbox/
├── index.html              ← Landing page (GitHub Pages)
├── tools/
│   └── SyntecLadderViewer.html   ← Ladder Viewer v4.0
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

---

**WujaMiko1** © 2025-2026
