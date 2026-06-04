# AKSO - pułapki Tak/Nie

Ten plik jest do dopisywania rzeczy, na których łatwo stracić punkt. Każda pozycja ma mieć krótką formę:

> **Pułapka:** zdanie, które może paść na egzaminie.  
> **Odpowiedź:** T/N.  
> **Dlaczego:** jedno zdanie wyjaśnienia.

## Architektura i organizacja

**Pułapka:** Organizacja komputera jest trwalsza niż architektura.  
**Odpowiedź:** N.  
**Dlaczego:** zwykle architektura jest trwalsza, a organizacja zmienia się szybciej wraz z technologią.

**Pułapka:** Lista rozkazów procesora jest elementem organizacji komputera.  
**Odpowiedź:** N.  
**Dlaczego:** lista rozkazów, czyli ISA, jest elementem architektury.

**Pułapka:** Cache jest architekturą, bo wpływa na szybkość programu.  
**Odpowiedź:** N.  
**Dlaczego:** sam wpływ na wydajność nie wystarcza; cache zwykle jest elementem organizacji.

## Von Neumann

**Pułapka:** W architekturze von Neumanna program i dane są w osobnych pamięciach.  
**Odpowiedź:** N.  
**Dlaczego:** von Neumann ma wspólną pamięć programu i danych; osobne pamięci są typowe dla architektury harwardzkiej.

**Pułapka:** ALU pobiera i dekoduje instrukcje.  
**Odpowiedź:** N.  
**Dlaczego:** pobieraniem i dekodowaniem kieruje jednostka sterująca, a ALU wykonuje obliczenia i operacje logiczne.

**Pułapka:** Szyna danych przenosi adres komórki pamięci.  
**Odpowiedź:** N.  
**Dlaczego:** adres przenosi szyna adresowa, a szyna danych przenosi dane.

## RISC/CISC i potok

**Pułapka:** RISC typowo ma mało rejestrów specjalizowanych.  
**Odpowiedź:** N.  
**Dlaczego:** RISC typowo ma dużo rejestrów ogólnego przeznaczenia.

**Pułapka:** RISC typowo ma małą liczbę trybów adresowania.  
**Odpowiedź:** T.  
**Dlaczego:** prostota rozkazów i mała liczba trybów adresowania to typowa cecha RISC.

**Pułapka:** Skoki warunkowe ułatwiają potokowanie.  
**Odpowiedź:** N.  
**Dlaczego:** skok utrudnia potokowanie, bo nie wiadomo od razu, którą instrukcję pobierać dalej.

**Pułapka:** Forwarding pozwala wykorzystać wynik przed jego pełnym zapisaniem do rejestru.  
**Odpowiedź:** T.  
**Dlaczego:** wynik może zostać przekazany bezpośrednio do następnego etapu potoku.

## Procesy

**Pułapka:** PCB zawiera pełną zawartość stron pamięci procesu.  
**Odpowiedź:** N.  
**Dlaczego:** PCB zawiera metadane procesu, np. stan, rejestry, licznik rozkazów i informacje o pamięci, ale nie kopię całej przestrzeni adresowej.

**Pułapka:** Proces gotowy i proces aktywny to to samo.  
**Odpowiedź:** N.  
**Dlaczego:** gotowy czeka na CPU, aktywny właśnie wykonuje się na CPU.

**Pułapka:** Funkcje systemowe Linuksa wykonują się w trybie użytkownika.  
**Odpowiedź:** N.  
**Dlaczego:** kod jądra obsługujący syscall wykonuje się w trybie uprzywilejowanym.

**Pułapka:** Przełączenie kontekstu polega na zmianie trybu użytkownika na tryb jądra.  
**Odpowiedź:** N.  
**Dlaczego:** zmiana trybu user/kernel to nie to samo co przełączenie kontekstu procesu A na proces B.

**Pułapka:** W kolejkach procesów system trzyma pełną pamięć procesów.  
**Odpowiedź:** N.  
**Dlaczego:** w kolejkach są PCB albo wskaźniki do PCB, nie kopie całej przestrzeni adresowej.

**Pułapka:** `exec` tworzy nowy proces potomny.  
**Odpowiedź:** N.  
**Dlaczego:** `exec` podmienia program w bieżącym procesie; nowy proces tworzy `fork`.

**Pułapka:** Przerwania sprzętowe są synchroniczne względem wykonywanej instrukcji.  
**Odpowiedź:** N.  
**Dlaczego:** przerwania sprzętowe są asynchroniczne; synchroniczne są np. wyjątki i pułapki programowe.

## Szeregowanie

**Pułapka:** SJF jest wywłaszczający, a SRTF niewywłaszczający.  
**Odpowiedź:** N.  
**Dlaczego:** SJF jest klasycznie niewywłaszczający, a SRTF jest wywłaszczającą wersją SJF.

**Pułapka:** RR nie wymaga czasomierza.  
**Odpowiedź:** N.  
**Dlaczego:** koniec kwantu czasu musi być sygnalizowany, zwykle przerwaniem zegarowym.

**Pułapka:** Bardzo duży kwant w RR upodabnia algorytm do FCFS.  
**Odpowiedź:** T.  
**Dlaczego:** proces rzadko zostaje przerwany z powodu końca kwantu.

## Synchronizacja i zakleszczenia

**Pułapka:** Cykl w grafie przydziału zasobów zawsze oznacza zakleszczenie.  
**Odpowiedź:** N.  
**Dlaczego:** cykl jest konieczny, ale przy wielu egzemplarzach zasobów nie zawsze wystarcza.

**Pułapka:** Stan bezpieczny oznacza, że żaden proces nie oczekuje na zasoby.  
**Odpowiedź:** N.  
**Dlaczego:** stan bezpieczny oznacza istnienie bezpiecznego ciągu, a nie brak oczekiwania.

**Pułapka:** Algorytm bankiera służy do wykrywania zakleszczeń po ich wystąpieniu.  
**Odpowiedź:** N.  
**Dlaczego:** bankier służy do unikania zakleszczeń przez sprawdzanie stanu bezpiecznego.

**Pułapka:** Semafor silny musi być semaforem binarnym.  
**Odpowiedź:** N.  
**Dlaczego:** silny/słaby dotyczy uczciwości budzenia, a nie tego, czy semafor ma tylko wartości 0/1.

**Pułapka:** W semantyce Mesy `signal` natychmiast przekazuje monitor obudzonemu procesowi.  
**Odpowiedź:** N.  
**Dlaczego:** natychmiastowe wznowienie jest charakterystyczne dla Hoare'a; w Mesie obudzony czeka na ponowne wejście.

## Pamięć

**Pułapka:** Fragmentacja zewnętrzna polega na niewykorzystanej części przydzielonego procesu obszaru.  
**Odpowiedź:** N.  
**Dlaczego:** to fragmentacja wewnętrzna; zewnętrzna dotyczy wolnych dziur między przydziałami.

**Pułapka:** Przydział ciągły może nie spełnić żądania mimo wystarczającej sumy wolnej pamięci.  
**Odpowiedź:** T.  
**Dlaczego:** wolna pamięć może być rozbita na zbyt małe dziury.

**Pułapka:** Defragmentacja jest zawsze łatwa, niezależnie od momentu wiązania adresów.  
**Odpowiedź:** N.  
**Dlaczego:** przesuwanie procesów wymaga możliwości relokacji, najlepiej wiązania podczas wykonania.

**Pułapka:** Program użytkownika może sam zmienić rejestr bazowy i graniczny.  
**Odpowiedź:** N.  
**Dlaczego:** rejestry ochrony pamięci ustawia system w trybie uprzywilejowanym, np. przy przełączeniu kontekstu.

## Stronicowanie i TLB

**Pułapka:** Offset zmienia się podczas translacji adresu w stronicowaniu.  
**Odpowiedź:** N.  
**Dlaczego:** zmienia się numer strony na numer ramki, offset zostaje taki sam.

**Pułapka:** Page fault jest zgłaszany przez system operacyjny.  
**Odpowiedź:** N.  
**Dlaczego:** zgłasza go sprzęt, a system operacyjny go obsługuje.

**Pułapka:** FIFO jest wolny od anomalii Belady'ego.  
**Odpowiedź:** N.  
**Dlaczego:** FIFO może wykazywać anomalię Belady'ego.

**Pułapka:** Przy migotaniu stron pomaga zwiększenie stopnia wieloprogramowości.  
**Odpowiedź:** N.  
**Dlaczego:** zwykle pomaga zmniejszenie liczby aktywnych procesów i zwolnienie ramek.

## System plików

**Pułapka:** I-węzeł na dysku zawiera nazwę pliku.  
**Odpowiedź:** N.  
**Dlaczego:** nazwa jest w katalogu, który mapuje nazwę na i-węzeł.

**Pułapka:** I-węzeł zawiera aktualną pozycję odczytu/zapisu procesu.  
**Odpowiedź:** N.  
**Dlaczego:** pozycja jest w strukturze otwartego pliku.

**Pułapka:** VFS to konkretny system plików.  
**Odpowiedź:** N.  
**Dlaczego:** VFS jest warstwą abstrakcji nad różnymi systemami plików.

## Wejście-wyjście

**Pułapka:** Polling wymaga kontrolera przerwań.  
**Odpowiedź:** N.  
**Dlaczego:** polling polega na odpytywaniu urządzenia przez CPU i nie wymaga przerwań.

**Pułapka:** DMA wymaga, żeby CPU kopiował każdy bajt transmisji.  
**Odpowiedź:** N.  
**Dlaczego:** DMA właśnie ogranicza udział CPU w przesyłaniu dużych bloków danych.

**Pułapka:** Koniec transmisji DMA jest zwykle sygnalizowany przerwaniem.  
**Odpowiedź:** T.  
**Dlaczego:** CPU ustawia transfer, a po zakończeniu dostaje informację, często przez przerwanie.

**Pułapka:** Operacje wejścia-wyjścia są porównywalnie szybkie z dostępem do pamięci RAM.  
**Odpowiedź:** N.  
**Dlaczego:** I/O jest zwykle o rzędy wielkości wolniejsze, więc proces czekający na I/O jest często wstrzymywany.
