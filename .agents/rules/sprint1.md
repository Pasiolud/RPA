---
trigger: manual
---

# Sprint 1: Fundamenty Danych (Modele Pydantic)

Pomyśl o tym sprincie jak o "nauczeniu naszego silnika słownictwa". Skoro nasz backend FastAPI dostanie z zewnątrz (czyli od interfejsu w React/Tauri) wielki plik JSON opisujący cały graf (np. klocek Start, potem klocek Otwórz Stronę, potem kliknij), musimy mieć narzędzie, żeby to bezpiecznie przyjąć do pamięci i z tego korzystać.

Dzięki bibliotece `Pydantic` zdefiniujemy struktury (Klasy), przez co nasz backend zyska gwarancję, że otrzymany JSON posiada wymagane parametry, a my zyskamy piękne podpowiadanie kodu w Pythonie!

## Zadania do wykonania krok po kroku w pliku `models.py`:

Chcę, żebyś stworzył nowy plik o nazwie `models.py` (może znajdować się np. w głównym folderze aplikacji serwera obok `main.py`).

### 1. Krok Pierwszy `GraphEdge`
Edge to kabel/strzałka w UI, która łączy dwa węzły (klocki). Posiada początek, koniec i opcjonalnie identyfikator punktu zaczepienia (bo klocek może mieć kilka wyjść).
```json
// Przykładowy JSON reprezentujący Edge od UI:
{
  "source": "123",
  "target": "456",
  "sourceHandle": "next_item" // to pole może być nieobecne (null) dla zwykłych klocków
}
```
**Twoje Zadanie:** 
Zbuduj pydanticowy model `GraphEdge` z polami odpowiadającymi powyższej nazwie. Pamiętaj, że dla Pythona konwencja nazewnicza to `snake_case`, podczas gdy UI przysyła nam JSONy w `camelCase`. `Pydantic` ma super funkcje, żeby automatycznie to mapować (np. aliasy lub Config `alias_generator`). Postaraj się użyć aliasów!

---

### 2. Krok Drugi `GraphNode`
Klocek to absolutnie każda operacja. Może to być `startNode` albo `clickNode`. W UI każdy z nich trzyma swoje ustawienia w zmiennej obiekcie `data`. Ze względu na to, że ustawienia to "wolna amerykanka" (jeden ma np. `url`, drugi wpisany tekst `text`), najlepiej dla pola `data` na ten moment użyć w Pythonie typu `dict`.

```json
// Przykładowy JSON reprezentujący Pojedynczy Węzeł
{
  "id": "abc-123",
  "type": "openUrlNode",
  "data": {
    "url": "https://google.com"
  }
}
```

**Twoje Zadanie:** 
Mając powyższą wiedzę, zdefiniuj model `GraphNode` używając klasy `BaseModel` z Pydantica. Powinien wymuszać trzy rzeczy: `id`, `type` oraz zbiór własnych ustawień czyli np. po prostu słownik w `data`.

---

### 3. Krok Trzeci `GraphPayload` 
To po prostu ostateczna teczka, podsumowująca co my tu wrzucamy. Cały nasz dokument to lista "kabli" (edges) i "klocków" (nodes).

```json
// Ostateczny struktura:
{
  "nodes": [ ... ], // Lista obiektów GraphNode
  "edges": [ ... ]  // Lista obiektów GraphEdge
}
```
**Twoje Zadanie:**
Skonfiguruj klasę `GraphPayload`, definiując po prostu odpowiednie typy listy dla `nodes` i `edges`. To ona będzie naszym głównym strażnikiem bram FastAPI.

---

### 🎯 Gdy Skończysz!
Wrzuć mi tutaj stworzony kod z Pydantic do recenzji, powiedz z czym miałeś trudności i jeśli będzie dobrze, przejdziemy do Sprintu 2! Powodzenia!
