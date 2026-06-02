# AKSO - plan ogarniania do egzaminu

Zakładam egzamin: **19 czerwca 2026**.

Cel nie jest taki, żeby przeczytać wszystko idealnie. Cel jest taki, żeby umieć odpowiadać na pytania **Tak/Nie** i nie wpadać w pułapki typu "zawsze", "na pewno", "każde", "tylko".

## Zasada główna

Każdy temat robimy w schemacie:

1. **Definicja** - co to jest.
2. **Konsekwencja** - co z tego wynika.
3. **Pułapka T/N** - jak mogą to przekręcić na egzaminie.
4. **Mini test** - kilka zdań Tak/Nie z odpowiedzią.

Nie przepisujemy slajdów. Wyciągamy rzeczy, które można zamienić na pytanie egzaminacyjne.

## Jak używać 13 nagrań

Nagrania nie są główną nauką. Są ratunkiem, gdy temat nie wchodzi.

Najlepszy tryb:

1. Najpierw slajdy/notatka przez 20-30 minut.
2. Potem stare pytania z tego tematu.
3. Dopiero jeśli coś się sypie, odpalasz fragment nagrania.
4. Słuchasz najlepiej 1.25x albo 1.5x.
5. Po nagraniu dopisujesz 3-5 zdań do pułapek, nie robisz transkrypcji wykładu.

Najbardziej warto odpalać nagrania do:

- szeregowania procesów,
- stronicowania, TLB i page fault,
- fragmentacji i przydziału pamięci,
- i-węzłów, deskryptorów i bloków pośrednich,
- mmap, DMA i wejścia-wyjścia,
- potokowania, hazardów i forwarding.

## Harmonogram

### 1-3 czerwca: mapa tematów

Cel: wiedzieć, jakie działy istnieją i gdzie są największe miny.

Do zrobienia:

- [x] architektura vs organizacja,
- [x] architektura von Neumanna,
- [x] bity, liczby, x86 i asembler,
- [x] procesy i stany procesów,
- [x] szeregowanie procesów,
- [x] zakleszczenia, stan bezpieczny, bankier,
- [x] pamięć: adresy, wiązanie, fragmentacja,
- [x] stronicowanie, TLB, page fault,
- [x] system plików: i-węzły, katalogi, deskryptory,
- [x] wejście-wyjście: przerwania, DMA, mmap,
- [x] architektura procesora: RISC/CISC, potok, hazardy, SIMD/SMP/NUMA.

### 4-10 czerwca: główna teoria i notatki

Każdego dnia jeden większy blok. Po każdym bloku mają powstać notatki w stylu obecnych plików.

Proponowana kolejność:

| Data | Blok | Wynik |
|---|---|---|
| 4 czerwca | Procesy i stany | notatka + T/N |
| 5 czerwca | Szeregowanie: FCFS, SJF, SRTF, RR | notatka + przykłady obliczeń |
| 6 czerwca | Synchronizacja, semafory, monitory, zakleszczenia | notatka + pułapki |
| 7 czerwca | Pamięć: adresy, wiązanie, fragmentacja | notatka + zadania |
| 8 czerwca | Stronicowanie, TLB, page fault | notatka + zadania |
| 9 czerwca | System plików: i-węzły, deskryptory, bloki | notatka + zadania |
| 10 czerwca | I/O, DMA, mmap + powtórka architektury procesora | notatka + T/N |

### 11-15 czerwca: stare egzaminy

To jest najważniejszy etap.

Robisz arkusze tak:

1. Najpierw sam, bez notatek.
2. Zaznaczasz niepewne odpowiedzi.
3. Sprawdzasz.
4. Każdą pomyłkę wpisujesz do [99_PULAPKI_TAK_NIE.md](/Users/miloszkubis/Documents/AKSO-NOTATKI/99_PULAPKI_TAK_NIE.md).
5. Jeśli błąd wynika z braku teorii, dopisujemy krótki akapit do odpowiedniej notatki.

Nie chodzi o liczbę zrobionych arkuszy. Chodzi o listę powtarzalnych pułapek.

### 16-18 czerwca: powtórka bojowa

Nie czytasz wszystkiego od nowa.

Czytasz tylko:

- miniściągi z notatek,
- pytania T/N,
- plik z pułapkami,
- błędne zadania ze starych egzaminów,
- krótkie opisówki: pamięć wirtualna, stronicowanie, szeregowanie, i-węzły, potok.

### 18 czerwca wieczorem

Lekka powtórka:

- szeregowanie,
- TLB i stronicowanie,
- i-węzły i deskryptory,
- fragmentacja,
- RISC/CISC i potok,
- DMA, mmap, przerwania.

Potem spać. Zmęczona głowa robi głupie błędy w Tak/Nie.

## Kolejność notatek do zrobienia

1. `03_architektura_procesora_risc_cisc_potok.md`
2. `04_bity_x86_assembler.md`
3. `05_procesy_stany_syscall_przerwania.md`
4. `06_szeregowanie_procesow.md`
5. `07_zakleszczenia_bankier.md`
6. `08_io_dyski_dma.md`
7. `09_system_plikow_vfs_inode_deskryptory.md`
8. `10_pamiec_wiazanie_fragmentacja.md`
9. `11_stronicowanie_tlb_wirtualna.md`
10. `12_synchronizacja_semafory_monitory.md` - dopisane z pytań egzaminacyjnych, bo w tym PDF-ie nie wyszło jako osobny blok.

## Format każdej notatki

Każda notatka ma mieć:

- sekcję **Najkrócej**,
- sekcję z definicjami,
- sekcję **Pułapki egzaminacyjne**,
- tabelę porównawczą, jeśli temat tego wymaga,
- pytania **T/N z odpowiedziami**,
- miniściągę na koniec.

## Najważniejsze tematy z PDF-a

Z przykładowych egzaminów najmocniej wychodzą:

- procesy, PCB i stany,
- szeregowanie: FCFS, SJF, SRTF, RR,
- semafory, monitory, sekcja krytyczna,
- zakleszczenia, stan bezpieczny, algorytm bankiera,
- pamięć: fragmentacja, alokacja ciągła, buddy,
- stronicowanie, TLB, page fault, FIFO/LRU/OPT,
- thrashing, czyli migotanie stron,
- system plików: i-węzły, superblok, ext2, bloki pośrednie,
- deskryptory plików i tablica otwartych plików,
- wejście-wyjście: przerwania, polling, DMA,
- architektura: von Neumann, RISC/CISC, potok, hazardy.

## Codzienna mikroprocedura

Minimum na dzień:

1. 25 minut teorii.
2. 25 minut pytań lub zadań.
3. 10 minut dopisywania pułapek.

Wersja dobra:

1. 45 minut notatki.
2. 60 minut zadań/starych pytań.
3. 20 minut poprawiania błędów.

Wersja bardzo dobra:

1. Jeden temat.
2. Jedna notatka.
3. Jedna porcja pytań.
4. Dopisane pułapki.
