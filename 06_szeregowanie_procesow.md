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

Cechy:

- wywłaszczające,
- dobre dla systemów z podziałem czasu,
- wymaga czasomierza,
- mocno zależy od długości kwantu.

Kwant:

- za mały -> dużo przełączeń kontekstu,
- za duży -> RR zbliża się do FCFS,
- bardzo mały teoretycznie zbliża się do processor sharing, ale praktycznie koszt przełączeń byłby za duży.

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

