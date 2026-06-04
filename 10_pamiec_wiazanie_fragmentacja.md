# AKSO - pamięć, wiązanie adresów i fragmentacja

## Najkrócej

Zarządzanie pamięcią odpowiada za przydział pamięci procesom, ochronę oraz translację adresów.

Najważniejsze hasła:

- adres symboliczny, logiczny, fizyczny,
- wiązanie adresów,
- ładowanie i łączenie,
- przydział ciągły,
- strefy statyczne,
- fragmentacja wewnętrzna i zewnętrzna,
- strategie first fit, best fit, worst fit.

## Adresy

| Rodzaj adresu | Sens |
|---|---|
| symboliczny | nazwa/etykieta w programie źródłowym |
| logiczny/wirtualny | adres generowany przez program/procesor |
| fizyczny | rzeczywisty adres w pamięci RAM |

System pamięci i sprzęt translacji mogą zamieniać adres logiczny na fizyczny.

## Wiązanie adresów

**Wiązanie adresów** to przypisanie adresom symbolicznym/logicznych konkretnych adresów.

Może zachodzić:

- podczas kompilacji,
- podczas ładowania,
- podczas wykonania.

Jeśli wiązanie następuje podczas wykonania, program może być przenoszony w pamięci w trakcie działania. Wymaga to wsparcia sprzętowego.

## Łączenie i ładowanie

**Łączenie statyczne**: biblioteki są dołączane do programu przed uruchomieniem.

**Ładowanie dynamiczne**: moduł jest ładowany dopiero, gdy program go potrzebuje.

**Łączenie dynamiczne**: odwołania do bibliotek są rozwiązywane przy uruchomieniu lub w czasie działania.

## Zarządca pamięci

Zarządca pamięci:

- wie, które obszary są wolne,
- przydziela pamięć procesom,
- odbiera pamięć,
- wspiera ochronę,
- może współpracować z pamięcią pomocniczą.

## Prosta ochrona pamięci: rejestr bazowy i graniczny

Jednym z prostych sposobów ochrony pamięci procesu są dwa rejestry:

| Rejestr | Sens |
|---|---|
| bazowy | początek dozwolonego obszaru pamięci procesu |
| graniczny | rozmiar dozwolonego obszaru albo informacja o jego końcu, zależnie od konwencji |

W wariancie ze slajdu sprzęt sprawdza każdy adres:

```text
adres >= baza
adres <= baza + granica
```

Jeśli oba warunki są spełnione, dostęp do pamięci jest dozwolony.

Jeśli któryś warunek nie jest spełniony, sprzęt zgłasza przerwanie/wyjątek: **błąd adresowania**.

Przykład:

```text
baza = 1000
granica = 500
dozwolony zakres: 1000 ... 1500
```

Adres `1200` jest poprawny, ale `999` i `1600` powodują błąd.

Wartości rejestru bazowego i granicznego ustawia system operacyjny, np. przy przełączeniu kontekstu. Zmiana tych rejestrów jest operacją uprzywilejowaną, więc zwykły program użytkownika nie może sam zwiększyć sobie dozwolonego zakresu pamięci.

Egzaminowo:

- rejestr bazowy i graniczny mogą służyć do ochrony pamięci procesu: **T**,
- proces użytkownika może dowolnie zmieniać te rejestry: **N**,
- system może ustawiać te rejestry przy przełączaniu kontekstu: **T**,
- adres spoza zakresu powoduje błąd adresowania: **T**.

## Brak zarządzania i pojedynczy obszar

Najprostsze modele:

- brak zarządzania,
- system i jeden proces w pamięci,
- programy nierelokowalne.

Są proste, ale nie nadają się dobrze do wieloprogramowości.

## Wymiana

**Swapping** polega na przenoszeniu całych procesów między pamięcią operacyjną a pamięcią pomocniczą.

Wymiana jest wolna, bo wymaga I/O.

Kwant czasu powinien być dużo większy niż czas wymiany, inaczej system marnuje czas.

## Strefy statyczne

Pamięć jest podzielona na ustalone partycje/strefy.

Proces trafia do jednej strefy.

Problem: **fragmentacja wewnętrzna**.

Jeśli proces potrzebuje 30 KB, a dostaje strefę 200 KB, to 170 KB jest zmarnowane wewnątrz przydziału.

## Przydział ciągły

Proces dostaje jeden ciągły obszar pamięci.

Liczba i rozmiar dziur zmieniają się w trakcie działania systemu.

Problem: **fragmentacja zewnętrzna**.

Może być dużo wolnej pamięci łącznie, ale w kawałkach zbyt małych na żądanie.

## Strategie wyboru dziury

| Strategia | Sens |
|---|---|
| first fit | pierwsza wystarczająco duża dziura |
| best fit | najmniejsza wystarczająco duża dziura |
| worst fit | największa dziura |

Best fit nie zawsze jest najlepszy praktycznie, bo może produkować dużo bardzo małych dziur.

## Fragmentacja

**Fragmentacja wewnętrzna**: marnuje się część przydzielonego obszaru.

**Fragmentacja zewnętrzna**: wolna pamięć jest rozproszona w wielu dziurach.

| Fragmentacja | Gdzie typowo |
|---|---|
| wewnętrzna | strefy statyczne, stronicowanie |
| zewnętrzna | przydział ciągły zmiennych obszarów |

## Defragmentacja

Defragmentacja/kompakcja przesuwa procesy tak, żeby połączyć wolne dziury.

Jest możliwa sensownie wtedy, gdy adresy mogą być relokowane w czasie wykonania.

Jeśli adresy zostały związane podczas ładowania i program nie może być przesuwany, defragmentacja jest trudna albo niemożliwa.

## Pułapki egzaminacyjne

- Fragmentacja wewnętrzna dotyczy pamięci już przydzielonej.
- Fragmentacja zewnętrzna dotyczy wolnych dziur między przydziałami.
- Przydział ciągły może powodować fragmentację zewnętrzną.
- Strefy statyczne powodują fragmentację wewnętrzną.
- Best fit nie oznacza automatycznie najmniejszej fragmentacji globalnej.
- Relokacja podczas wykonania wymaga wsparcia sprzętowego.
- Swapping całych procesów jest kosztowny.
- Rejestr bazowy i graniczny to prosty mechanizm ochrony zakresu adresów procesu.

## Pytania T/N z odpowiedziami

1. Fragmentacja zewnętrzna występuje przy przydziale ciągłym zmiennych obszarów.  
   **T** - wolne dziury mogą być rozproszone.

2. Fragmentacja wewnętrzna polega na niewykorzystanej części przydzielonego obszaru.  
   **T** - pamięć jest przydzielona, ale niewykorzystana.

3. W strefach statycznych typowym problemem jest fragmentacja zewnętrzna, a nie wewnętrzna.  
   **N** - typowo marnuje się miejsce wewnątrz przydzielonej strefy.

4. W przydziale ciągłym suma wolnej pamięci może wystarczać, ale żądanie i tak się nie powiedzie.  
   **T** - jeśli nie ma jednej odpowiednio dużej dziury.

5. Best fit wybiera najmniejszy wystarczająco duży wolny obszar.  
   **T** - to definicja.

6. Worst fit wybiera największy wolny obszar.  
   **T** - zostawia możliwie dużą resztę.

7. Wiązanie adresów podczas wykonania ułatwia relokację procesu.  
   **T** - adresy mogą być tłumaczone dynamicznie.

8. Swapping nie wymaga operacji wejścia-wyjścia.  
   **N** - przenosi procesy do/z pamięci pomocniczej.

9. Defragmentacja jest łatwa, gdy adresy są na sztywno związane przy ładowaniu.  
   **N** - przesuwanie procesu byłoby problematyczne.

10. Adres fizyczny to rzeczywisty adres w pamięci operacyjnej.  
    **T** - po translacji.

11. Rejestr bazowy może określać początek dozwolonego obszaru pamięci procesu.  
    **T** - sprzęt porównuje adres z bazą.

12. Program użytkownika może samodzielnie zmienić rejestr bazowy i graniczny.  
    **N** - to operacja uprzywilejowana.

13. Adres spoza zakresu wyznaczonego przez bazę i granicę może spowodować błąd adresowania.  
    **T** - sprzęt odrzuca nielegalny dostęp.
