# AKSO - stronicowanie, TLB, pamięć wirtualna i page fault

## Najkrócej

**Stronicowanie** dzieli pamięć logiczną na strony, a pamięć fizyczną na ramki tego samego rozmiaru.

Adres logiczny:

> numer strony + offset

Translacja zamienia numer strony na numer ramki. Offset się nie zmienia.

## Strony i ramki

| Pojęcie | Sens |
|---|---|
| strona | blok przestrzeni logicznej procesu |
| ramka | blok pamięci fizycznej |
| tablica stron | mapuje strony na ramki |
| offset | przesunięcie w obrębie strony/ramki |

Rozmiar strony jest zwykle potęgą dwójki, więc offset to dolne bity adresu.

## Translacja adresu

Dla adresu logicznego:

1. procesor dzieli adres na numer strony i offset,
2. numer strony służy do znalezienia wpisu w tablicy stron,
3. z wpisu pobierany jest numer ramki,
4. adres fizyczny to numer ramki + ten sam offset.

Translacja jest wykonywana sprzętowo, ale tablicami stron zarządza system operacyjny.

## Tablica stron

Wpis tablicy stron może zawierać:

- numer ramki,
- bit poprawności,
- bity ochrony,
- bit modyfikacji,
- bit odwołania,
- informacje pomocnicze dla wymiany stron.

Jeśli tablica stron jest w pamięci, zwykły dostęp do danych bez TLB może wymagać dwóch dostępów do RAM:

1. odczyt wpisu tablicy stron,
2. odczyt/zapis właściwej komórki.

## TLB

**TLB** to szybki bufor translacji adresów.

Przechowuje ostatnio używane mapowania:

> numer strony -> numer ramki

Przy trafieniu w TLB nie trzeba czytać tablicy stron z RAM.

Przy chybieniu trzeba sprawdzić tablicę stron i ewentualnie dodać wpis do TLB.

## Efektywny czas dostępu

TLB poprawia średni czas dostępu, jeśli współczynnik trafień jest wysoki.

Pułapka:

> Trafienie w TLB nie oznacza, że nie ma dostępu do pamięci po dane. Oznacza, że nie trzeba dodatkowo czytać tablicy stron z RAM.

## Wielopoziomowe tablice stron

Stosuje się je, żeby nie trzymać jednej ogromnej tablicy stron dla całej przestrzeni adresowej.

Cena:

- mniej pamięci na puste obszary,
- potencjalnie więcej kroków translacji,
- większy efektywny czas dostępu bez TLB.

Zwiększenie liczby poziomów może zwiększyć maksymalny rozmiar przestrzeni adresowej, jeśli każdy poziom mieści się na stronie i dokładamy kolejne bity indeksu.

## Pamięć wirtualna

W pamięci wirtualnej nie wszystkie strony procesu muszą być w RAM.

Część może być:

- w pamięci operacyjnej,
- na dysku,
- jeszcze niezaalokowana fizycznie.

To pozwala uruchamiać procesy większe niż dostępna pamięć RAM, ale page faulty są kosztowne.

## Page fault

**Błąd braku strony** powstaje, gdy proces odwoła się do strony, której nie ma aktywnie w pamięci fizycznej.

Zgłasza go sprzęt, a obsługuje system operacyjny.

Typowa obsługa:

1. sprzęt wykrywa brak poprawnego wpisu,
2. następuje pułapka do jądra,
3. system sprawdza, czy adres jest legalny,
4. znajduje wolną ramkę albo wybiera ofiarę,
5. sprowadza stronę z dysku lub alokuje ją,
6. aktualizuje tablicę stron,
7. wznawia instrukcję.

## Algorytmy zastępowania stron

| Algorytm | Idea |
|---|---|
| FIFO | usuń stronę najdłużej przebywającą w pamięci |
| OPT | usuń stronę, która będzie najpóźniej potrzebna |
| LRU | usuń stronę najdawniej używaną |
| druga szansa | FIFO z bitem odwołania |
| LFU | usuń stronę najrzadziej używaną |

OPT jest teoretyczny i służy jako punkt odniesienia.

## Anomalia Belady'ego

W niektórych algorytmach, zwłaszcza FIFO, zwiększenie liczby ramek może zwiększyć liczbę błędów braku strony.

To nazywa się **anomalią Belady'ego**.

LRU i OPT nie mają tej anomalii, bo są algorytmami stosowymi.

## Lokalna i globalna wymiana stron

**Lokalna**: proces może wymieniać tylko swoje strony.

**Globalna**: ofiarą może być strona dowolnego procesu.

Globalna strategia może poprawiać wykorzystanie pamięci, ale procesy mogą sobie wzajemnie podbierać ramki.

## Migotanie stron

**Migotanie stron** występuje, gdy system spędza bardzo dużo czasu na obsłudze page faultów zamiast na wykonywaniu programów.

Lokalnie:

- proces ma za mało ramek,
- jego aktywny zbiór stron nie mieści się w pamięci.

Globalnie:

- procesy podbierają sobie ramki,
- page faulty rozlewają się na cały system,
- wykorzystanie CPU spada.

Pomaga:

- zmniejszenie stopnia wieloprogramowości,
- wstrzymanie części procesów,
- zwiększenie liczby ramek/RAM,
- model zbioru roboczego.

Nie pomaga samo zwiększenie dysku.

## Copy-on-write

Copy-on-write pozwala współdzielić strony pamięci do momentu zapisu.

Typowy przykład: `fork`.

Rodzic i dziecko początkowo współdzielą strony tylko do odczytu. Gdy któryś proces próbuje pisać, system kopiuje stronę.

## mmap

`mmap` odwzorowuje plik lub obszar pamięci w przestrzeń adresową procesu.

Może służyć do:

- wygodnego dostępu do pliku jak do pamięci,
- współdzielenia pamięci między procesami,
- ładowania bibliotek,
- mapowania anonimowej pamięci.

## Buddy allocator

Algorytm bliźniaków przydziela bloki o rozmiarach będących potęgami dwójki lub wielokrotnościami minimalnego bloku.

Gdy potrzeba bloku, większy blok może zostać podzielony na dwóch bliźniaków.

Gdy dwa wolne bliźniaki są dostępne, można je scalić.

Problemem jest fragmentacja wewnętrzna, bo żądanie jest zaokrąglane do większego bloku.

## Pułapki egzaminacyjne

- Offset nie zmienia się przy translacji stronicowania.
- Page fault zgłasza sprzęt, ale obsługuje system operacyjny.
- Trafienie TLB nie oznacza braku odczytu danych z pamięci.
- Bez TLB jednopoziomowa tablica stron zwykle daje dwa dostępy do RAM dla jednego odwołania.
- FIFO może mieć anomalię Belady'ego.
- OPT i LRU nie mają anomalii Belady'ego.
- Migotanie stron obniża wykorzystanie CPU.
- Przy thrashingu pomaga zmniejszenie wieloprogramowości, nie dokładanie samego dysku.
- Copy-on-write dotyczy pamięci, nie deskryptorów plików.

## Pytania T/N z odpowiedziami

1. W stronicowaniu adres logiczny składa się z numeru strony i offsetu.  
   **T** - to podstawowy podział adresu.

2. Offset zmienia się podczas translacji adresu.  
   **N** - zmienia się numer strony na numer ramki, offset zostaje.

3. Translacja adresu jest wykonywana sprzętowo, a tablicami stron zarządza SO.  
   **T** - sprzęt tłumaczy, system ustawia struktury.

4. TLB przechowuje wybrane mapowania stron na ramki.  
   **T** - to bufor translacji.

5. Trafienie w TLB eliminuje potrzebę odczytu właściwych danych z pamięci.  
   **N** - eliminuje dodatkowy odczyt wpisu tablicy stron.

6. Page fault jest zgłaszany przez sprzęt.  
   **T** - sprzęt wykrywa brak poprawnej strony.

7. Page fault jest obsługiwany przez system operacyjny.  
   **T** - jądro sprowadza stronę lub zgłasza błąd.

8. FIFO może wykazywać anomalię Belady'ego.  
   **T** - więcej ramek może dać więcej faultów.

9. LRU ma anomalię Belady'ego.  
   **N** - LRU jest algorytmem stosowym.

10. Migotanie stron może obniżyć wykorzystanie procesora.  
    **T** - CPU czeka na obsługę braków stron.

11. Zwiększenie stopnia wieloprogramowości zawsze pomaga przy migotaniu stron.  
    **N** - zwykle pogarsza sytuację.

12. Tymczasowe wstrzymanie części procesów może pomóc przy thrashingu.  
    **T** - zwalnia ramki dla pozostałych procesów.

13. Copy-on-write pozwala uniknąć natychmiastowego kopiowania pamięci po `fork`.  
    **T** - strony są kopiowane dopiero przy zapisie.

14. W buddy allocator każde żądanie dostaje dokładnie tyle bajtów, ile poprosiło.  
    **N** - rozmiar jest zaokrąglany do obsługiwanego bloku.

