# Profi-Poż — wersja TESTOWA, build 148

## Co jest w paczce
- `app/` — pliki aplikacji do nowego repo (index.html, sw.js, manifest.json, 2 ikony)
- `gas/Code.js` — backend do NOWEGO, testowego projektu Apps Script (arkusz: `testy_profi_apka`)

## Trzy rzeczy PRZED wgraniem
1. **Adres serwera.** Podany link `…/macros/library/d/…/1` to link do biblioteki, nie adres aplikacji — nie zadziała.
   W testowym projekcie: Wdróż → Nowe wdrożenie → typ „Aplikacja internetowa”, Wykonaj jako: *Ja*, Dostęp: *Wszyscy*
   → skopiuj adres kończący się na `/exec`. Wpisz go w zakładce **Serwer** (aplikacja odrzuci adres w złym formacie)
   albo w `app/index.html` w stałej `DEFAULT_SERVER_URL`. W Termuxie:
   `sed -i "s#const DEFAULT_SERVER_URL = '.*';#const DEFAULT_SERVER_URL = 'TU_ADRES_EXEC';#" index.html`
2. **Inna domena niż produkcja.** Repo na INNYM koncie/organizacji GitHub (np. `profipoz-test.github.io`).
   Repo na tym samym koncie co produkcja ma tę samą domenę, czyli wspólną pamięć przeglądarki: test mógłby użyć
   produkcyjnego adresu serwera i nadpisać jego ustawienia.
3. **Arkusz.** GAS pracuje na pliku Dysku o nazwie `testy_profi_apka` (jeśli go nie ma, utworzy go; nazwa musi być unikalna).

## Uruchomienie
1. Wgraj `gas/Code.js` do testowego projektu i wdróż (patrz pkt 1).
2. W edytorze uruchom `utworzProfilAdmin` (wcześniej możesz zmienić w niej `NAZWA`). Kod `XXXX-XXXX` odczytaj w Dzienniku wykonywania; Google może poprosić o zgodę.
3. Wgraj `app/`, ustaw adres. **INFO → Wpisz kod autoryzacji**.
4. **ADMIN → Dostęp urządzeń i profile**: twórz profile, rozdawaj kody.
5. Gdy wszyscy mają aktywne kody, ustaw **Wymagane**.

## Tryby
- **Wyłączone** — aplikacja działa jak dotąd.
- **Wymagane** — pierwszy ekran to „Wymagany kod autoryzacji”, bez pomijania. Serwer odrzuca każde wywołanie bez ważnego tokenu
  (także ze starych wersji aplikacji). Publiczne zostają tylko formularz zgłoszeń klientów (`verifyKodObiektu`, POST `newZgloszenie`).
- Zawieszenie profilu działa tylko w trybie Wymagane.

## Awaryjnie (edytor Apps Script)
`authAwaryjnieWylacz` (tryb Wyłączone dla wszystkich) · `nowyKodDlaProfilu` · `authPokazProfile`

## Czego to NIE zamyka
- **Sam arkusz Google.** Domyślnie `ARKUSZ_PUBLICZNY_LINKIEM = true` w Code.js: każdy z linkiem może go wyświetlać, niezależnie od aplikacji.
  Ustaw `false`, jeśli arkusz ma być prywatny (otworzą go tylko konta, którym go udostępnisz — także przycisk „Excel”).
- Protokoły już zapisane w pamięci telefonu zawieszonego użytkownika.
- Offline: zapamiętany kod ważny 14 dni bez kontaktu z serwerem, potem wymaga połączenia.
- Nie sprawdzano na prawdziwym Google ani Chrome na Androidzie (testy: atrapy usług Google + Chromium).
