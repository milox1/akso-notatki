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

## Tryb użytkownika i tryb uprzywilejowany

System operacyjny potrzebuje ochrony, więc procesor ma co najmniej dwa tryby:

- **tryb użytkownika** - zwykłe programy,
- **tryb uprzywilejowany/jądra** - system operacyjny.

Instrukcje niebezpieczne, np. bezpośredni dostęp do urządzeń albo zmiana konfiguracji pamięci, są wykonywane w trybie uprzywilejowanym.

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

## Przerwania

Przerwanie powoduje chwilowe przerwanie normalnego wykonania programu i przejście do procedury obsługi.

Rodzaje:

- sprzętowe, np. od urządzenia,
- zegarowe, np. koniec kwantu,
- wyjątki, np. błąd dzielenia albo page fault,
- programowe, np. mechanizm syscall.

Przerwania sprzętowe są asynchroniczne względem programu, bo mogą pojawić się w dowolnym momencie.

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
