# AKSO - Architektura von Neumanna

## Najkrócej

**Architektura von Neumanna** to model komputera, w którym **program i dane są przechowywane w tej samej pamięci**.

Procesor pobiera instrukcje z pamięci, wykonuje je, a w razie potrzeby odczytuje lub zapisuje dane także w tej samej pamięci.

Egzaminacyjny skrót:

> Von Neumann = **wspólna pamięć dla programu i danych** + procesor wykonujący instrukcje krok po kroku.

## Główne elementy

| Element | Rola |
|---|---|
| Pamięć | Przechowuje program i dane |
| Jednostka sterująca | Pobiera instrukcje, dekoduje je i steruje wykonaniem |
| ALU | Wykonuje operacje arytmetyczne i logiczne |
| Akumulator | Rejestr przechowujący argumenty lub wyniki operacji |
| Wejście | Dostarcza dane do komputera |
| Wyjście | Odbiera wyniki działania programu |

## Pamięć

W architekturze von Neumanna pamięć przechowuje:

- instrukcje programu,
- dane używane przez program,
- wyniki pośrednie i końcowe.

To jest bardzo ważne, bo odróżnia ten model od architektury harwardzkiej, gdzie program i dane mają osobne pamięci.

## Jednostka sterująca

**Jednostka sterująca** odpowiada za kierowanie pracą procesora.

Robi między innymi:

- pobiera instrukcję z pamięci,
- rozpoznaje, co instrukcja ma zrobić,
- wydaje sygnały sterujące do ALU, pamięci i urządzeń wejścia-wyjścia,
- pilnuje kolejności wykonywania instrukcji.

Jednostka sterująca nie wykonuje samych obliczeń. Od obliczeń jest ALU.

## ALU

**ALU** to jednostka arytmetyczno-logiczna.

Wykonuje operacje takie jak:

- dodawanie,
- odejmowanie,
- porównywanie,
- AND, OR, NOT, XOR,
- przesunięcia bitowe.

ALU jest częścią procesora. W klasycznym schemacie von Neumanna współpracuje z akumulatorem.

## Akumulator

**Akumulator** to rejestr procesora używany do przechowywania danych podczas obliczeń.

Przykład:

1. Procesor pobiera liczbę z pamięci do akumulatora.
2. ALU dodaje do niej drugą liczbę.
3. Wynik trafia z powrotem do akumulatora.
4. Wynik może zostać zapisany do pamięci albo wysłany na wyjście.

## Wejście i wyjście

Urządzenia wejścia pozwalają wprowadzać dane do systemu, np. klawiatura, mysz, dysk, karta sieciowa.

Urządzenia wyjścia pozwalają odebrać wynik, np. monitor, drukarka, dysk, karta sieciowa.

Niektóre urządzenia mogą być jednocześnie wejściem i wyjściem, np. dysk albo karta sieciowa.

## Szyny

Elementy komputera komunikują się przez szyny.

| Szyna | Co przenosi |
|---|---|
| Szyna danych | Dane przesyłane między procesorem, pamięcią i urządzeniami |
| Szyna adresowa | Adres komórki pamięci albo urządzenia |
| Szyna sterująca | Sygnały typu odczyt, zapis, przerwanie |

Przykład odczytu z pamięci:

1. Procesor wystawia adres na szynie adresowej.
2. Procesor wysyła sygnał odczytu szyną sterującą.
3. Pamięć odsyła dane szyną danych.

## Cykl rozkazowy

Typowy cykl wykonania instrukcji:

1. **Pobranie** instrukcji z pamięci.
2. **Dekodowanie** instrukcji przez jednostkę sterującą.
3. **Wykonanie** instrukcji, np. przez ALU.
4. **Zapis wyniku**, jeśli instrukcja tego wymaga.

Po tym procesor przechodzi do następnej instrukcji.

## Wąskie gardło von Neumanna

Ponieważ instrukcje i dane korzystają z tej samej pamięci oraz często z tej samej drogi komunikacji, procesor może czekać na dostęp do pamięci.

To ograniczenie nazywa się **wąskim gardłem von Neumanna**.

Egzaminowo:

- wspólna pamięć dla programu i danych to cecha von Neumanna,
- wspólna droga dostępu do instrukcji i danych może ograniczać wydajność,
- cache i potokowanie mogą zmniejszać skutki tego problemu, ale nie zmieniają samej idei modelu.

## Von Neumann vs Harvard

| Cecha | Von Neumann | Harvard |
|---|---|---|
| Pamięć programu i danych | Wspólna | Osobna |
| Dostęp do instrukcji i danych | Przez wspólny system pamięci | Może odbywać się równolegle |
| Prostota | Zwykle prostsza koncepcyjnie | Zwykle bardziej rozdzielona sprzętowo |
| Typowa pułapka | Program i dane są w tej samej pamięci | Program i dane są w osobnych pamięciach |

## Pytania T/N z odpowiedziami

1. W architekturze von Neumanna program i dane są przechowywane w tej samej pamięci.  
   **T** - to najważniejsza cecha tego modelu.

2. ALU odpowiada za pobieranie i dekodowanie instrukcji.  
   **N** - robi to jednostka sterująca; ALU wykonuje operacje arytmetyczne i logiczne.

3. Jednostka sterująca kieruje działaniem pozostałych elementów procesora.  
   **T** - wydaje sygnały sterujące i pilnuje wykonania instrukcji.

4. Akumulator jest rejestrem procesora.  
   **T** - przechowuje dane lub wyniki operacji.

5. Szyna danych przenosi adres komórki pamięci.  
   **N** - adres przenosi szyna adresowa; szyna danych przenosi dane.

6. Szyna sterująca przenosi sygnały typu odczyt i zapis.  
   **T** - informuje, jaka operacja ma zostać wykonana.

7. W architekturze von Neumanna instrukcje programu mogą być traktowane jak dane zapisane w pamięci.  
   **T** - program też jest przechowywany w pamięci.

8. Architektura harwardzka ma wspólną pamięć programu i danych.  
   **N** - to cecha von Neumanna; Harvard rozdziela pamięć programu i danych.

9. Wąskie gardło von Neumanna wynika między innymi ze wspólnego dostępu do pamięci dla instrukcji i danych.  
   **T** - procesor może czekać na transfer z pamięci.

10. Urządzenie wejścia-wyjścia nie może być jednocześnie wejściem i wyjściem.  
    **N** - np. dysk i karta sieciowa działają w obie strony.

11. ALU jest częścią procesora.  
    **T** - razem z jednostką sterującą i rejestrami tworzy podstawowe elementy CPU.

12. W cyklu rozkazowym instrukcja jest najpierw wykonywana, a dopiero potem pobierana z pamięci.  
    **N** - najpierw trzeba ją pobrać, potem zdekodować i wykonać.

## Miniściąga

| Hasło | Znaczenie |
|---|---|
| Von Neumann | Wspólna pamięć programu i danych |
| Memory | Program + dane |
| Control Unit | Sterowanie wykonaniem instrukcji |
| ALU | Obliczenia i logika |
| Accumulator | Rejestr na dane/wyniki |
| Input | Dane do komputera |
| Output | Wyniki z komputera |
| Szyna danych | Przesyła dane |
| Szyna adresowa | Przesyła adresy |
| Szyna sterująca | Przesyła sygnały sterujące |

