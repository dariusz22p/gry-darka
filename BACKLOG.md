# Pojedynek Zamków — Backlog

Gra artyleryjska 2-osobowa (hotseat). Plik gry: `pojedynek-zamkow/index.html`.
Stack: HTML5 Canvas + JavaScript + [Matter.js](https://brm.io/matter-js/) (fizyka 2D, self-hosted — offline). PWA: manifest + service worker.

## Struktura repo (layout pod wiele gier)
- `index.html` — strona-hub z listą gier.
- `pojedynek-zamkow/index.html` — gra „Pojedynek Zamków (Janek i Darek)".
- `pojedynek-zamkow/` — PWA: `manifest.webmanifest`, `sw.js`, `icon.svg`, `matter.min.js` (self-host).
- `gra-2/index.html` — placeholder na kolejną grę.
- `.nojekyll` — wyłącza przetwarzanie Jekyll na GitHub Pages.

## Jak uruchomić
- Online (GitHub Pages): <https://dariusz22p.github.io/gry-darka/>
- Lokalnie: otwórz `index.html` (hub) lub `pojedynek-zamkow/index.html` (gra) — Matter.js jest lokalny, działa offline.
- Na telefonie (PWA): wejdź na hosting, otwórz grę i „Dodaj do ekranu głównego" (instalowalna, offline).
- Sterowanie: `↑`/`↓` kąt, `←`/`→` moc, `Spacja` strzał, `R` nowy mecz, `F` pełny ekran, `M` dźwięk.
- 2 graczy na zmianę na jednej klawiaturze. Wygrywa ten, kto zbije HP przeciwnika do 0.

## Gdzie co jest (mapa kodu w pojedynek-zamkow/index.html)
- `CONFIG` (góra `<script>`) — wszystkie liczby do strojenia balansu: kąt, moc, HP, obrażenia, grawitacja, rozmiar/pozycja zamków.
- `setupWorld()` / `buildCastle()` — budowa planszy i zamków (cegły to fizyczne bryły).
- `fire()` — tworzenie i wystrzelenie pocisku.
- `collisionStart` — naliczanie obrażeń (skalowane prędkością, z limitem na strzał).
- `afterUpdate` — logika końca tury (spoczynek / poza planszą / timeout).
- `afterRender` — rysowanie armat, celownika i efektów trafień.
- HUD/DOM — paski HP, wskaźnik tury, ekran zwycięstwa.

## Strojenie balansu (szybkie pokrętła w CONFIG)
- Pocisk za słaby/za mocny → `power.max`, `power.def`, `powerScale`.
- Za szybkie/za wolne niszczenie → `dmgScale`, `dmgMax`, `maxHp`.
- Łuk lotu za płaski/za stromy → `gravity`.
- Trudniej/łatwiej trafić → odległość zamków `p1x`/`p2x`, wielkość zamku `castle`.

---

## Etap 1 — MVP (ZROBIONE)
- [x] Szkielet: `index.html`, canvas, pętla Matter.js
- [x] Plansza: ziemia + dwa zamki z cegieł + paski HP
- [x] Wskaźnik tury i celowania (kąt/moc)
- [x] Sterowanie klawiaturą (hotseat)
- [x] Pocisk z grawitacją (lot po paraboli)
- [x] Kolizje i obrażenia zależne od siły uderzenia
- [x] Zmiana tury po strzale (auto-wykrycie końca lotu)
- [x] Warunek zwycięstwa + ekran „Zagraj ponownie"
- [x] Fizyczne rozpadanie się cegieł (Matter.js)

## Etap 2 — Głębia rozgrywki
- [x] Wiatr losowany co turę (wskaźnik + wpływ na tor lotu)
- [x] Podgląd trajektorii (linia kropkowana / „ghost shot")
- [x] Przeszkody/teren w połowie planszy do przestrzelenia (losowa wysokość co rundę)
- [x] Punktacja i rundy (best of 3)
- [ ] Warunek zwycięstwa „przewrócenie wieży" (nie tylko HP) — kolor/„korona" na szczycie
- [ ] Ograniczony czas/tura albo licznik amunicji
- [ ] Losowe rozstawienie/wysokość zamków

## Etap 3 — Grafika, audio, UX
- [ ] Sprite'y zamków i tła (zamiast prostokątów)
- [ ] Efekty cząsteczkowe wybuchu i dymu
- [x] Dźwięki: strzał, lot (subtelny), trafienie/wybuch, wygrana (Web Audio, syntezowane) + wyciszenie (klawisz M)
- [ ] Animowany pasek mocy (naciśnij i przytrzymaj Spację)
- [ ] Wstrząs ekranu przy trafieniu (screen shake)
- [ ] Ekran startowy + menu (start / instrukcja / ustawienia)
- [x] Pełny ekran (Fullscreen API + skalowanie z zachowaniem proporcji, letterbox) — wariant A
- [ ] W pełni responsywny świat: canvas = rozmiar okna, layout przeliczany z W/H + obsługa resize (wariant B)

## Etap 4 — Tryby i przeciwnicy
- [ ] Tryb 1-osobowy: przeciwnik AI (łatwy/średni/trudny — dobór kąta i mocy)
- [ ] Split-keyboard: obaj gracze na raz (WASD vs strzałki)
- [ ] Multiplayer online (WebSocket + mały serwer Node)
- [ ] Różne typy pocisków / power-upy (rozłupujący, cięższy, wybuchowy)
- [ ] Wybór postaci/zamku i kolorów

## Etap 5 — Jakość i infrastruktura
- [ ] Rozdzielenie na moduły (`game.js`, `render.js`, `ui.js`, `physics.js`) + `styles.css`
- [x] Build/hosting: publikacja na GitHub Pages (<https://dariusz22p.github.io/gry-darka/>)
- [x] Strona-hub z listą gier (layout pod kolejne gry) + placeholder `gra-2/`
- [ ] Zapis lokalny (localStorage): wynik meczów, ustawienia
- [ ] Podstawowe testy logiki (obrażenia, koniec tury, warunek zwycięstwa)
- [x] Self-host Matter.js (bez zależności od CDN) — działanie offline

## Etap 6 — Mobile / Android
Cel: grać na telefonie/tablecie (hotseat świetnie działa na jednym ekranie).
- [x] A. Sterowanie dotykowe: przyciski kąta/mocy (hold-repeat) + „OGNIA" (pointer/touch); tap „następna runda"
- [x] B. Responsywny layout: skalowanie widoku do ekranu (fit-to-viewport, dvh), kompaktowy HUD i ukryte podpowiedzi na telefonie, podpowiedź obrotu w pionie
- [x] C. Hosting pod URL (GitHub Pages) — zrobione w Etapie 5
- [x] D. PWA: manifest + service worker + self-host Matter.js (offline, „dodaj do ekranu głównego")
- [ ] E. APK / Google Play: Capacitor lub TWA (build w Android Studio/Gradle) — nakład średni (dochodzi toolchain)

## Balans / fair play
Problem: w turowej artylerii pierwszy strzelający ma przewagę (może zadać śmiertelny cios wcześniej).

- [x] Wyrównanie liczby strzałów w rundzie: obaj oddają tyle samo strzałów, więc startujący nie wygrywa tylko dlatego, że strzela pierwszy (drugi ma „wyrównanie"; oba zamki padają → remis i powtórka rundy)

Quick wins:
- [ ] Losowy startujący w rundzie decydującej przy 1:1
- [ ] Opcjonalny handicap startującego (np. −5% mocy w 1. turze)
- [ ] Wyraźniejszy wskaźnik „wyrównanie / ostatni strzał" (kolor/animacja + dźwięk)
- [ ] Informacja „kto zaczyna rundę" na ekranie startu rundy

Cięższe zadania:
- [ ] Symultaniczne strzały: obaj celują w tajemnicy, potem jednoczesne rozwiązanie tury
- [ ] Sudden death po serii remisów (kurczące się zamki / rosnący wiatr)
- [ ] Statystyki celności + prosty ranking (localStorage)
- [ ] AI z regulacją trudności (trening solo, dobór kąta/mocy)

## Parking lot (pomysły na później)
- Zniszczalny teren (kratery po trafieniach)
- Grawitacja/pogoda jako modyfikatory rundy
- Edytor plansz
- Tabela wyników / statystyki celności
- Tryb turniejowy dla wielu graczy

## Znane ograniczenia MVP
- Wartości fizyki są wstępne — mogą wymagać dostrojenia w `CONFIG` po pierwszych partiach.
- Można trafić własny zamek przy bardzo niskim kącie (celowe — element strategii; do zmiany w `collisionStart`).
- Wymaga połączenia z internetem (Matter.js z CDN) — patrz Etap 5 (self-host).
