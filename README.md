# Microbit-Ranking

Gra zręcznościowa na płytkę micro:bit z przesyłaniem wyników drogą radiową i tabelą wyników na żywo w internecie.

Micro:bit (gra) → Radio → Micro:bit (brama) → USB → Komputer (Python) → REST API (FastAPI) → Baza danych (PostgreSQL) → Strona WWW

## Działanie:

1. Micro:bit (gra)
   Gracz przez 5 sekund klika przycisk A tak szybko, jak potrafi. Po zakończeniu gry płytka pokazuje wynik na ekranie LED i wysyła go drogą radiową (wbudowany moduł Radio) do bramy komunikacyjnej.

2. Micro:bit (brama) + Komputer  
   Drugi micro:bit (brama prowadzącego podłączona przez USB do komputera) odbiera wynik z radia i przekazuje go do programu w Pythonie, który wysyła dane przez internet do serwera w chmurze.

3. Backend i Baza danych (FastAPI + PostgreSQL)
   Serwer na platformie Render odbiera wynik i zapisuje go w bazie danych PostgreSQL. Udostępnia też listę 10 najlepszych wyników.

4. Strona WWW (ranking na żywo)
   Strona internetowa (HTML, TailwindCSS, JavaScript) pobiera wyniki z serwera i na bieżąco pokazuje tabelę najlepszych graczy pod adresem.  



## Baza danych

Do zapisywania wyników służy baza danych PostgreSQL w chmurze (Render). 

Dane trafiają do tabeli **`scores`**, która ma 4 kolumny:
* `id` – unikalny numer wyniku (dodawany automatycznie).
* `player_id` – nazwa gracza (tekst).
* `score` – zdobyte punkty (liczba).
* `created_at` – data i godzina zapisania wyniku.

Baza przyjmuje nowe wpisy z serwera, sortuje je od najwyższego wyniku i zwraca 10 najlepszych na stronę WWW.

---

## Szczegóły techniczne

* Komunikacja radiowa: wysyłanie tekstu `NAZWA_GRACZA,WYNIK` przez wbudowany moduł Radio w micro:bicie.
* Komunikacja z serwerem (HTTPS JSON):
  * `POST /api/scores` – zapisuje nowy wynik w bazie.
  * `GET /api/leaderboard` – pobiera listę 10 najlepszych graczy.
* Użyte technologie: micro:bit (MicroPython, Radio), Python (FastAPI, pyserial, requests), PostgreSQL, Render, HTML, TailwindCSS, JavaScript, GitHub.

---

## Status projektu
* Serwer, baza danych oraz strona WWW działają w chmurze.
* Wysyłanie wyników do bazy zostało przetestowane za pomocą symulatora.
* Integracja z bramą radiową i fizycznym micro:bitem jest gotowa do uruchomienia.
