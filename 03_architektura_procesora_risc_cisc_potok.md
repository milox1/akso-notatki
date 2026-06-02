# AKSO - RISC, CISC, potok i równoległość procesora

## Najkrócej

Ten dział opisuje, jak procesor wykonuje instrukcje i jak próbuje robić to szybciej.

Najważniejsze hasła:

- **CISC** - złożone instrukcje, zwykle więcej trybów adresowania.
- **RISC** - proste instrukcje, zwykle dużo rejestrów ogólnego przeznaczenia.
- **Potokowanie** - różne etapy wykonania kilku instrukcji zachodzą na siebie.
- **Hazardy** - sytuacje, które przeszkadzają potokowi.
- **Forwarding** - przekazanie wyniku dalej bez czekania na pełny zapis do rejestru.
- **Superskalarność, SIMD, VLIW, EPIC, wielordzeniowość** - różne sposoby wykonywania wielu rzeczy równolegle.

## RISC vs CISC

| Cecha | RISC | CISC |
|---|---|---|
| Rozkazy | Proste | Bardziej złożone |
| Liczba rozkazów | Mniejsza | Większa |
| Długość rozkazów | Często stała | Często zmienna |
| Tryby adresowania | Zwykle mało | Zwykle dużo |
| Rejestry | Dużo ogólnego przeznaczenia | Częściej specjalizowane |
| Dostęp do pamięci | Zwykle przez load/store | Częściej bezpośrednio w wielu instrukcjach |
| Cel | Ułatwić szybkie wykonanie prostych instrukcji | Dać bogaty zestaw instrukcji |

Ważna uwaga egzaminacyjna: to są **typowe cechy**, nie prawa fizyki. Współczesne procesory mogą mieszać idee. Na przykład x86 jest architekturą CISC, ale wewnętrznie może tłumaczyć złożone instrukcje na prostsze mikrooperacje.

## RISC

Typowe cechy RISC:

- prosty zbiór rozkazów,
- mała liczba trybów adresowania,
- dużo rejestrów ogólnego przeznaczenia,
- proste instrukcje łatwiejsze do potokowania,
- często architektura typu load/store.

**Load/store** oznacza, że operacje arytmetyczne i logiczne zwykle działają na rejestrach, a dostęp do pamięci robi się osobnymi instrukcjami odczytu i zapisu.

Przykład myślenia:

- najpierw wczytaj dane z pamięci do rejestrów,
- potem wykonaj operację w ALU,
- potem ewentualnie zapisz wynik do pamięci.

## CISC

Typowe cechy CISC:

- większy i bardziej złożony zbiór rozkazów,
- więcej trybów adresowania,
- instrukcje mogą robić więcej w jednym rozkazie,
- rozkazy mogą mieć różną długość,
- dekodowanie instrukcji może być bardziej złożone.

CISC historycznie miał ułatwiać pisanie programów i ograniczać liczbę instrukcji potrzebnych do wykonania zadania.

## Potokowanie

**Potokowanie** polega na podziale wykonania instrukcji na etapy. Gdy jedna instrukcja jest dekodowana, kolejna może być już pobierana, a wcześniejsza może być wykonywana.

Klasyczne etapy:

| Skrót | Nazwa | Sens |
|---|---|---|
| F | Fetch | Pobranie instrukcji |
| D | Decode | Dekodowanie instrukcji |
| E | Execute | Wykonanie operacji |
| M | Memory | Dostęp do pamięci |
| W | Write back | Zapis wyniku |

Potok nie oznacza, że jedna instrukcja wykonuje się magicznie szybciej. Oznacza, że po zapełnieniu potoku procesor może kończyć instrukcje częściej.

Egzaminacyjna intuicja:

> Potokowanie zwiększa przepustowość, ale pojedyncza instrukcja nadal musi przejść przez swoje etapy.

## Budowa potoku

Na slajdzie z budową potoku widać klasyczny potok pięcioetapowy:

```text
IF -> ID -> EX -> MEM -> WB
```

Między etapami są **rejestry potokowe**, które zapamiętują wyniki jednego etapu dla następnego cyklu.

| Rejestr potokowy | Co rozdziela |
|---|---|
| IF/ID | pobranie instrukcji od dekodowania |
| ID/EX | dekodowanie od wykonania |
| EX/MEM | wykonanie od dostępu do pamięci |
| MEM/WB | dostęp do pamięci od zapisu wyniku |

Bez rejestrów potokowych etapy nie mogłyby pracować równolegle na różnych instrukcjach, bo dane z etapów mieszałyby się ze sobą.

### IF - Instruction Fetch

Etap **IF** pobiera instrukcję z pamięci.

Najważniejsze elementy:

- **PC** - Program Counter, adres aktualnie pobieranej instrukcji,
- pamięć instrukcji,
- układ liczący następny adres, np. `PC + 4`,
- rejestr **IF/ID**, który przechowuje pobraną instrukcję i następny PC.

Egzaminowo:

> IF odpowiada za pobranie instrukcji, nie za jej wykonanie.

### ID - Instruction Decode / Register Fetch

Etap **ID** dekoduje instrukcję i odczytuje rejestry.

Najważniejsze elementy:

- **Register File** - plik rejestrów procesora,
- odczyt rejestrów źródłowych, np. `RS1`, `RS2`,
- dekodowanie typu instrukcji,
- **Sign Extend** - rozszerzenie wartości natychmiastowej ze znakiem,
- rejestr **ID/EX**.

Egzaminowo:

> ID rozpoznaje instrukcję i pobiera argumenty z rejestrów.

### EX - Execute / Address Calculation

Etap **EX** wykonuje operację albo liczy adres.

Najważniejsze elementy:

- **ALU** - wykonuje obliczenia,
- **MUX** - wybiera, skąd ma przyjść argument do ALU,
- obliczanie adresu dla `load`/`store`,
- sprawdzanie warunku skoku, np. czy wynik jest zero,
- wyliczenie adresu skoku,
- rejestr **EX/MEM**.

Egzaminowo:

> EX to nie tylko "arytmetyka"; tu może być też obliczanie adresu pamięci i decyzja o skoku.

### MEM - Memory Access

Etap **MEM** wykonuje dostęp do pamięci danych.

Dotyczy głównie instrukcji:

- `load` - odczyt z pamięci,
- `store` - zapis do pamięci.

Dla instrukcji arytmetycznych etap MEM może tylko przepuścić wynik dalej.

Rejestr **MEM/WB** zapamiętuje wynik dla ostatniego etapu.

Egzaminowo:

> Nie każda instrukcja realnie korzysta z pamięci w etapie MEM.

### WB - Write Back

Etap **WB** zapisuje wynik do rejestru docelowego.

Wynik może pochodzić:

- z ALU,
- z pamięci po instrukcji `load`.

MUX przy WB wybiera, która wartość ma zostać zapisana.

Egzaminowo:

> WB zapisuje wynik do rejestru, a nie do pamięci danych.

## Elementy ze schematu potoku

| Element | Co robi |
|---|---|
| PC | przechowuje adres następnej/pobieranej instrukcji |
| Memory w IF | pamięć instrukcji |
| Memory w MEM | pamięć danych |
| Register File | zbiór rejestrów procesora |
| Sign Extend | rozszerza immediate ze znakiem |
| ALU | wykonuje operację albo liczy adres |
| MUX | wybiera jedno z kilku wejść |
| IF/ID, ID/EX, EX/MEM, MEM/WB | rejestry potokowe między etapami |
| Next PC | adres kolejnej instrukcji, normalnie następna sekwencyjna albo cel skoku |

W prostym rysunku często są dwie pamięci: jedna dla instrukcji i jedna dla danych. To przypomina architekturę harwardzką, ale w praktycznych procesorach może oznaczać też osobne cache instrukcji i danych przy wspólnej pamięci głównej.

## Skoki w budowie potoku

Na schemacie skok wpływa na wybór **Next PC**.

Jeśli skok jest wykonany:

- PC powinien dostać adres celu skoku,
- instrukcje pobrane po drodze mogą być złe,
- potok może wymagać opróżnienia albo korekty.

Dlatego branch/jump jest hazardem sterowania.

## Forwarding na schemacie

Linie wracające z późniejszych etapów do wcześniejszych oznaczają możliwość przekazania wyniku bez czekania na pełny zapis w WB.

Przykład:

```asm
add r1, r2, r3
sub r4, r1, r5
```

`sub` potrzebuje `r1`, ale `add` może jeszcze nie zapisał wyniku do rejestru. Forwarding może przekazać wynik z EX/MEM albo MEM/WB bezpośrednio do wejścia ALU.

## Czasy wykonania w potoku

W potoku wszystkie etapy są taktowane wspólnym czasem cyklu. Ten czas musi być co najmniej tak długi, jak najwolniejszy etap.

Przykładowe czasy ze slajdu:

| Klasa instrukcji | F | D | E | M | W | Razem bez potoku |
|---|---:|---:|---:|---:|---:|---:|
| odczyt z pamięci | 4 ns | 2 ns | 3 ns | 4 ns | 2 ns | 15 ns |
| zapis do pamięci | 4 ns | 3 ns | 3 ns | 4 ns | - | 14 ns |
| arytm.-logiczne | 4 ns | 3 ns | 4 ns | - | 2 ns | 13 ns |
| rozgałęzienia | 4 ns | 3 ns | 3 ns | - | - | 10 ns |

Najwolniejszy etap trwa **4 ns**, więc w potoku każdy etap zajmuje pełne **4 ns**, nawet jeśli realnie mógłby skończyć szybciej.

Dla potoku pięcioetapowego jedna instrukcja potokowo przechodzi:

```text
5 etapów * 4 ns = 20 ns
```

To może być dłużej niż wykonanie pojedynczej instrukcji bez potoku.

Pułapka:

> Potokowanie nie musi skracać czasu pojedynczej instrukcji. Zwiększa głównie przepustowość, czyli liczbę instrukcji kończonych w jednostce czasu.

Dla `n` instrukcji w idealnym potoku pięcioetapowym:

```text
czas = 4(n + 4) ns
```

Dlaczego `n + 4`?

- pierwsza instrukcja potrzebuje 5 cykli, żeby przejść przez cały potok,
- potem po napełnieniu potoku kończy się jedna instrukcja na cykl,
- dla `n` instrukcji i 5 etapów liczba cykli to `n + 5 - 1 = n + 4`.

Przykład:

```text
n = 1  -> 4(1 + 4) = 20 ns
n = 10 -> 4(10 + 4) = 56 ns
```

Bez potoku 10 instrukcji trzeba wykonywać po kolei, więc czas byłby sumą czasów każdej instrukcji. Potok daje zysk dopiero przy serii instrukcji.

## Hazardy

**Hazard** to sytuacja, która przeszkadza w płynnym wykonaniu instrukcji w potoku.

### Hazard danych

Występuje, gdy instrukcje zależą od tych samych danych.

| Typ | Znaczenie | Intuicja |
|---|---|---|
| RAW | Read After Write | instrukcja chce czytać wynik, którego poprzednia jeszcze nie zapisała |
| WAR | Write After Read | instrukcja chce zapisać zanim wcześniejsza zdąży odczytać |
| WAW | Write After Write | dwie instrukcje zapisują w to samo miejsce w złej kolejności |

Najważniejszy klasyczny hazard to **RAW**, bo następna instrukcja potrzebuje wyniku poprzedniej.

### Define-use i forwarding

Przykład zwykłej zależności RAW:

```text
r1 := r2 + r3
r4 := r1 - r5
```

Drugi rozkaz potrzebuje `r1`, które produkuje pierwszy rozkaz.

Dla instrukcji arytmetycznej wynik `r1` powstaje na końcu etapu **E** pierwszej instrukcji. Druga instrukcja potrzebuje tego wyniku w swoim etapie **E**.

Bez pomocy procesor musiałby czekać, aż pierwszy rozkaz zapisze wynik do rejestru w etapie **W**.

**Data forwarding** / **data bypassing** pozwala przekazać wynik bezpośrednio z późniejszego etapu pierwszej instrukcji do etapu **E** drugiej instrukcji, zanim wynik zostanie zapisany do pliku rejestrów.

Egzaminowo:

> Forwarding pozwala użyć wyniku przed jego pełnym zapisaniem do rejestru.

Odpowiedź: **T**.

### Load-use

Zależność **load-use** jest trudniejsza:

```text
r1 := [r2 + 4]
r4 := r1 - r5
```

Pierwszy rozkaz ładuje dane z pamięci.

Dla `load`:

- w etapie **E** liczony jest adres, np. `r2 + 4`,
- w etapie **M** dopiero następuje odczyt danych z pamięci,
- więc wartość `r1` jest gotowa później niż przy zwykłej operacji ALU.

Drugi rozkaz potrzebuje `r1` w swoim etapie **E**. Jeśli idzie zaraz po `load`, może chcieć użyć wyniku zanim dane z pamięci będą gotowe.

Dlatego forwarding **może być użyty**, ale często dopiero po wstawieniu jednego opóźnienia:

```text
load: F D E M W
sub:    F D D E M W
```

Dodatkowe `D` oznacza stall/opóźnienie. Po nim wynik z etapu **M** instrukcji `load` można przekazać do etapu **E** instrukcji `sub`.

Najważniejsze:

```text
wynik ALU  -> gotowy po E
wynik load -> gotowy po M
```

Pułapka:

> Data forwarding zawsze usuwa zależność load-use bez opóźnienia.

Odpowiedź: **N**.

To opóźnienie nazywa się **latency**. Kompilator może czasem zmniejszyć jego koszt, przestawiając niezależną instrukcję między `load` a użycie wyniku.

### Hazard sterowania

Występuje przy skokach, zwłaszcza warunkowych.

Problem:

- procesor nie wie od razu, czy skok zostanie wykonany,
- nie wie więc, którą instrukcję powinien pobierać dalej,
- błędnie pobrane instrukcje trzeba odrzucić.

Dlatego skoki utrudniają potokowanie.

### Hazard strukturalny

Występuje, gdy dwie instrukcje chcą użyć tego samego zasobu sprzętowego w tym samym czasie.

Przykład: jedna jednostka pamięci, a dwie operacje chcą z niej skorzystać jednocześnie.

## Forwarding

**Forwarding**, czyli przekazywanie danych, pozwala ograniczyć czekanie w potoku.

Jeżeli wynik został już obliczony, ale jeszcze nie zapisany do rejestru, można przekazać go bezpośrednio do kolejnej instrukcji, która go potrzebuje.

To pomaga szczególnie przy zależnościach typu RAW.

Forwarding nie rozwiązuje wszystkich problemów. Czasem potok i tak musi zostać zatrzymany na chwilę, czyli pojawia się stall.

## Skoki i przewidywanie

Skoki są problemem, bo zmieniają kolejność wykonywania instrukcji.

Procesor może stosować:

- przewidywanie skoków,
- wykonywanie spekulacyjne,
- opróżnianie potoku po błędnym przewidywaniu.

Egzaminowo wystarczy pamiętać:

> Skok warunkowy może popsuć potok, bo procesor nie wie od razu, skąd pobierać kolejne instrukcje.

## Superskalarność

Procesor superskalarny może wykonywać kilka instrukcji równolegle w jednym cyklu, jeśli ma odpowiednie jednostki wykonawcze i instrukcje nie są od siebie zależne.

Nie każda para instrukcji może być wykonana równolegle. Zależności danych i konflikty zasobów nadal przeszkadzają.

## SIMD

**SIMD** oznacza Single Instruction, Multiple Data.

Jedna instrukcja wykonuje tę samą operację na wielu danych naraz.

Przykład: dodaj do siebie wiele par liczb w wektorze.

SIMD jest dobre do:

- grafiki,
- multimediów,
- obliczeń wektorowych,
- prostych operacji na dużych tablicach danych.

## VLIW i EPIC

**VLIW** oznacza Very Long Instruction Word.

Idea: jedna bardzo długa instrukcja zawiera kilka operacji, które mogą być wykonane równolegle.

W VLIW dużo odpowiedzialności za znalezienie równoległości ma kompilator.

**EPIC** to rozwinięcie podobnej idei: kompilator jawnie wskazuje, które operacje można wykonywać równolegle.

Egzaminowo:

- superskalarność częściej szuka równoległości sprzętowo,
- VLIW/EPIC mocniej przesuwają odpowiedzialność na kompilator.

## Wielordzeniowość

Procesor wielordzeniowy ma więcej niż jeden rdzeń wykonawczy.

Pozwala to uruchamiać równolegle wiele wątków lub procesów, ale program musi mieć pracę, którą da się podzielić.

Więcej rdzeni nie oznacza automatycznie proporcjonalnie szybszego działania każdego programu.

## SMP i NUMA

| Cecha | SMP | NUMA |
|---|---|---|
| Rozwinięcie | Symmetric Multiprocessing | Non-Uniform Memory Access |
| Pamięć | Wspólna, widziana podobnie przez procesory | Wspólna logicznie, ale dostęp ma różne czasy |
| Dostęp do pamięci | Zasadniczo jednolity | Lokalna pamięć szybsza, zdalna wolniejsza |
| Pułapka | nie oznacza osobnej pamięci dla każdego procesora | nie oznacza braku wspólnej przestrzeni adresowej |

W NUMA ważne jest, gdzie fizycznie znajduje się pamięć względem rdzenia/procesora, który z niej korzysta.

## Pułapki egzaminacyjne

- **RISC nie znaczy "mało rejestrów"**. Typowo jest odwrotnie: dużo rejestrów ogólnego przeznaczenia.
- **CISC/RISC to typowe cechy, nie absolutne zasady dla każdego procesora.**
- **Potokowanie nie skraca koniecznie czasu wykonania jednej instrukcji**, tylko zwiększa przepustowość.
- **RAW/WAR/WAW dotyczą kolejności odczytu i zapisu danych.**
- **Forwarding pomaga, ale nie usuwa wszystkich hazardów.**
- **Skoki warunkowe utrudniają potokowanie.**
- **SIMD to jedna instrukcja na wielu danych, a nie wiele różnych instrukcji naraz.**
- **SMP i NUMA mogą mieć wspólną przestrzeń adresową, ale NUMA ma niejednolity czas dostępu do pamięci.**

## Pytania T/N z odpowiedziami

1. RISC typowo ma prosty zbiór rozkazów.  
   **T** - to podstawowa cecha RISC.

2. RISC typowo ma małą liczbę rejestrów specjalizowanych.  
   **N** - RISC typowo ma dużo rejestrów ogólnego przeznaczenia.

3. RISC typowo ma mało trybów adresowania.  
   **T** - upraszcza to dekodowanie i wykonanie instrukcji.

4. CISC typowo ma bardziej złożone instrukcje niż RISC.  
   **T** - to klasyczne rozróżnienie.

5. Każdy procesor CISC musi wewnętrznie tłumaczyć instrukcje na mikrooperacje RISC.  
   **N** - to może być sposób implementacji, ale nie definicja CISC.

6. Potokowanie polega na nachodzeniu na siebie etapów wykonania kolejnych instrukcji.  
   **T** - np. jedna instrukcja jest wykonywana, a kolejna dekodowana.

7. Potokowanie zawsze skraca czas wykonania pojedynczej instrukcji.  
   **N** - głównie zwiększa przepustowość.

8. RAW oznacza, że instrukcja chce czytać dane, których wcześniejsza instrukcja jeszcze nie zapisała.  
   **T** - to Read After Write.

9. WAR oznacza Write After Read.  
   **T** - zapis po odczycie.

10. Forwarding może ograniczać opóźnienia wynikające z zależności danych.  
    **T** - wynik można przekazać bez czekania na zapis do rejestru.

11. Skoki warunkowe mogą powodować problemy w potoku.  
    **T** - nie wiadomo od razu, którą instrukcję pobrać następną.

12. SIMD oznacza wykonywanie jednej instrukcji na wielu danych.  
    **T** - Single Instruction, Multiple Data.

13. Procesor superskalarny może próbować wykonywać kilka instrukcji równolegle.  
    **T** - jeśli są dostępne zasoby i nie blokują tego zależności.

14. W VLIW kompilator może mieć dużą rolę w ustalaniu operacji równoległych.  
    **T** - równoległość jest jawniej planowana przez kompilator.

15. NUMA oznacza, że każdy procesor ma całkowicie osobną przestrzeń adresową.  
    **N** - pamięć może być wspólna logicznie, ale dostęp do niej ma różne czasy.

16. SMP oznacza symetryczne wieloprocesorowe przetwarzanie ze wspólną pamięcią.  
    **T** - procesory są traktowane podobnie i korzystają ze wspólnej pamięci.

17. Rejestry IF/ID, ID/EX, EX/MEM i MEM/WB przechowują informacje między etapami potoku.  
    **T** - dzięki nim różne instrukcje mogą być jednocześnie w różnych etapach.

18. Etap ID zwykle odczytuje rejestry źródłowe instrukcji.  
    **T** - dlatego bywa opisany jako Instruction Decode / Register Fetch.

19. Etap EX może służyć do obliczenia adresu dla instrukcji dostępu do pamięci.  
    **T** - EX to Execute / Address Calculation.

20. Etap WB zapisuje wynik do pamięci danych.  
    **N** - WB zapisuje wynik do rejestru; pamięć danych jest używana w MEM.

21. MUX w potoku służy do wyboru jednej z możliwych wartości wejściowych.  
    **T** - np. wybiera argument ALU albo wartość zapisywaną w WB.

22. PC przechowuje adres instrukcji do pobrania.  
    **T** - Program Counter wskazuje, skąd pobrać instrukcję.

23. Czas cyklu potoku musi uwzględniać najwolniejszy etap.  
    **T** - każdy etap w potoku trwa pełny cykl.

24. Jeśli najwolniejszy etap trwa 4 ns, to pięcioetapowy potok wykonuje pojedynczą instrukcję w 5 ns.  
    **N** - pojedyncza instrukcja przechodzi 5 etapów, czyli 5 * 4 ns = 20 ns.

25. W idealnym pięcioetapowym potoku o cyklu 4 ns wykonanie `n` instrukcji trwa `4(n+4)` ns.  
    **T** - liczba cykli to `n + liczba_etapów - 1`.

## Miniściąga

| Hasło | Zapamiętaj |
|---|---|
| RISC | proste instrukcje, dużo rejestrów ogólnych, mało trybów adresowania |
| CISC | złożone instrukcje, więcej trybów adresowania |
| Potok | F/D/E/M/W nachodzą na siebie |
| IF/ID | rejestr między pobraniem a dekodowaniem |
| ID/EX | rejestr między dekodowaniem a wykonaniem |
| EX/MEM | rejestr między wykonaniem a pamięcią |
| MEM/WB | rejestr między pamięcią a zapisem wyniku |
| MUX | wybiera jedno z wejść |
| Register File | rejestry procesora |
| czas cyklu potoku | czas najwolniejszego etapu |
| 5 etapów, cykl 4 ns | jedna instrukcja trwa 20 ns |
| n instrukcji | w idealnym potoku 5-etapowym: `cykl * (n + 4)` |
| RAW | czytanie po zapisie |
| WAR | zapis po odczycie |
| WAW | zapis po zapisie |
| Forwarding | wynik idzie dalej bez pełnego czekania na write back |
| Branch | psuje potok, bo nie wiadomo co pobierać |
| Superskalarność | kilka instrukcji równolegle sprzętowo |
| SIMD | jedna instrukcja, wiele danych |
| VLIW/EPIC | równoległość mocniej planowana przez kompilator |
| SMP | wspólna pamięć, podobny dostęp |
| NUMA | wspólna logicznie pamięć, różne czasy dostępu |
