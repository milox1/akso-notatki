# AKSO - zakleszczenia, stan bezpieczny i algorytm bankiera

## Najkrócej

**Zakleszczenie** występuje, gdy zbiór procesów czeka na zdarzenia, które mogą zostać spowodowane tylko przez inne procesy z tego samego zbioru.

W praktyce: każdy trzyma coś, czego potrzebuje ktoś inny, i nikt nie może ruszyć dalej.

## Cztery warunki konieczne zakleszczenia

Zakleszczenie może wystąpić, gdy jednocześnie zachodzą:

1. **wzajemne wykluczanie** - zasób nie może być używany przez wiele procesów naraz,
2. **przetrzymywanie i oczekiwanie** - proces trzyma zasoby i czeka na kolejne,
3. **brak wywłaszczeń** - zasobu nie można odebrać siłą,
4. **czekanie cykliczne** - istnieje cykl oczekiwania procesów na zasoby.

Jeśli usuniemy dowolny z tych warunków, zakleszczenie nie wystąpi.

## Graf przydziału zasobów

W grafie:

- procesy są jednym typem wierzchołków,
- zasoby drugim typem,
- krawędź proces -> zasób oznacza żądanie,
- krawędź zasób -> proces oznacza przydział.

Jeśli jest zakleszczenie, to w grafie istnieje cykl.

Pułapka:

> Cykl jest warunkiem koniecznym zakleszczenia, ale nie zawsze wystarczającym, gdy zasoby mają wiele egzemplarzy.

## Strategie radzenia sobie

| Strategia | Sens |
|---|---|
| ignorowanie | zakładamy, że problem jest rzadki |
| zapobieganie | uniemożliwiamy spełnienie któregoś warunku koniecznego |
| unikanie | przydzielamy zasoby tylko, gdy system zostaje w stanie bezpiecznym |
| wykrywanie | dopuszczamy zakleszczenia i okresowo je wykrywamy |
| usuwanie | kończymy procesy albo odbieramy zasoby |

## Stan bezpieczny

Stan jest **bezpieczny**, jeśli istnieje bezpieczny ciąg procesów, czyli taka kolejność, w której wszystkie procesy mogą zakończyć działanie.

Ważne:

- stan bezpieczny nie oznacza, że nikt nie czeka,
- stan bezpieczny oznacza, że istnieje plan wyjścia,
- stan niebezpieczny nie musi już być zakleszczeniem, ale może do niego prowadzić.

## Algorytm bankiera

Algorytm bankiera służy do **unikania zakleszczeń**.

Wymaga, żeby procesy znały i deklarowały maksymalne zapotrzebowanie na zasoby.

Typowe struktury:

- dostępne zasoby,
- maksymalne zapotrzebowanie,
- aktualny przydział,
- jeszcze potrzebne zasoby.

Zasób przydziela się tylko wtedy, gdy po symulacji przydziału system nadal jest w stanie bezpiecznym.

## Wykrywanie zakleszczeń

Jeśli system nie zapobiega i nie unika zakleszczeń, może je wykrywać.

Algorytm wykrywania sprawdza, czy da się znaleźć procesy, których żądania można zaspokoić dostępnymi zasobami, potem symuluje ich zakończenie i zwrot zasobów.

Jeśli pewne procesy nie mogą zostać oznaczone jako kończące, są podejrzane o zakleszczenie.

## Usuwanie zakleszczeń

Możliwe działania:

- zakończyć wszystkie zakleszczone procesy,
- kończyć procesy pojedynczo,
- odebrać zasoby wybranym procesom,
- cofnąć proces do wcześniejszego stanu, jeśli system to wspiera.

To zwykle jest kosztowne albo brutalne, dlatego systemy często wolą zapobieganie, unikanie albo ignorowanie problemu.

## Pułapki egzaminacyjne

- Zakleszczenie wymaga spełnienia wszystkich czterech warunków koniecznych.
- Cykl w grafie jest konieczny, ale nie zawsze wystarczający.
- Stan bezpieczny oznacza istnienie bezpiecznego ciągu.
- Stan bezpieczny nie oznacza, że żaden proces nie czeka.
- Algorytm bankiera wymaga deklaracji maksymalnych potrzeb.
- Bankier unika zakleszczeń, a nie tylko je wykrywa.
- Wykrywanie zakleszczeń może dopuszczać powstanie zakleszczenia.

## Pytania T/N z odpowiedziami

1. Do zakleszczenia mogą doprowadzić tylko sytuacje, w których spełnione są wszystkie cztery warunki konieczne.  
   **T** - brak jednego z nich uniemożliwia zakleszczenie.

2. Jeśli w grafie przydziału zasobów jest zakleszczenie, to istnieje cykl.  
   **T** - cykl jest warunkiem koniecznym.

3. Jeśli w grafie przydziału zasobów istnieje cykl, to zawsze jest zakleszczenie.  
   **N** - przy wielu egzemplarzach zasobów cykl nie musi wystarczać.

4. Stan bezpieczny oznacza, że istnieje bezpieczny ciąg procesów.  
   **T** - to definicja.

5. Stan bezpieczny oznacza, że żaden proces nie czeka na zasoby.  
   **N** - procesy mogą czekać, ale istnieje kolejność pozwalająca wszystkim skończyć.

6. Algorytm bankiera wymaga znajomości maksymalnych potrzeb procesów.  
   **T** - bez tego nie da się sprawdzić bezpieczeństwa.

7. Algorytm bankiera jest algorytmem wykrywania zakleszczeń po fakcie.  
   **N** - służy do unikania zakleszczeń.

8. Zapobieganie zakleszczeniom polega na niedopuszczeniu do spełnienia co najmniej jednego warunku koniecznego.  
   **T** - to klasyczna metoda.

9. Brak wywłaszczeń jest jednym z warunków koniecznych zakleszczenia.  
   **T** - zasób nie może być odebrany siłą.

10. Czekanie cykliczne nie ma związku z zakleszczeniami.  
    **N** - to jeden z warunków koniecznych.

