# AKSO - procesy, stany, przerwania i wywołania systemowe

## Najkrócej

**Proces** to wykonywany program wraz z jego kontekstem.

Program jest pasywny, bo jest plikiem albo kodem. Proces jest aktywny, bo wykonuje się, ma stan, rejestry, pamięć i zasoby.

## Co należy do procesu

Proces obejmuje między innymi:

- przestrzeń adresową,
- kod programu,
- dane,
- stos,
- stertę,
- wartości rejestrów procesora,
- licznik rozkazów,
- otwarte pliki,
- identyfikator procesu,
- stan procesu,
- informacje planisty.

System operacyjny przechowuje te informacje w **PCB**, czyli bloku kontrolnym procesu.

## Przestrzeń adresowa procesu

Przestrzeń adresowa procesu jest logicznym obrazem pamięci widzianej przez proces.

Typowe części:

| Część | Co zawiera |
|---|---|
| text | kod programu, czyli instrukcje maszynowe |
| data | zmienne globalne i statyczne |
| heap | pamięć dynamiczna, np. `malloc`, `new` |
| stack | stos wywołań funkcji, zmienne lokalne, adresy powrotu |

W podstawowym ujęciu ze slajdów przestrzeń adresowa zawiera:

- kod programu, czyli **text section**,
- dane, czyli **data section**,
- stos procesu, czyli **stack section**.

Przestrzeń adresowa jest adresowana logicznie, często tak, jakby zaczynała się od adresu 0. Proces nie musi wiedzieć, gdzie jego dane naprawdę leżą w RAM.

Adresy logiczne są sprzętowo odwzorowywane na adresy fizyczne. To odwzorowanie nazywa się **translacją adresów**.

Przestrzeń adresowa procesu:

- nie musi w całości znajdować się w pamięci fizycznej, jeśli system używa pamięci wirtualnej,
- podlega ochronie przed innymi procesami,
- daje procesowi wrażenie własnej, odizolowanej pamięci.

### Text

Segment **text** zawiera kod programu.

Typowo:

- jest wykonywalny,
- często jest tylko do odczytu,
- może być współdzielony między procesami uruchamiającymi ten sam program.

### Data

Segment **data** zawiera dane globalne i statyczne.

Przykłady:

```c
int global_x = 5;
static int counter = 0;
```

Często wyróżnia się też **BSS**, czyli obszar na zmienne globalne/statyczne bez jawnej wartości początkowej albo zerowane.

### Heap

**Heap** służy do pamięci dynamicznej.

Przykłady:

```c
malloc(100);
new int;
```

Heap zwykle rośnie w stronę większych adresów, ale to szczegół implementacyjny, nie definicja.

### Stack

**Stack** zawiera informacje związane z wywołaniami funkcji.

Typowo:

- zmienne lokalne,
- adres powrotu,
- zapisane rejestry,
- ramki funkcji.

Stos w wielu architekturach, np. x86, rośnie w dół adresów.

Pułapka:

> Zmienne lokalne zwykle są na stosie, ale zmienne globalne/statyczne są w data/BSS, nie na stosie.

Druga pułapka:

> To, że proces widzi adres logiczny 0, nie znaczy, że używa fizycznego adresu RAM 0. Sprzęt i system operacyjny tłumaczą adresy.

## PCB

PCB zawiera informacje potrzebne do zarządzania procesem i wznowienia jego wykonania.

PCB jest **metryczką/opisem procesu**, a nie samym procesem.

Typowo zawiera:

- PID,
- stan procesu,
- licznik rozkazów,
- zapisane rejestry,
- informacje o pamięci procesu,
- informacje o szeregowaniu,
- informacje o otwartych plikach.

Pułapka:

> PCB nie przechowuje całej zawartości stron pamięci procesu. Przechowuje metadane i wskaźniki/struktury potrzebne do zarządzania.

Inaczej:

```text
proces = wykonywany program + pamięć + rejestry + zasoby + stan
PCB    = informacje o procesie potrzebne systemowi
```

W kolejkach system przechowuje PCB albo wskaźniki do PCB. To nie znaczy, że proces "siedzi w PCB"; w PCB siedzi opis procesu.

## Kolejki procesów

System operacyjny przechowuje procesy w kolejkach przez ich **PCB** albo wskaźniki do PCB. Nie przenosi w kolejce całej pamięci procesu.

Najważniejsze kolejki:

| Kolejka | Co zawiera |
|---|---|
| kolejka gotowych | PCB procesów gotowych do dostania CPU |
| kolejka wstrzymanych na zasobie | PCB procesów czekających na konkretny zasób albo zdarzenie |

Przykład dla dysku:

```text
proces aktywny
-> zleca operację dyskową
-> przechodzi do stanu wstrzymanego
-> jego PCB trafia do kolejki procesów czekających na dysk
-> dysk kończy operację i zgłasza przerwanie
-> PCB procesu wraca do kolejki gotowych
```

Po zakończeniu I/O proces zwykle nie staje się od razu aktywny. Najpierw trafia do kolejki gotowych i dopiero planista może przydzielić mu CPU.

Pułapki:

- proces gotowy czeka na CPU,
- proces wstrzymany czeka na zdarzenie, np. dysk, semafor, dane z sieci,
- w kolejkach są PCB/wskaźniki do PCB, a nie pełne kopie procesów.

## Stany procesu

Najważniejsze stany:

| Stan | Sens |
|---|---|
| nowy | proces jest tworzony |
| gotowy | proces czeka na procesor |
| aktywny | proces wykonuje się na CPU |
| wstrzymany/zablokowany | proces czeka na zdarzenie, np. I/O |
| zakończony | proces skończył działanie |

Typowe przejścia:

- gotowy -> aktywny: planista przydziela CPU,
- aktywny -> gotowy: wywłaszczenie, koniec kwantu,
- aktywny -> wstrzymany: proces czeka na I/O lub zasób,
- wstrzymany -> gotowy: zdarzenie zaszło, np. zakończyło się I/O,
- aktywny -> zakończony: proces kończy działanie.

## Podział czasu

W systemie z podziałem czasu każdy proces dostaje kwant czasu procesora.

Po wykorzystaniu kwantu system może przerwać proces i wybrać inny.

Wymaga to:

- czasomierza,
- przerwań,
- przełączania kontekstu,
- planisty.

## Przełączenie kontekstu

**Przełączenie kontekstu** to zmiana procesu wykonywanego przez CPU.

System musi:

1. zapisać kontekst starego procesu,
2. wybrać nowy proces,
3. odtworzyć kontekst nowego procesu,
4. przekazać mu procesor.

Przełączenie kontekstu może być kosztowne, bo samo w sobie nie wykonuje pracy użytkownika.

## Przełączenie kontekstu a zmiana trybu

Tego nie wolno mylić:

| Pojęcie | Co się zmienia |
|---|---|
| zmiana trybu | tryb użytkownika <-> tryb jądra/uprzywilejowany |
| przełączenie kontekstu | wykonywany proces/wątek, np. proces A -> proces B |

Zmiana trybu może nastąpić np. przy:

- wywołaniu systemowym,
- przerwaniu,
- wyjątku.

Przełączenie kontekstu oznacza zapisanie stanu jednego procesu i odtworzenie stanu innego procesu.

Te rzeczy mogą wystąpić razem, ale nie są tym samym.

Przykład:

```text
proces A w user mode
-> przerwanie zegarowe
-> wejście do jądra, czyli zmiana trybu
-> jądro zapisuje kontekst A
-> jądro wybiera proces B
-> jądro odtwarza kontekst B
-> powrót do user mode procesu B
```

Pułapka egzaminacyjna:

> Przełączanie kontekstu polega na przełączeniu kontekstu wykonania między trybem uprzywilejowanym a trybem użytkownika.

Odpowiedź: **N**.

To jest opis zmiany trybu, a nie przełączenia kontekstu między procesami.

## Realizacja przełączenia kontekstu

Przełączenie kontekstu może być realizowane:

- sprzętowo,
- na poziomie systemu operacyjnego.

Sprzętowo, np. w architekturze x86, istnieją mechanizmy zadań procesora, takie jak:

- Task State Segment,
- deskryptor TSS,
- task gate,
- task register.

W takim modelu sprzęt może automatycznie zapisać część stanu starego zadania i załadować stan nowego.

W praktyce przełączanie kontekstu najczęściej realizuje system operacyjny, bo daje to większą elastyczność i przenośność.

Egzaminowo:

- przełączanie kontekstu może być realizowane sprzętowo: **T**,
- przełączanie kontekstu może być realizowane przez system operacyjny: **T**,
- przełączanie kontekstu polega na zmianie trybu user/kernel: **N**.

## Tryb użytkownika i tryb uprzywilejowany

System operacyjny potrzebuje ochrony, więc procesor ma co najmniej dwa tryby:

- **tryb użytkownika** - zwykłe programy,
- **tryb uprzywilejowany/jądra** - system operacyjny.

Instrukcje niebezpieczne, np. bezpośredni dostęp do urządzeń albo zmiana konfiguracji pamięci, są wykonywane w trybie uprzywilejowanym.

W x86 poziomy ochrony nazywa się ringami:

| Ring | Znaczenie | Typowe użycie |
|---|---|---|
| 0 | najbardziej uprzywilejowany | jądro systemu |
| 1 | pośredni | rzadko używany |
| 2 | pośredni | rzadko używany |
| 3 | najmniej uprzywilejowany | programy użytkownika |

W praktyce typowe systemy używają głównie:

```text
ring 0 = kernel mode
ring 3 = user mode
```

Sprzętowe wymagania ochrony:

- procesor musi mieć co najmniej tryb użytkownika i tryb uprzywilejowany,
- system operacyjny startuje i działa w trybie uprzywilejowanym,
- procesy użytkownika są uruchamiane w trybie użytkownika,
- przerwania, wyjątki i wywołania systemowe są obsługiwane przez system w trybie uprzywilejowanym,
- przed powrotem do programu użytkownika system przełącza tryb pracy na tryb użytkownika,
- niektóre rozkazy są uprzywilejowane.

Przykłady rozkazów/operacji uprzywilejowanych:

- zmiana tablic stron albo rejestrów ochrony pamięci,
- dostęp do urządzeń bezpośrednio przez porty,
- wyłączanie przerwań,
- zmiana trybu pracy,
- operacje na rejestrach systemowych.

Pułapka:

> Program użytkownika nie decyduje sam, że od teraz działa w trybie jądra. Wejście do jądra odbywa się kontrolowanym mechanizmem, np. przez syscall, przerwanie albo wyjątek.

## Wywołania systemowe

**Wywołanie systemowe** to kontrolowane wejście z programu użytkownika do jądra.

Przykłady:

- `read`,
- `write`,
- `open`,
- `fork`,
- `exec`,
- `wait`.

Funkcja systemowa wykonuje się w trybie jądra, nawet jeśli została wywołana przez proces użytkownika.

Mechanizm wygląda w skrócie tak:

```text
proces użytkownika
-> przygotowuje numer wywołania systemowego i argumenty
-> wykonuje instrukcję syscall/trap
-> CPU przechodzi do trybu uprzywilejowanego
-> jądro wybiera odpowiednią funkcję systemową po numerze
-> jądro wykonuje operację
-> wynik wraca do procesu
-> CPU wraca do trybu użytkownika
```

Przykład:

```c
read(fd, buf, 100);
```

Program użytkownika nie czyta dysku sam. Przez syscall prosi jądro, a jądro sprawdza uprawnienia, deskryptor, bufor i zleca właściwą operację.

Wywołanie systemowe nie jest zwykłym wywołaniem funkcji, bo zmienia poziom ochrony i wchodzi do kodu jądra.

### `fork`, `exec`, `wait`, `exit`

| Funkcja | Co robi |
|---|---|
| `fork` | tworzy nowy proces potomny jako kopię bieżącego |
| `exec` | zastępuje program w bieżącym procesie nowym programem |
| `wait` | rodzic czeka na zakończenie procesu potomnego |
| `exit` | kończy bieżący proces |

Najważniejsza para:

- `fork` tworzy nowy proces,
- `exec` **nie tworzy nowego procesu**, tylko podmienia obraz programu w obecnym procesie.

Typowy schemat shella:

```text
shell
-> fork: powstaje proces potomny
-> dziecko robi exec("program")
-> rodzic może zrobić wait
```

Po `exec` PID procesu zwykle zostaje ten sam, ale zmienia się kod programu i przestrzeń adresowa.

Pułapki:

- `fork` tworzy nowy proces: **T**,
- `exec` tworzy nowy proces: **N**,
- `exec` zastępuje obraz programu bieżącego procesu: **T**,
- `wait` czeka na zakończenie dziecka: **T**.

## Przerwania

Przerwanie powoduje chwilowe przerwanie normalnego wykonania programu i przejście do procedury obsługi.

Rodzaje:

- sprzętowe, np. od urządzenia,
- zegarowe, np. koniec kwantu,
- wyjątki, np. błąd dzielenia albo page fault,
- programowe, np. mechanizm syscall.

Przerwania sprzętowe są asynchroniczne względem programu, bo mogą pojawić się w dowolnym momencie.

Podział egzaminacyjny:

| Typ zdarzenia | Przykład | Charakter |
|---|---|---|
| przerwanie sprzętowe | klawiatura, dysk, timer | asynchroniczne |
| pułapka/wyjątek | syscall, dzielenie przez zero, page fault | synchroniczne |

**Asynchroniczne** oznacza, że zdarzenie nie wynika bezpośrednio z aktualnie wykonywanej instrukcji. Przykład: dysk kończy operację i zgłasza przerwanie.

**Synchroniczne** oznacza, że zdarzenie wynika z aktualnie wykonywanej instrukcji. Przykład: program wykonuje syscall albo próbuje dzielić przez zero.

Typowy przebieg obsługi przerwania:

1. procesor wykrywa przerwanie,
2. zwykle kończy cykl aktualnie wykonywanego rozkazu,
3. sprzęt zapisuje adres powrotu na stosie, czasem też część rejestrów/flag,
4. procesor przechodzi do procedury obsługi przerwania,
5. obsługa może wykonać kod jądra, zmienić stan procesu albo obudzić proces,
6. po obsłudze adres powrotu jest odtwarzany i trafia do licznika rozkazów.

Szczegóły zależą od sprzętu. Obsługa może używać osobnego stosu, zmieniać poziom ochrony albo doprowadzić do przełączenia kontekstu.

Pułapki:

- przerwanie sprzętowe jest asynchroniczne: **T**,
- wyjątek/programowa pułapka jest synchroniczna względem instrukcji: **T**,
- przerwanie zawsze musi przełączyć na inny proces: **N**,
- procesor zwykle nie urywa instrukcji w połowie: **T**,
- obsługa przerwania może zmienić poziom ochrony: **T**.

## Wejście-wyjście a procesy

Operacje I/O są dużo wolniejsze niż CPU.

Dlatego proces wykonujący I/O często przechodzi:

> aktywny -> wstrzymany -> gotowy

Po zakończeniu I/O przerwanie informuje system, że proces może wrócić do kolejki gotowych.

## Pułapki egzaminacyjne

- Proces to nie to samo co program.
- Gotowy nie znaczy aktywny.
- Wstrzymany proces nie czeka na CPU, tylko na zdarzenie.
- PCB zawiera licznik rozkazów i stan, ale nie całą zawartość pamięci procesu.
- Funkcja systemowa wykonuje się w trybie uprzywilejowanym.
- Procedura obsługi sygnału użytkownika wykonuje się w trybie użytkownika, nie jądra.
- Przerwanie zegarowe umożliwia wywłaszczanie.
- Zmiana trybu user/kernel to nie to samo co przełączenie kontekstu.
- W kolejkach procesów są PCB albo wskaźniki do PCB, nie pełna pamięć procesu.
- `fork` tworzy proces, a `exec` podmienia program w obecnym procesie.
- Przerwania sprzętowe są asynchroniczne, a wyjątki/programowe pułapki synchroniczne.

## Pytania T/N z odpowiedziami

1. Proces jest wykonaniem programu.  
   **T** - program staje się procesem podczas wykonania.

2. Program i proces znaczą dokładnie to samo.  
   **N** - program jest pasywny, proces aktywny.

3. PCB typowo zawiera stan procesu.  
   **T** - stan jest potrzebny do zarządzania procesem.

4. PCB typowo zawiera zapamiętaną wartość licznika rozkazów.  
   **T** - jest potrzebna do wznowienia procesu.

5. PCB zawiera pełną zawartość wszystkich stron procesu.  
   **N** - zawiera metadane, nie kopię całej pamięci procesu.

6. Proces gotowy czeka na procesor.  
   **T** - jest gotowy do wykonania, ale CPU ma ktoś inny.

7. Proces wstrzymany czeka na zdarzenie, np. zakończenie I/O.  
   **T** - nie może od razu dostać CPU.

8. Wywołanie systemowe wykonuje kod jądra w trybie uprzywilejowanym.  
   **T** - to kontrolowane przejście do systemu operacyjnego.

9. Koniec operacji I/O może przenieść proces ze wstrzymanego do gotowego.  
   **T** - proces może potem czekać na CPU.

10. Przełączenie kontekstu zawsze polega tylko na zmianie trybu użytkownika na tryb jądra.  
    **N** - chodzi o zmianę kontekstu wykonania między procesami lub wątkami.

11. System operacyjny umieszcza PCB procesów w kolejce gotowych.  
    **T** - kolejka gotowych zawiera procesy gotowe do dostania CPU.

12. Proces wstrzymany na dysku znajduje się w kolejce gotowych.  
    **N** - znajduje się w kolejce czekających na dysk lub dane zdarzenie.

13. Przerwanie sprzętowe jest zdarzeniem asynchronicznym.  
    **T** - może pojawić się niezależnie od aktualnie wykonywanej instrukcji.

14. Wyjątek spowodowany dzieleniem przez zero jest synchroniczny względem instrukcji.  
    **T** - wynika z aktualnie wykonywanego rozkazu.

15. Funkcja systemowa wykonuje się w trybie użytkownika, bo wywołuje ją proces użytkownika.  
    **N** - kod jądra obsługujący syscall wykonuje się w trybie uprzywilejowanym.

16. `fork` tworzy nowy proces.  
    **T** - proces potomny jest kopią procesu rodzica.

17. `exec` tworzy nowy proces potomny.  
    **N** - `exec` podmienia obraz programu w bieżącym procesie.

18. `wait` może służyć rodzicowi do czekania na zakończenie dziecka.  
    **T** - to typowe użycie `wait`.

19. Program użytkownika może dowolnie wykonać wszystkie rozkazy uprzywilejowane.  
    **N** - rozkazy uprzywilejowane są zarezerwowane dla trybu jądra.

20. W x86 ring 0 jest bardziej uprzywilejowany niż ring 3.  
    **T** - ring 0 to typowo jądro, ring 3 to programy użytkownika.
