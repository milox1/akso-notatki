# AKSO - skąd brać zadania obliczeniowe

## Najkrócej

Zadania obliczeniowe bierz z trzech źródeł:

1. **stare egzaminy** - najlepsze źródło, bo pokazują styl pytań,
2. **przykłady ze slajdów** - dobre do nauki procedury krok po kroku,
3. **własne warianty** - zmieniasz liczby i liczysz od nowa.

Nie szukaj najpierw losowych zadań w internecie. Na AKSO najważniejszy jest lokalny styl prowadzącego.

## Typy zadań, które trzeba umieć

| Temat | Co liczyć |
|---|---|
| szeregowanie | czasy obrotu, oczekiwania, odpowiedzi dla FCFS/SJF/SRTF/RR |
| stronicowanie | liczba page faultów dla FIFO/LRU/OPT |
| TLB | efektywny czas dostępu do pamięci |
| adresy i strony | podział adresu na numer strony i offset |
| fragmentacja | wewnętrzna/zewnętrzna, first fit/best fit/worst fit |
| buddy allocator | zaokrąglanie żądań, wolne bloki, fragmentacja wewnętrzna |
| zakleszczenia | bankier, stan bezpieczny, algorytm wykrywania |
| i-węzły | bloki bezpośrednie i pośrednie, maksymalny rozmiar pliku |
| ext2 | liczba grup, bitmapy, tablice i-węzłów, superbloki |
| dyski | FCFS/SSTF/SCAN/C-SCAN, jeśli pojawi się w zadaniach |

## Konkretne miejsca w starym egzaminie

W `so_egz.pdf` zadania liczbowe/obliczeniowe są szczególnie tutaj:

- szeregowanie: pytanie 15 w drugiej części,
- fragmentacja pamięci: pytania 14 i 18,
- stronicowanie/page faulty: pytania 16 i 20,
- buddy allocator: pytania 17 i 21,
- TLB / efektywny czas dostępu: pytanie 19,
- zakleszczenia / wykrywanie: pytania 12 i 13,
- i-węzły / bloki pośrednie: pytania 18 i 29,
- ext2: pytanie 30.

To są najważniejsze wzorce. Jak je umiesz, to większość podobnych pytań jest tylko zmianą liczb.

## Konkretne miejsca w slajdach `akso (3).pdf`

Najlepsze przykłady ze slajdów:

- FCFS/SJF/SRTF/RR: okolice slajdów **233-294**,
- real-time scheduling: okolice **318-373**,
- bankier i wykrywanie zakleszczeń: okolice **397-419**,
- dyski i strategie dostępu: okolice **433-439**,
- wiązanie i fragmentacja pamięci: okolice **502-546**,
- stronicowanie i TLB: okolice **547-565**,
- FIFO/LRU/OPT i Belady: okolice **576-588**,
- thrashing / zbiór roboczy: okolice **592-596**,
- buddy allocator w Linuxie: okolice **617-620**.

## Jak robić własne warianty

### Szeregowanie

Weź tabelę:

| Proces | Przybycie | Faza CPU |
|---|---:|---:|
| P1 | 0 | 5 |
| P2 | 1 | 3 |
| P3 | 5 | 2 |

Potem zmień jedną rzecz:

- kolejność przybycia,
- długości faz,
- kwant RR.

Policz dla:

- FCFS,
- SJF,
- SRTF,
- RR.

### Page faulty

Weź ciąg:

```text
1, 2, 1, 3, 4, 1, 3, 2, 3, 1, 4, 2
```

Zmieniaj:

- liczbę ramek: 3 albo 4,
- algorytm: FIFO, LRU, OPT,
- kolejność kilku odwołań.

Cel: umieć liczyć bez zgadywania.

### TLB

Schemat:

```text
hit ratio = 90%
czas TLB = 10 ns
czas RAM = 100 ns
page fault rate = 0 albo np. 1/1 000 000
obsługa page fault = 50 ms
```

Najpierw licz bez page faultów, potem z page faultami.

Pułapka: ms trzeba zamienić na ns, jeśli wszystko liczysz w ns.

### Fragmentacja

Schemat:

```text
pamięć: 600 KB
alokacje: 200 KB, 100 KB, 100 KB
zwolnij drugi blok
zaalokuj 30 KB
```

Potem pytania:

- jaka największa dziura?
- ile wynosi fragmentacja wewnętrzna?
- czy kolejne żądanie się powiedzie?
- co zmienia first fit/best fit/worst fit?

### Buddy

Schemat:

```text
pamięć: 1 MB
minimalny blok: 16 KB
żądania: 50 KB, 200 KB, 300 KB, 200 KB
```

Zaokrąglasz każde żądanie do najbliższego obsługiwanego bloku:

- 50 KB -> 64 KB,
- 200 KB -> 256 KB,
- 300 KB -> 512 KB,
- 200 KB -> 256 KB.

Potem sprawdzasz, czy da się podzielić pamięć i ile pamięci marnuje się wewnętrznie.

### I-węzły

Schemat:

```text
10 wskaźników bezpośrednich
1 pojedynczo pośredni
1 podwójnie pośredni
blok: 1 KB
wpis: 4 B
```

Licz:

- ile adresów mieści blok pośredni,
- ile bloków danych obsługują wskaźniki bezpośrednie,
- ile pojedynczo pośredni,
- ile podwójnie pośredni,
- maksymalny rozmiar pliku.

## Minimalny bank do przerobienia

Przed egzaminem zrób minimum:

- 3 zadania z szeregowania,
- 3 zadania z page faultów,
- 2 zadania z TLB,
- 3 zadania z fragmentacji/buddy,
- 2 zadania z bankiera/wykrywania zakleszczeń,
- 2 zadania z i-węzłów,
- 1 zadanie z ext2.

To daje lepszy efekt niż kolejne bierne czytanie slajdów.

## Co dopisać później

Docelowo warto zrobić osobny plik:

```text
96_ZADANIA_OBLICZENIOWE_Z_ROZWIAZANIAMI.md
```

W nim powinny być pełne rozwiązania krok po kroku:

- tabelki Gantta,
- tabele ramek dla FIFO/LRU/OPT,
- obliczenia TLB,
- rysowanie dziur w pamięci,
- bankier krok po kroku,
- bloki i-węzłów.

