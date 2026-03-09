# Mapa Drogowa (Sprinty) - NodeGhost Python

Budowa własnego RPA i to opartego na grafie klocków logicznych to ambitne zadanie, ale podzielimy je na bardzo małe, strawne kawałki. Będziemy robić krok za krokiem.

---

## Sprint 1: Fundamenty Danych (Obecny)
**Cel:** Nauczyć system języka, w jakim rozmawia UI (czyli walidacja przychodzącego JSONa z grafem).
1. Zbudowanie struktury modeli `GraphNode`, `GraphEdge` i `GraphPayload` za pomocą `Pydantic`.
2. Stworzenie prostego, pustego endpointu FastAPI, który przyjmuje ten Payload i zwraca "OK - Zrozumiałem Graf".

## Sprint 2: Zbudowanie Mapy (Adjacency List)
**Cel:** Silnik musi wiedzieć, po jakich ścieżkach może wędrować z klocka na klocek.
1. Napisanie silnika analizującego listę Node'ów i Edge'y z Payloadu.
2. Zbudowanie listy sąsiedztwa – zmapowanie, że Klocek A łączy się z Klockiem B.
3. Odnajdywanie punktu startowego (`startNode`).

## Sprint 3: Wędrówka Podstawowa i Polimorfizm (Bez Pętli)
**Cel:** Przechodzenie od klocka do klocka w kodzie (podstawowa Engine Loop) dla prostych, zdefiniowanych z góry węzłów (klocków).
1. Stworzenie klasy `MacroContext` (Plecak z danymi).
2. Zbudowanie klasy bazowej `BaseNode` posiadającej asynchroniczną metodę `execute`.
3. Napisanie klas dla prostych akcji: np. `LogNode` (wypisanie tekstu do zmiennej w pamięci) i `ExportCsvNode`.
4. Stworzenie Głównej Pętli `while`, która idzie z Klocka A do B i na każdym wykonuje `.execute()`.

## Sprint 4: Interpolacja Mocy
**Cel:** Dodanie magii podmieniania tagów w tekście.
1. Napisanie parsera `MacroContext`.
2. Jeżeli w klocku jest zdefiniowany tekst np: `"Przetwarzam {{nazwa}}"`, silnik musi przed wywołaniem akcji zamienić to na odpowiednią wartość w zmiennej `nazwa` ze słownika w MacroContext.

## Sprint 5: Mózg Operacyjny (Stos i Pętle)
**Cel:** Największe inżynierskie wyzwanie. Ogarnięcie rozgałęzień (Branches np. `next_item`, `completed`) i ślepych uliczek.
1. Zmodernizowanie pętli Engine Loop, by zapisywała na "stosie historii" (Call Stack) numery ID klocków, które odwiedziła.
2. Napisanie logiki odnajdywania uśpionych wędlin (`LoopNode`). Gdy Engine trafia na mur (błąd, brak dalszej drogi), cofa się stosu szukając niezamkniętego cyklu loopa, ładuje kolejny stan zmiennej z listy i zaczyna "ścieżkę z powrotem" drogą do `next_item`.

## Sprint 6: Narzędzia do Skrapowania (Przeglądarka)
**Cel:** Podpięcie do systemu skrapującego stronę (Playwright/Puppeteer Python). Właściwie tu powstanie klaster Node'ów symulujących działanie na przeglądarce.
1. `OpenUrlNode`
2. `ClickNode` / `TypeNode`.
3. `ExtractTextNode` oraz wielki `ExtractMultipleNode` (Który to właśnie wypełnia tablice by wykarmić Loop).

## Sprint 7: Dodanie Mocy AI 🤖
**Cel:** Uzbroić Twoje RPA w analizę za pomocą LLM i modeli wizyjnych (Vision).
1. `AiExtractNode` - LLM odpowiada na podstawie kontekstu strony tekstowej.
2. `AiConditionNode` - LLM decyduje w którą stronę idzie gałąź grafu (True/False).
3. `AiClickNode` - Wizualne oznaczanie elementów na stronie i zmuszanie AI do kliknięcia na podstawie Screena!

---

*Zaczynamy od początku: Sprint 1.*
