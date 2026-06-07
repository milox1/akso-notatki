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

## Systemy czasu rzeczywistego

W systemach real-time ważne są terminy.

Pojęcia:

- termin uwolnienia - kiedy zadanie może wejść do kolejki,
- termin ostateczny - do kiedy musi skończyć,
- zadanie okresowe - pojawia się cyklicznie,
- hard real-time - przekroczenie terminu jest błędem krytycznym,
- soft real-time - przekroczenie pogarsza jakość, ale nie musi niszczyć systemu.

**Szeregowanie synchroniczne** dzieli czas na ramy i układa statyczny plan przed uruchomieniem.

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
