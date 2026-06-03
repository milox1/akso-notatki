# AKSO - drzewo nauczania do egzaminu

Egzamin: **19 czerwca 2026**.  
Stan na **3 czerwca 2026**: masz 16 dni nauki razem z dniem egzaminu, praktycznie 15 dni roboczej nauki do 18 czerwca.

## Jak używać drzewa

Nie ucz się liniowo wszystkich slajdów. Idź po gałęziach:

1. Najpierw rozumiesz węzeł.
2. Potem robisz 5-10 pytań T/N z tego węzła.
3. Potem robisz jedno zadanie, jeśli węzeł jest obliczeniowy.
4. Dopiero wtedy idziesz niżej.

Statusy:

- `[ ]` nie ruszone,
- `[~]` kojarzę, ale mylę pułapki,
- `[x]` umiem odpowiedzieć T/N i wyjaśnić czemu.

## Drzewo główne

```text
AKSO / SO
|
+-- 1. Fundament sprzętowy
|   |
|   +-- architektura vs organizacja
|   +-- von Neumann / Harvard
|   +-- bity, U2, flagi
|   +-- x86, tryby adresowania
|   +-- RISC/CISC
|   +-- potok, hazardy, forwarding
|   +-- architektury równoległe
|
+-- 2. Proces jako obiekt SO
|   |
|   +-- proces vs program
|   +-- przestrzeń adresowa: text/data/heap/stack
|   +-- PCB
|   +-- stany procesu
|   +-- przerwania
|   +-- syscall
|   +-- przełączenie kontekstu
|
+-- 3. Szeregowanie
|   |
|   +-- FCFS
|   +-- SJF
|   +-- SRTF
|   +-- RR
|   +-- priorytety i postarzanie
|   +-- real-time
|   +-- zadania z czasem obrotu/oczekiwania
|
+-- 4. Synchronizacja i zakleszczenia
|   |
|   +-- sekcja krytyczna
|   +-- semafory
|   +-- monitory Hoare/Mesa
|   +-- warunki zakleszczenia
|   +-- graf zasobów
|   +-- stan bezpieczny
|   +-- bankier / wykrywanie
|
+-- 5. Pamięć
|   |
|   +-- adresy logiczne/fizyczne
|   +-- wiązanie adresów
|   +-- fragmentacja
|   +-- przydział ciągły
|   +-- buddy allocator
|   +-- stronicowanie
|   +-- TLB
|   +-- page fault
|   +-- FIFO/LRU/OPT
|   +-- thrashing
|   +-- mmap / copy-on-write
|
+-- 6. Wejście-wyjście i pliki
    |
    +-- polling / przerwania / DMA
    +-- dyski i strategie dostępu
    +-- plik / katalog / dowiązanie
    +-- i-węzeł
    +-- deskryptor pliku
    +-- tablica otwartych plików
    +-- VFS / v-węzeł
    +-- ext2 i bloki pośrednie
```

## Kolejność zależności

### Najpierw sprzęt

Bez tego potykasz się później na pamięci i przerwaniach.

Do ogarnięcia:

- [x] architektura vs organizacja,
- [x] von Neumann,
- [~] Harvard,
- [~] flagi `CF/ZF/OF/SF`,
- [~] RISC/CISC,
- [~] potok `F/D/E/M/W`,
- [~] forwarding i load-use,
- [~] superskalarne/SIMD/VLIW/EPIC/SMP/NUMA.

Notatki:

- [01_architektura_vs_organizacja.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/01_architektura_vs_organizacja.md)
- [02_architektura_von_neumanna.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/02_architektura_von_neumanna.md)
- [03_architektura_procesora_risc_cisc_potok.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/03_architektura_procesora_risc_cisc_potok.md)
- [04_bity_x86_assembler.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/04_bity_x86_assembler.md)

### Potem procesy

Proces jest centrum systemu operacyjnego. Szeregowanie, pamięć, pliki i I/O są potem “dla procesu”.

Do ogarnięcia:

- [~] proces vs program,
- [~] przestrzeń adresowa,
- [~] PCB,
- [~] stany procesu,
- [~] przełączenie kontekstu,
- [ ] syscall i tryby pracy,
- [ ] przerwania.

Notatka:

- [05_procesy_stany_syscall_przerwania.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/05_procesy_stany_syscall_przerwania.md)

### Potem algorytmy CPU

Tu wchodzą zadania obliczeniowe.

Do ogarnięcia:

- [ ] FCFS,
- [ ] SJF,
- [ ] SRTF,
- [ ] RR,
- [ ] czas obrotu,
- [ ] czas oczekiwania,
- [ ] czas odpowiedzi,
- [ ] real-time.

Notatka:

- [06_szeregowanie_procesow.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/06_szeregowanie_procesow.md)

### Potem synchronizacja

Tu egzamin lubi bardzo precyzyjne T/N.

Do ogarnięcia:

- [ ] sekcja krytyczna,
- [ ] safety vs liveness,
- [ ] aktywne oczekiwanie,
- [ ] semafor silny/słaby,
- [ ] monitor,
- [ ] Hoare vs Mesa,
- [ ] zakleszczenia,
- [ ] bankier.

Notatki:

- [12_synchronizacja_semafory_monitory.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/12_synchronizacja_semafory_monitory.md)
- [07_zakleszczenia_bankier.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/07_zakleszczenia_bankier.md)

### Potem pamięć

Największa gałąź i najwięcej obliczeń.

Do ogarnięcia:

- [ ] adres logiczny/fizyczny,
- [ ] translacja adresów,
- [ ] fragmentacja,
- [ ] buddy,
- [ ] strona vs ramka,
- [ ] tablica stron,
- [ ] TLB,
- [ ] page fault,
- [ ] FIFO/LRU/OPT,
- [ ] thrashing,
- [ ] copy-on-write,
- [ ] mmap.

Notatki:

- [10_pamiec_wiazanie_fragmentacja.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/10_pamiec_wiazanie_fragmentacja.md)
- [11_stronicowanie_tlb_wirtualna.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/11_stronicowanie_tlb_wirtualna.md)

### Na końcu I/O i pliki

To jest dużo definicji i parę zadań z i-węzłów/ext2.

Do ogarnięcia:

- [ ] polling,
- [ ] przerwania,
- [ ] DMA,
- [ ] dyski,
- [ ] pliki/katalogi,
- [ ] i-węzły,
- [ ] deskryptory,
- [ ] VFS,
- [ ] bloki pośrednie,
- [ ] ext2.

Notatki:

- [08_io_dyski_dma.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/08_io_dyski_dma.md)
- [09_system_plikow_vfs_inode_deskryptory.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/09_system_plikow_vfs_inode_deskryptory.md)

## Plan do 19 czerwca

### 3-6 czerwca: fundament i procesy

Cel: rozumieć sprzęt, potok, proces i przełączenie kontekstu.

- 3 czerwca: potok, forwarding, superskalarne, proces vs program,
- 4 czerwca: PCB, stany, przerwania, syscall, context switch,
- 5 czerwca: FCFS/SJF/SRTF/RR teoria,
- 6 czerwca: zadania z szeregowania.

### 7-10 czerwca: synchronizacja i pamięć podstawowa

Cel: umieć pułapki definicyjne.

- 7 czerwca: semafory, monitory, Hoare/Mesa,
- 8 czerwca: zakleszczenia, bankier,
- 9 czerwca: adresy, wiązanie, fragmentacja,
- 10 czerwca: buddy + zadania z fragmentacji.

### 11-14 czerwca: stronicowanie i pliki

Cel: wejść w najczęstsze zadania egzaminacyjne.

- 11 czerwca: strony, ramki, tablice stron, TLB,
- 12 czerwca: page fault, FIFO/LRU/OPT,
- 13 czerwca: thrashing, COW, mmap,
- 14 czerwca: i-węzły, deskryptory, VFS, ext2.

### 15-17 czerwca: stare egzaminy

Cel: przerobić pytania, nie czytać biernie.

- 15 czerwca: arkusz 1 + lista błędów,
- 16 czerwca: arkusz 2 + lista błędów,
- 17 czerwca: powtórka tylko błędów i zadań obliczeniowych.

### 18 czerwca: powtórka bojowa

Tylko:

- [99_PULAPKI_TAK_NIE.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/99_PULAPKI_TAK_NIE.md),
- miniściągi z notatek,
- zadania obliczeniowe,
- rzeczy, które dalej mylisz.

Nie czytaj 620 slajdów od nowa.

### 19 czerwca: egzamin

Rano tylko lekka powtórka:

- TLB/page fault,
- szeregowanie,
- i-węzły,
- semafory/monitory,
- bankier,
- potok i hazardy.

## Reguła przechodzenia dalej

Nie musisz mieć 100% pewności. Wystarczy:

- umiesz powiedzieć definicję własnymi słowami,
- umiesz wskazać typową pułapkę T/N,
- umiesz rozwiązać prosty przykład,
- nie mylisz głównych par pojęć.

Główne pary, których nie wolno mylić:

| Para | Różnica |
|---|---|
| architektura / organizacja | co widzi programista / jak to zrobiono |
| program / proces | plik/kod / wykonanie z kontekstem |
| strona / ramka | kawałek adresów procesu / kawałek RAM |
| adres logiczny / fizyczny | widziany przez proces / realny w RAM |
| CF / OF | bez znaku / ze znakiem |
| SJF / SRTF | niewywłaszczający / wywłaszczający |
| Hoare / Mesa | natychmiastowe wznowienie / ponowne wejście i sprawdzenie warunku |
| i-węzeł / deskryptor | metadane pliku / uchwyt procesu |
| polling / przerwanie | CPU pyta / urządzenie zgłasza |
| FIFO / LRU / OPT | najstarsza / najdawniej użyta / najpóźniej potrzebna |

