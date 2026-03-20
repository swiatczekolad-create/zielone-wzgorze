# Szablon strony deweloperskiej — GitHub Pages

## Architektura

```
repo/
├── index.html        # Główna strona (landing page)
├── admin.html        # Panel admina (edycja treści)
├── content.json      # Edytowalne treści (ładowane dynamicznie)
├── CNAME             # Domena własna
└── img/              # Zdjęcia lokalne (slider, wizualizacje)
    ├── slide1.jpg
    ├── slide2.jpg
    └── ...
```

## Stos technologiczny

- **Hosting:** GitHub Pages (darmowy, SSL automatyczny)
- **Domena:** dowolny rejestrator (OVH, home.pl, nazwa.pl)
- **Panel admina:** czysty HTML/JS + GitHub API (zero backendu)
- **Style:** Google Fonts (DM Serif Display + Outfit), czyste CSS, SVG ikony line-art
- **Brak frameworków** — jeden plik HTML, zero zależności

## Konfiguracja domeny (OVH)

### Rekordy DNS:
| Typ   | Nazwa | Wartość                              |
|-------|-------|--------------------------------------|
| A     | @     | 185.199.108.153                      |
| A     | @     | 185.199.109.153                      |
| A     | @     | 185.199.110.153                      |
| A     | @     | 185.199.111.153                      |
| CNAME | www   | [user].github.io.  (z kropką!)       |

### Po propagacji DNS:
- GitHub API: `PUT /repos/{owner}/{repo}/pages` z `https_enforced: true`
- Albo ręcznie: repo → Settings → Pages → Enforce HTTPS

## Panel Admina — jak działa

### Logowanie:
1. **Pierwszy raz:** klient dostaje link z tokenem: `domena.pl/admin.html#klucz=ghp_xxx`
2. Token zapisuje się w `localStorage`
3. **Następne razy:** automatyczne logowanie (token z localStorage)
4. **Wylogowanie** czyści localStorage — potrzebny ponownie link

### Edycja treści:
1. admin.html wczytuje content.json przez GitHub API
2. Klient edytuje pola w formularzu
3. Klik "Zapisz" → PUT do GitHub API → aktualizacja content.json
4. GitHub Pages przebudowuje stronę (1-2 min)
5. index.html ładuje content.json i podmienia elementy z `data-content="klucz"`

### Token GitHub (dla klienta):
- Typ: Fine-grained lub Classic
- Uprawnienia: `repo` (classic) lub Contents read/write (fine-grained)
- Ograniczenie: najlepiej tylko do jednego repo

## Optymalizacja wydajności

- **Slider:** tylko pierwszy slide ładowany od razu, reszta lazy (data-bg)
- **Zdjęcia w treści:** `loading="lazy"`
- **Mapa Google:** `loading="lazy"` na iframe
- **Preload:** pierwszy slide w `<head>`
- **Zdjęcia lokalne w repo** — zero zewnętrznych zależności

## Sekcje strony (typowy układ)

1. **Nav** — logotyp tekstowy + linki + CTA
2. **Hero** — slider z wizualizacjami + podpis architekta (kreska + tekst)
3. **Pasek liczb** — metraż, działka, ilość, rok
4. **O inwestycji** — lifestyle intro + opis + zdjęcia + "Dlaczego my?" grid + CTA banner
5. **Domy** — karty parter/piętro/działka + rozkład pomieszczeń
6. **Lokalizacja** — atuty + mapa Google
7. **Cennik** — karty budynków z ceną i terminami
8. **Harmonogram** — timeline
9. **Kontakt** — dane + formularz
10. **Footer** — deweloper + disclaimer

## Checklist nowego projektu

- [ ] Nowe repo na GitHub
- [ ] Skopiować strukturę z tego szablonu
- [ ] Zmienić treści w index.html i content.json
- [ ] Podmienić zdjęcia w /img/
- [ ] Zmienić nazwę repo w admin.html (zmienna REPO)
- [ ] Dodać CNAME z domeną
- [ ] Skonfigurować DNS u rejestratora
- [ ] Włączyć GitHub Pages + HTTPS
- [ ] Wygenerować token dla klienta
- [ ] Wysłać klientowi link do admina z tokenem

## Uwagi

- **content.json** — uważać na polskie cudzysłowy „ " w JSON (łamią parser)
- **Token w kodzie** — nigdy nie commitować! GitHub Push Protection to blokuje
- **Mobile menu** — zamykanie: tap w tło, swipe w dół, Escape, hamburger, klik linku
- **Formularz kontaktowy** — wymaga backendu (np. Formspree) lub jest dekoracyjny
