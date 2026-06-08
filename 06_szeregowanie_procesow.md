# AKSO - szeregowanie procesów

## Najkrócej

**Szeregowanie** to wybór procesu, który ma dostać procesor.

Najważniejsze algorytmy egzaminacyjne:

- FCFS/FIFO,
- SJF,
- SRTF,
- priorytetowe,
- RR, czyli round-robin,
- wielopoziomowe,
- wielopoziomowe ze sprzężeniem zwrotnym.

## Rodzaje planowania

| Rodzaj | Co robi |
|---|---|
| długoterminowe | decyduje, które zadania wprowadzić do pamięci |
| krótkoterminowe | wybiera następny proces do wykonania na CPU |
| średnioterminowe | może czasowo usuwać procesy z pamięci, np. swapping |

Na egzaminie najczęściej chodzi o **szeregowanie krótkoterminowe**.

## Wywłaszczające i niewywłaszczające

**Niewywłaszczające**: proces oddaje CPU sam, np. kończy fazę CPU albo blokuje się na I/O.

**Wywłaszczające**: system może przerwać proces, np. po końcu kwantu albo po pojawieniu się ważniejszego procesu.

Wywłaszczanie wymaga przerwań zegarowych i powoduje potrzebę synchronizacji danych współdzielonych.

## Miary

| Miara | Sens |
|---|---|
| czas obrotu | od przybycia do zakończenia |
| czas oczekiwania | łączny czas w kolejce gotowych |
| czas odpowiedzi | od przybycia do pierwszego dostania CPU |
| wykorzystanie CPU | procent czasu użytecznej pracy CPU |
| przepustowość | liczba zakończonych procesów na jednostkę czasu |

Wzór:

> czas obrotu = czas zakończenia - czas przybycia

## FCFS / FIFO

**First-Come First-Served**: kto pierwszy przyjdzie, ten pierwszy dostaje CPU.

Cechy:

- proste,
- niewywłaszczające,
- może mieć długi średni czas oczekiwania,
- podatne na efekt konwoju.

Efekt konwoju: krótki proces czeka długo za długim procesem.

## SJF

**Shortest Job First**: najpierw proces z najkrótszą następną fazą CPU.

Cechy:

- niewywłaszczające,
- minimalizuje średni czas oczekiwania przy znanych czasach faz,
- w praktyce trzeba przewidywać długość następnej fazy,
- może prowadzić do zagłodzenia długich procesów.

## SRTF

**Shortest Remaining Time First** to wywłaszczająca wersja SJF.

Jeśli przyjdzie nowy proces z krótszym pozostałym czasem niż aktualnie wykonywany, system może wywłaszczyć obecny proces.

SRTF zawsze wybiera proces, któremu zostało najmniej czasu do końca aktualnej fazy CPU.

Przykład:

```text
P1 przychodzi w t=0, potrzebuje 8
P2 przychodzi w t=2, potrzebuje 3
```

Od `t=0` do `t=2` działa `P1`, więc zostaje mu `6`.

W `t=2` przychodzi `P2`:

```text
P1 zostało 6
P2 potrzebuje 3
```

SRTF wybiera `P2`, bo ma krótszy pozostały czas. `P1` zostaje wywłaszczony.

Diagram:

```text
0   2   5       11
| P1 | P2 |  P1  |
```

Cechy:

- wywłaszczające,
- dobre dla krótkich zadań,
- może zagładzać długie zadania,
- wymaga wiedzy lub estymacji pozostałego czasu.

## Szeregowanie priorytetowe

Proces z najwyższym priorytetem dostaje CPU.

Może być:

- wywłaszczające,
- niewywłaszczające.

Problem: procesy o niskim priorytecie mogą głodować.

Rozwiązanie: **postarzanie**, czyli stopniowe zwiększanie priorytetu długo czekających procesów.

## Zagłodzenie

**Zagłodzenie** oznacza, że proces istnieje i czeka, ale przez politykę planisty bardzo długo albo nigdy nie dostaje zasobu.

Najczęściej chodzi o CPU, ale ten sam problem może dotyczyć np. pamięci, dysku albo blokady.

Przykład:

```text
P1 ma niski priorytet.
Ciągle przychodzą procesy o wysokim priorytecie.
Planista ciągle wybiera procesy wysokopriorytetowe.
P1 czeka bez końca.
```

Zagłodzenie to nie to samo co zakleszczenie.

| Problem | Sens |
|---|---|
| zagłodzenie | proces jest pomijany przez politykę przydziału zasobu |
| zakleszczenie | procesy czekają na siebie nawzajem w cyklu |

Sposób ograniczania zagłodzenia: **postarzanie**.

## Round-robin

**RR**: każdy proces dostaje kwant czasu, potem wraca na koniec kolejki, jeśli się nie zakończył.

Algorytm:

1. Procesy stoją w kolejce gotowych.
2. Pierwszy proces dostaje CPU na maksymalnie jeden kwant.
3. Jeśli skończy wcześniej, znika z kolejki.
4. Jeśli nie skończy, po końcu kwantu trafia na koniec kolejki.
5. Następny proces z kolejki dostaje CPU.

Przykład dla kwantu `3`:

```text
P1: 8
P2: 4
P3: 2
```

Wykonanie:

```text
P1 robi 3, zostaje 5 -> na koniec
P2 robi 3, zostaje 1 -> na koniec
P3 robi 2, kończy
P1 robi 3, zostaje 2 -> na koniec
P2 robi 1, kończy
P1 robi 2, kończy
```

Diagram:

```text
0   3   6   8   11  12  14
|P1|P2|P3| P1 |P2| P1 |
```

Cechy:

- wywłaszczające,
- dobre dla systemów z podziałem czasu,
- wymaga czasomierza,
- mocno zależy od długości kwantu.

Kwant:

- za mały -> dużo przełączeń kontekstu,
- za duży -> RR zbliża się do FCFS,
- bardzo mały teoretycznie zbliża się do processor sharing, ale praktycznie koszt przełączeń byłby za duży.

### Narzut przełączania kontekstu

**Narzut przełączania kontekstu** to czas tracony na samo przełączenie CPU z jednego procesu na drugi.

W tym czasie system robi techniczne rzeczy:

- zapisuje rejestry starego procesu,
- zapisuje licznik rozkazów,
- aktualizuje PCB,
- wybiera następny proces,
- odtwarza rejestry nowego procesu,
- przełącza informacje o pamięci, jeśli trzeba.

To nie jest użyteczna praca programu użytkownika.

Przykład:

```text
kwant = 100 ms
context switch = 1 ms
```

Narzut jest mały.

```text
kwant = 2 ms
context switch = 1 ms
```

Narzut jest bardzo duży, bo system często przełącza procesy i traci dużo czasu na administrację.

Egzaminowo:

> Zbyt mały kwant w RR zwiększa narzut przełączania kontekstu.

Odpowiedź: **T**.

## Procesy CPU-bound i I/O-bound

**CPU-bound**: proces ma długie fazy procesora i rzadko czeka na I/O.

Przykłady:

- obliczenia numeryczne,
- kompresja,
- renderowanie.

**I/O-bound**: proces często potrzebuje wejścia-wyjścia i ma krótkie fazy CPU.

Przykłady:

- program interaktywny,
- proces często czytający z dysku,
- obsługa sieci.

Pułapka:

```text
proces ograniczany przez I/O często potrzebuje faz wejścia-wyjścia
i zwykle nie potrzebuje długich faz CPU
```

Dlatego odpowiedzi:

- często potrzebuje faz wejścia-wyjścia - **TAK**,
- potrzebuje długich faz procesora - zwykle **NIE**,
- nie jest procesem interaktywnym - **NIE**, bo procesy I/O-bound często są interaktywne.

## Szeregowanie wielopoziomowe

Procesy są w różnych kolejkach, np.:

- systemowe,
- interaktywne,
- wsadowe.

Każda kolejka może mieć własny algorytm. Między kolejkami też musi być reguła wyboru.

## Kolejki ze sprzężeniem zwrotnym

Proces może zmieniać kolejkę.

Typowa idea:

- procesy interaktywne zostają wyżej,
- procesy zużywające dużo CPU spadają niżej,
- można stosować postarzanie, żeby uniknąć zagłodzenia.

Przykład:

```text
priorytet 0: RR, kwant = 10
priorytet 1: RR, kwant = 20
priorytet 2: FCFS
```

Jeśli proces zużyje cały kwant, może spaść do kolejki o niższym priorytecie.

Jeśli proces nie wykorzystuje całego kwantu, bo np. szybko przechodzi na I/O, zwykle zostaje wyżej. To premiuje procesy interaktywne.

Ważne:

- procesy nie są przypisane na stałe do jednej kolejki,
- algorytm obserwuje zachowanie procesu,
- proces CPU-bound może spadać niżej,
- proces I/O-bound/interaktywny może zostawać wyżej.

## Szeregowanie loteryjne

W szeregowanie loteryjnym procesy dostają **losy** na zasoby, np. na CPU.

System co jakiś czas urządza losowanie. Zwycięski los wskazuje proces, który dostanie zasób na pewien czas.

Przykład:

```text
P1 ma 1 los
P2 ma 3 losy
P3 ma 6 losów
```

`P3` ma największą szansę dostać CPU, ale `P1` też może wygrać.

Zalety:

- proste,
- daje statystycznie sprawiedliwy podział,
- łatwo robić priorytety przez liczbę losów,
- można przydzielać czas użytkownikom, a nie pojedynczym procesom,
- współpracujące procesy mogą przekazywać sobie losy.

Pułapka:

> Sprawiedliwość loteryjna jest statystyczna, nie gwarantowana w każdym krótkim przedziale czasu.

## Systemy czasu rzeczywistego

W systemach real-time ważne jest nie tylko to, czy wynik jest poprawny, ale też czy pojawił się na czas.

Pojęcia:

- termin uwolnienia - chwila, gdy zadanie dołącza do kolejki gotowych,
- czas wykonania - maksymalny czas trwania wykonania zadania,
- czas reakcji - czas od uwolnienia zadania do zakończenia jego wykonania,
- termin ostateczny, czyli deadline - do kiedy zadanie musi skończyć,
- okres zadania - co ile czasu pojawia się kolejne uruchomienie zadania okresowego.

Nie mylić:

| Pojęcie | Sens |
|---|---|
| czas wykonania | ile CPU potrzebuje zadanie |
| czas reakcji | czekanie + wykonanie |
| okres | co ile zadanie pojawia się ponownie |
| deadline | do kiedy zadanie ma skończyć |

Przykład:

```text
zadanie pojawia się w t = 10 ms
potrzebuje 4 ms CPU
kończy się w t = 17 ms
deadline ma w t = 20 ms
```

Wtedy:

```text
termin uwolnienia = 10 ms
czas wykonania = 4 ms
czas reakcji = 17 - 10 = 7 ms
termin ostateczny = 20 ms
```

### Hard i soft real-time

**Hard real-time**: zadanie nie może przekroczyć deadline'u. Przekroczenie terminu oznacza błąd systemu.

Przykłady:

- sterowanie hamulcem,
- airbag,
- sterownik medyczny,
- kontrola lotu.

**Soft real-time**: deadline jest ważny, ale czasem może zostać przekroczony. System działa gorzej, ale niekoniecznie krytycznie się psuje.

Przykłady:

- streaming,
- audio,
- gry,
- wideokonferencja.

UDP/TCP nie definiują hard/soft real-time. UDP bywa używany w soft real-time, ale `UDP = soft` i `TCP = hard` to niepoprawna definicja.

### Rodzaje zadań real-time

| Rodzaj | Sens |
|---|---|
| okresowe | kolejne terminy uwolnienia są odległe o stały okres |
| sporadyczne | nie są okresowe i mają sztywne deadline'y |
| nieokresowe | nie są okresowe i mają elastyczne deadline'y |

Okresowe zadanie jest intuicyjnie podobne do cron joba: wraca co stały czas. Różnica jest taka, że w real-time zwykle mówimy o milisekundach i deadline'ach.

### Sensowność i optymalność

Przydział procesora jest **sensowny**, jeśli da się zagwarantować, że wszystkie zadania zakończą się przed swoimi deadline'ami.

Algorytm jest **optymalny w danej klasie**, jeśli zawsze wtedy, gdy jakiś algorytm z tej klasy potrafi uszeregować zadania sensownie, ten algorytm też potrafi.

To nie znaczy, że ma najmniejszy narzut albo jest najprostszy. Chodzi o zdolność dotrzymania deadline'ów.

Oznaczenia ze slajdów:

| Symbol | Sens |
|---|---|
| `e_i` | czas wykonania zadania `i` |
| `p_i` | okres zadania `i` |
| `e_i / p_i` | udział zadania w czasie CPU |

Jeśli suma `e_i / p_i` jest większa niż `1`, to na jednym CPU jest więcej pracy niż czasu.

### Szeregowanie synchroniczne

**Szeregowanie synchroniczne** dzieli czas procesora na przedziały o ustalonej długości, czyli **ramy**.

Program dzieli się na zadania, a tablica szeregująca z góry przypisuje zadania do ram.

Przykład:

```text
rama = 10 ms

0-10 ms    zadanie A
10-20 ms   zadanie B
20-30 ms   zadanie A
30-40 ms   zadanie C
```

Tablica szeregująca:

- zawiera przypisanie zadań do ram,
- jest konstruowana wcześniej, czyli a priori,
- daje bardzo przewidywalne zachowanie.

Najważniejszy warunek:

> Zadanie nawet w najgorszym przypadku musi zakończyć się w ciągu jednej ramy.

Jeśli zadanie nie mieści się w jednej ramie, programista/projektant musi je podzielić na części.

Jeśli zmieni się:

- rozmiar ramy,
- kod zadania,
- najgorszy czas wykonania zadania,

to trzeba ponownie sprawdzić harmonogram.

Zalety:

- bardzo przewidywalne,
- dobre do hard real-time,
- mały narzut działania planisty,
- łatwo analizować przed uruchomieniem.

Wady:

- mało elastyczne,
- trzeba znać najgorszy czas wykonania zadań,
- zmiana kodu może wymagać przebudowy tablicy,
- jeśli zadanie skończy wcześniej, reszta ramy może się zmarnować.

### Szeregowanie asynchroniczne

W szeregowanie asynchronicznym nie ma sztywnej tablicy ramek. Scheduler decyduje w trakcie pracy systemu.

Wersja bez wywłaszczania:

```text
zadanie działa do zakończenia
dopiero potem wybierane jest następne
```

Problem: jeśli pojawi się pilniejsze zadanie, może czekać zbyt długo i przekroczyć deadline.

W praktyce często stosuje się:

```text
szeregowanie asynchroniczne + priorytety + wywłaszczanie
```

Wywłaszczanie oznacza, że mniej ważne zadanie może zostać przerwane przez zadanie ważniejsze.

### Sekcja krytyczna i odwrócenie priorytetów

**Sekcja krytyczna** to fragment kodu korzystający ze wspólnego zasobu, którego nie powinno wykonywać naraz kilka zadań.

Przykłady zasobów:

- bufor,
- zmienna globalna,
- plik,
- struktura danych,
- urządzenie.

Odwrócenie priorytetów:

```text
zadanie niskiego priorytetu trzyma lock
zadanie wysokiego priorytetu potrzebuje tego locka
zadanie wysokiego priorytetu musi czekać
```

Jeśli dodatkowo zadanie średniego priorytetu wywłaszcza zadanie niskiego priorytetu, problem robi się gorszy:

```text
wysoki czeka na niski
niski nie może skończyć, bo działa średni
średni opóźnia wysoki, mimo że nie trzyma locka
```

To nie jest automatycznie zakleszczenie.

| Problem | Sens |
|---|---|
| odwrócenie priorytetów | wysoki priorytet czeka, bo niski trzyma zasób |
| zakleszczenie | procesy czekają na siebie nawzajem w cyklu |
| zagłodzenie | proces długo nie dostaje zasobu przez politykę przydziału |

### Dziedziczenie priorytetu

**Dziedziczenie priorytetu** ogranicza odwrócenie priorytetów.

Jeśli zadanie niskiego priorytetu trzyma zasób potrzebny zadaniu wysokiego priorytetu, to tymczasowo dostaje wysoki priorytet.

Dzięki temu:

```text
niski szybko kończy sekcję krytyczną
oddaje lock
wysoki może działać
średni nie opóźnia niepotrzebnie wysokiego
```

Ten wyższy priorytet jest tymczasowy, tylko na czas trzymania zasobu.

### Wartownik

**Wartownik** to specjalne zadanie kontrolne.

Cechy:

- zwykle ma najwyższy priorytet,
- wykonuje się okresowo,
- sprawdza, czy inne zadania wykonują się dostatecznie często.

Może np. wykrywać, że zadanie real-time przestało się uruchamiać albo przekracza terminy.

### RM i EDF

**RM**, czyli rate monotonic:

```text
im krótszy okres zadania, tym wyższy priorytet
```

Cechy RM:

- stałe priorytety,
- optymalny w klasie algorytmów ze stałymi priorytetami,
- intuicja: zadania, które pojawiają się częściej, są ważniejsze.

Warunek wystarczający ze slajdów:

```text
suma(e_i / p_i) <= n(2^(1/n) - 1)
```

**EDF**, czyli earliest deadline first:

```text
im bliższy deadline, tym wyższy priorytet
```

Cechy EDF:

- dynamiczne priorytety,
- wybiera zadanie z najbliższym terminem ostatecznym,
- optymalny w klasie algorytmów szeregujących przy typowych założeniach.

Warunek ze slajdów:

```text
suma(e_i / p_i) <= 1
```

Porównanie:

| Algorytm | Priorytety | Zasada |
|---|---|---|
| RM | stałe | krótszy okres = wyższy priorytet |
| EDF | dynamiczne | bliższy deadline = wyższy priorytet |

## Klasy systemów a dobór algorytmu

Algorytm szeregowania dobiera się do typu systemu.

| System | Co jest ważne |
|---|---|
| wsadowy | przepustowość, mniej przełączeń kontekstu |
| interakcyjny | szybka reakcja, brak monopolizacji CPU |
| czasu rzeczywistego | dotrzymanie deadline'ów |

W systemach wsadowych proces może dostać CPU na dłużej, bo użytkownik nie czeka aktywnie przy terminalu.

W systemach interakcyjnych potrzebne jest wywłaszczanie, bo żaden proces nie powinien zmonopolizować CPU.

W systemach czasu rzeczywistego najważniejsze jest to, czy zadania zdążą przed deadline'ami.

## Pułapki egzaminacyjne

- FCFS jest niewywłaszczające.
- SJF jest niewywłaszczające, SRTF wywłaszczające.
- RR wymaga kwantu i czasomierza.
- Duży kwant RR upodabnia RR do FCFS.
- Mały kwant zwiększa narzut przełączania kontekstu.
- Proces po wykorzystaniu całego kwantu w RR wraca na koniec kolejki, jeśli się nie zakończył.
- SJF minimalizuje średni czas oczekiwania, jeśli znamy czasy faz.
- Priorytety mogą prowadzić do zagłodzenia.
- Postarzanie przeciwdziała zagłodzeniu.
- W real-time chodzi o dotrzymanie terminów, nie tylko o średni czas.
- Proces I/O-bound często potrzebuje I/O i zwykle ma krótkie fazy CPU.
- W szeregowanie niewywłaszczającym przejście `aktywny -> gotowy` nie powinno wynikać z odebrania CPU przez planistę.
- `gotowy` nie znaczy `zakończony`.
- W RR proces po wykorzystaniu kwantu i braku zakończenia wraca do kolejki gotowych.
- Szeregowanie loteryjne daje sprawiedliwość statystyczną, nie idealną w każdym momencie.
- Hard real-time nie oznacza "szybkie", tylko "musi dotrzymać deadline'u".
- Soft real-time nie oznacza braku deadline'ów.
- UDP nie jest definicją soft real-time, a TCP nie jest definicją hard real-time.
- W synchronicznym real-time tablica szeregująca jest konstruowana a priori.
- W synchronicznym real-time zadanie musi zmieścić się w ramie w najgorszym przypadku.
- Zmiana kodu zadania może wymagać ponownej analizy harmonogramu.
- Asynchroniczne nie znaczy automatycznie wywłaszczające; wywłaszczanie to osobna cecha.
- Sekcja krytyczna może opóźnić zadanie wysokiego priorytetu.
- Odwrócenie priorytetów to nie to samo co zakleszczenie.
- Dziedziczenie priorytetu jest tymczasowe.
- RM ma stałe priorytety: krótszy okres oznacza wyższy priorytet.
- EDF ma dynamiczne priorytety: bliższy deadline oznacza wyższy priorytet.
- `e_i / p_i` oznacza udział zadania w obciążeniu CPU.

## Pytania T/N z odpowiedziami

1. FCFS jest algorytmem niewywłaszczającym.
   **T** - proces oddaje CPU dopiero po zakończeniu fazy lub blokadzie.

2. SRTF jest wywłaszczającą wersją SJF.
   **T** - może przerwać proces, gdy pojawi się krótszy.

3. SJF wymaga znajomości lub przewidywania długości następnej fazy CPU.
   **T** - bez tego nie wiadomo, który proces jest najkrótszy.

4. RR z bardzo dużym kwantem zachowuje się podobnie do FCFS.
   **T** - proces rzadko jest wywłaszczany z powodu kwantu.

5. RR nie wymaga wsparcia sprzętowego czasomierza.
   **N** - czasomierz jest potrzebny do końca kwantu.

5a. Proces w RR, który wykorzystał cały kwant i nie skończył działania, wraca na koniec kolejki.
   **T** - to podstawowa zasada szeregowania rotacyjnego.

5b. Zbyt mały kwant w RR może powodować duży narzut przełączania kontekstu.
   **T** - CPU traci dużo czasu na przełączanie procesów.

5c. SRTF może wywłaszczyć proces, gdy pojawi się nowy proces z krótszym pozostałym czasem.
   **T** - dlatego SRTF jest wywłaszczającą wersją SJF.

6. Postarzanie może przeciwdziałać zagłodzeniu.
   **T** - długo czekający proces zyskuje priorytet.

7. Czas obrotu to czas od przybycia procesu do jego zakończenia.
   **T** - to podstawowa definicja.

8. Czas oczekiwania obejmuje czas wykonywania na CPU.
   **N** - chodzi o czas czekania w kolejce gotowych.

9. Szeregowanie synchroniczne real-time wymaga przygotowania planu/ram.
   **T** - plan jest ustalany przed działaniem systemu.

10. Hard real-time oznacza, że przekroczenie terminu jest akceptowalne, jeśli średnio system działa szybko.
    **N** - w hard real-time termin musi być dotrzymany.

11. Proces ograniczany przez I/O często potrzebuje faz wejścia-wyjścia.
    **T** - dlatego często blokuje się na I/O i oddaje CPU.

12. Proces ograniczany przez I/O zwykle potrzebuje długich faz procesora.
    **N** - zwykle ma krótkie fazy CPU i częste I/O.

13. W szeregowanie niewywłaszczającym przejście `aktywny -> gotowy` przez odebranie CPU jest typowe.
    **N** - takie odebranie CPU to wywłaszczenie.

14. W kolejkach ze sprzężeniem zwrotnym procesy mogą zmieniać kolejkę.
    **T** - algorytm reaguje na zachowanie procesu.

15. Proces CPU-bound może spaść do kolejki o niższym priorytecie.
    **T** - bo często wykorzystuje całe kwanty CPU.

16. Szeregowanie loteryjne może realizować priorytety przez liczbę losów.
    **T** - więcej losów oznacza większą szansę na CPU.

17. Sprawiedliwość w szeregowanie loteryjnym jest gwarantowana idealnie w każdym krótkim odcinku czasu.
    **N** - jest statystyczna, czyli ujawnia się w dłuższym czasie.

18. Deadline to czas wykonania zadania.
    **N** - deadline to termin, do którego zadanie ma się zakończyć.

19. Okres zadania to odstęp między kolejnymi uwolnieniami zadania okresowego.
    **T** - jak cron job: co pewien ustalony czas.

20. Czas reakcji może być większy od czasu wykonania.
    **T** - bo obejmuje także czekanie na CPU.

21. W soft real-time deadline'y nie istnieją.
    **N** - istnieją, ale ich przekroczenie może być dopuszczalne.

22. Zadanie sporadyczne jest nieokresowe i ma sztywny deadline.
    **T** - to klasyczna definicja ze slajdów.

23. Szeregowanie synchroniczne opiera się na tablicy przygotowanej a priori.
    **T** - zadania są wcześniej przypisane do ram.

24. W szeregowanie synchronicznym zadanie może przekroczyć ramę, jeśli potem nadrobi.
    **N** - zadanie ma zmieścić się w ramie nawet w najgorszym przypadku.

25. Szeregowanie asynchroniczne oznacza automatycznie wywłaszczanie.
    **N** - synchroniczne/asynchroniczne i wywłaszczające/niewywłaszczające to osobne osie.

26. Sekcja krytyczna może powodować opóźnienie zadania wysokiego priorytetu.
    **T** - jeśli zasób trzyma zadanie niższego priorytetu.

27. Odwrócenie priorytetów to to samo co zakleszczenie.
    **N** - odwrócenie priorytetów to opóźnienie przez lock i priorytety, niekoniecznie cykl oczekiwania.

28. Dziedziczenie priorytetu może ograniczać odwrócenie priorytetów.
    **T** - niski priorytet tymczasowo dostaje wyższy, żeby oddać zasób.

29. W RM krótszy okres zadania oznacza wyższy priorytet.
    **T** - RM ma stałe priorytety wynikające z okresu.

30. W EDF priorytety są stałe i zależą tylko od okresu zadania.
    **N** - EDF ma dynamiczne priorytety zależne od najbliższego deadline'u.

31. Jeśli `suma(e_i / p_i) > 1`, jeden procesor ma więcej pracy niż czasu.
    **T** - obciążenie przekracza 100%.
