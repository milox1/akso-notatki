# AKSO - bank własnych pytań Tak/Nie

Jak używać:

1. Najpierw odpowiedz samodzielnie na pytania.
2. Zaznacz te, przy których strzelasz.
3. Dopiero potem sprawdź klucz na dole.
4. Błędne pytania dopisz do `99_PULAPKI_TAK_NIE.md`.

## Architektura i organizacja

1. Architektura komputera obejmuje cechy widzialne dla programisty.
2. Organizacja komputera opisuje sposób realizacji konkretnej architektury.
3. Taktowanie procesora jest typowym elementem architektury, bo wpływa na szybkość programu.
4. Tryby adresowania należą raczej do architektury niż organizacji.
5. Dwa procesory mogą mieć tę samą architekturę, ale inną organizację.

## Von Neumann, bity i x86

6. W architekturze von Neumanna program i dane są w tej samej pamięci.
7. Architektura harwardzka ma wspólną pamięć programu i danych.
8. ALU odpowiada za operacje arytmetyczne i logiczne.
9. Jednostka sterująca wykonuje dodawanie i odejmowanie.
10. Szyna adresowa przenosi dane odczytane z pamięci.
11. Kod maszynowy jest zależny od architektury procesora.
12. Asembler jest zawsze przenośny między procesorami.
13. W pamięci są bity, a typ danych wynika z interpretacji.
14. CF jest szczególnie ważny przy arytmetyce bez znaku.
15. OF jest szczególnie ważny przy arytmetyce ze znakiem w U2.

## RISC, CISC i potok

16. RISC typowo ma prostszy zbiór rozkazów niż CISC.
17. RISC typowo ma mało rejestrów ogólnego przeznaczenia.
18. CISC typowo ma więcej trybów adresowania niż RISC.
19. Potokowanie zwiększa przepustowość wykonywania instrukcji.
20. Potokowanie zawsze skraca czas wykonania pojedynczej instrukcji.
21. RAW oznacza, że instrukcja czyta wynik, którego wcześniejsza instrukcja jeszcze nie zapisała.
22. Forwarding może zmniejszać opóźnienia wynikające z hazardów danych.
23. Skoki warunkowe ułatwiają potokowanie.
24. SIMD oznacza wykonywanie jednej instrukcji na wielu danych.
25. NUMA oznacza niejednolity czas dostępu do pamięci.
25a. Rejestr IF/ID przechowuje informacje między etapem pobrania instrukcji i dekodowania.
25b. Etap EX może obliczać adres dla instrukcji `load` albo `store`.
25c. Etap WB zapisuje wynik do pamięci danych.
25d. MUX wybiera jedno z kilku wejść, np. dla ALU albo zapisu wyniku.
25e. PC przechowuje adres instrukcji do pobrania.
25f. Czas cyklu potoku musi być co najmniej tak długi jak najwolniejszy etap.
25g. W pięcioetapowym potoku o cyklu 4 ns pojedyncza instrukcja trwa 4 ns.
25h. W idealnym pięcioetapowym potoku o cyklu 4 ns wykonanie `n` instrukcji trwa `4(n+4)` ns.

## Procesy i system operacyjny

26. Proces to wykonywany program wraz z kontekstem.
27. Program i proces to dokładnie to samo.
28. PCB zawiera stan procesu.
29. PCB zawiera pełną kopię całej pamięci procesu.
30. Proces gotowy czeka na procesor.
31. Proces wstrzymany czeka na zdarzenie, np. zakończenie I/O.
32. Przełączenie kontekstu wymaga zapisania i odtworzenia części stanu procesu.
33. Funkcja systemowa wykonuje się w trybie uprzywilejowanym.
34. Przerwanie zegarowe może umożliwić wywłaszczenie procesu.
35. Zakończenie operacji I/O może przenieść proces ze wstrzymanego do gotowego.
35a. Segment text zawiera kod programu.
35b. Segment data zawiera zwykle zmienne globalne i statyczne.
35c. Stos służy głównie do przechowywania zmiennych globalnych.
35d. Heap służy do alokacji dynamicznej, np. przez `malloc`.
35e. PCB przechowuje całe segmenty text, data, heap i stack procesu.

## Szeregowanie

36. FCFS jest algorytmem niewywłaszczającym.
37. SJF wybiera proces o najkrótszej następnej fazie CPU.
38. SRTF jest wywłaszczającą wersją SJF.
39. RR wymaga kwantu czasu.
40. RR z bardzo dużym kwantem zbliża się zachowaniem do FCFS.
41. Bardzo mały kwant RR zmniejsza narzut przełączania kontekstu.
42. Postarzanie może przeciwdziałać zagłodzeniu.
43. Czas obrotu to czas od przybycia procesu do zakończenia.
44. Czas oczekiwania obejmuje czas wykonywania procesu na CPU.
45. SJF może prowadzić do zagłodzenia długich procesów.

## Synchronizacja

46. Sekcja krytyczna to fragment kodu korzystający ze współdzielonego zasobu.
47. Bezpieczeństwo sekcji krytycznej oznacza, że naraz jest w niej co najwyżej jeden proces.
48. Żywotność i bezpieczeństwo znaczą to samo.
49. Aktywne oczekiwanie zużywa CPU.
50. Spinlock jest przykładem aktywnego oczekiwania.
51. Semafor silny musi być binarny.
52. Semafor silny gwarantuje pewną uczciwość budzenia.
53. Monitor pozwala wielu procesom wykonywać procedury monitorowe naraz.
54. `wait` na zmiennej warunkowej zwalnia monitor.
55. Semantyka Hoare'a wymusza natychmiastowe wznowienie obudzonego procesu.

## Zakleszczenia

56. Zakleszczenie wymaga spełnienia wszystkich czterech warunków koniecznych.
57. Brak wywłaszczeń jest jednym z warunków koniecznych zakleszczenia.
58. Jeśli system jest w stanie zakleszczenia, to w grafie przydziału zasobów istnieje cykl.
59. Każdy cykl w grafie przydziału zasobów zawsze oznacza zakleszczenie.
60. Stan bezpieczny oznacza istnienie bezpiecznego ciągu procesów.
61. Stan bezpieczny oznacza, że żaden proces nie oczekuje na zasoby.
62. Algorytm bankiera wymaga deklaracji maksymalnych potrzeb procesów.
63. Algorytm bankiera jest metodą unikania zakleszczeń.
64. Wykrywanie zakleszczeń dopuszcza możliwość powstania zakleszczenia.
65. Zapobieganie zakleszczeniom polega na uniemożliwieniu spełnienia co najmniej jednego warunku koniecznego.

## Wejście-wyjście i DMA

66. Polling polega na odpytywaniu urządzenia przez procesor.
67. Polling zawsze wymaga kontrolera przerwań.
68. Przerwania pozwalają CPU nie sprawdzać ciągle stanu urządzenia.
69. DMA przesyła dane z dużym udziałem CPU kopiującego każdy bajt.
70. Koniec transmisji DMA jest często sygnalizowany przerwaniem.
71. Urządzenie blokowe przechowuje dane w blokach o ustalonym rozmiarze.
72. Urządzenie znakowe jest typowo strumieniowe.
73. Memory-mapped I/O oznacza, że rejestry urządzenia są widoczne pod adresami.
74. SSTF może prowadzić do zagłodzenia odległych żądań dyskowych.
75. SCAN jest nazywany algorytmem windy.

## System plików

76. Katalog mapuje nazwy na i-węzły.
77. I-węzeł zwykle przechowuje nazwę pliku.
78. I-węzeł zawiera informacje o rozmieszczeniu bloków pliku.
79. Pozycja bieżącego odczytu pliku jest przechowywana w i-węźle na dysku.
80. Deskryptor pliku jest lokalny dla procesu.
81. `dup` tworzy deskryptor wskazujący na ten sam obiekt otwartego pliku.
82. Po `dup` deskryptory mogą współdzielić pozycję w pliku.
83. Dowiązanie symboliczne przechowuje ścieżkę.
84. VFS jest warstwą abstrakcji nad systemami plików.
85. W Uniksie przestrzeń nazw plików tworzy jedno drzewo.

## Pamięć i fragmentacja

86. Adres fizyczny to rzeczywisty adres w pamięci operacyjnej.
87. Wiązanie adresów podczas wykonania ułatwia relokację.
88. Swapping całych procesów nie wymaga operacji I/O.
89. Fragmentacja wewnętrzna oznacza niewykorzystaną część przydzielonego obszaru.
90. Fragmentacja zewnętrzna oznacza rozproszone wolne dziury.
91. Przydział ciągły może cierpieć na fragmentację zewnętrzną.
92. Strefy statyczne typowo powodują fragmentację wewnętrzną.
93. Best fit wybiera największy wolny obszar.
94. Worst fit wybiera największy wolny obszar.
95. Defragmentacja jest najłatwiejsza, gdy procesów nie da się relokować.

## Stronicowanie, TLB i VM

96. W stronicowaniu adres logiczny składa się z numeru strony i offsetu.
97. Offset zmienia się podczas translacji adresu.
98. Tablica stron mapuje strony na ramki.
99. TLB buforuje wybrane translacje adresów.
100. Trafienie w TLB oznacza, że nie trzeba czytać danych z pamięci.
101. Bez TLB odwołanie przy jednopoziomowej tablicy stron może wymagać dwóch dostępów do RAM.
102. Page fault jest zgłaszany przez sprzęt.
103. Page fault jest obsługiwany przez system operacyjny.
104. FIFO może wykazywać anomalię Belady'ego.
105. LRU typowo nie wykazuje anomalii Belady'ego.
106. OPT jest algorytmem teoretycznym minimalizującym liczbę braków stron.
107. Lokalna strategia wymiany stron wybiera ofiarę tylko ze stron danego procesu.
108. Globalna strategia wymiany stron może podbierać ramki innym procesom.
109. Migotanie stron może obniżać wykorzystanie CPU.
110. Przy migotaniu stron zwykle pomaga zwiększenie stopnia wieloprogramowości.
111. Copy-on-write pozwala uniknąć natychmiastowego kopiowania stron po `fork`.
112. `mmap` może odwzorować plik w przestrzeń adresową procesu.
113. Buddy allocator zaokrągla żądania do obsługiwanych rozmiarów bloków.
114. W buddy allocator nie może wystąpić fragmentacja wewnętrzna.
115. Wielopoziomowe tablice stron mogą zmniejszać pamięć zużywaną na puste obszary przestrzeni adresowej.

## Klucz odpowiedzi

| Nr | Odp. | Nr | Odp. | Nr | Odp. | Nr | Odp. | Nr | Odp. |
|---:|:---:|---:|:---:|---:|:---:|---:|:---:|---:|:---:|
| 1 | T | 2 | T | 3 | N | 4 | T | 5 | T |
| 6 | T | 7 | N | 8 | T | 9 | N | 10 | N |
| 11 | T | 12 | N | 13 | T | 14 | T | 15 | T |
| 16 | T | 17 | N | 18 | T | 19 | T | 20 | N |
| 21 | T | 22 | T | 23 | N | 24 | T | 25 | T |
| 25a | T | 25b | T | 25c | N | 25d | T | 25e | T |
| 25f | T | 25g | N | 25h | T |  |  |  |  |
| 26 | T | 27 | N | 28 | T | 29 | N | 30 | T |
| 31 | T | 32 | T | 33 | T | 34 | T | 35 | T |
| 35a | T | 35b | T | 35c | N | 35d | T | 35e | N |
| 36 | T | 37 | T | 38 | T | 39 | T | 40 | T |
| 41 | N | 42 | T | 43 | T | 44 | N | 45 | T |
| 46 | T | 47 | T | 48 | N | 49 | T | 50 | T |
| 51 | N | 52 | T | 53 | N | 54 | T | 55 | T |
| 56 | T | 57 | T | 58 | T | 59 | N | 60 | T |
| 61 | N | 62 | T | 63 | T | 64 | T | 65 | T |
| 66 | T | 67 | N | 68 | T | 69 | N | 70 | T |
| 71 | T | 72 | T | 73 | T | 74 | T | 75 | T |
| 76 | T | 77 | N | 78 | T | 79 | N | 80 | T |
| 81 | T | 82 | T | 83 | T | 84 | T | 85 | T |
| 86 | T | 87 | T | 88 | N | 89 | T | 90 | T |
| 91 | T | 92 | T | 93 | N | 94 | T | 95 | N |
| 96 | T | 97 | N | 98 | T | 99 | T | 100 | N |
| 101 | T | 102 | T | 103 | T | 104 | T | 105 | T |
| 106 | T | 107 | T | 108 | T | 109 | T | 110 | N |
| 111 | T | 112 | T | 113 | T | 114 | N | 115 | T |
