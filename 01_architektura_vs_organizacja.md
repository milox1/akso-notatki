# AKSO - Architektura komputera vs organizacja komputera

## Najkrócej

**Architektura komputera** mówi, co jest widzialne dla programisty i co wpływa na sposób tworzenia programu.

**Organizacja komputera** mówi, jak dana architektura jest fizycznie lub technicznie zrealizowana.

Egzaminacyjny skrót:

> Architektura = **co program widzi i czego musi przestrzegać**.  
> Organizacja = **jak sprzęt to robi w środku**.

## Architektura komputera

Architektura obejmuje atrybuty komputera widzialne dla programisty, zwłaszcza takie, które wpływają na sposób pisania, kompilowania albo uruchamiania programu.

Typowe elementy architektury:

- lista rozkazów procesora, czyli ISA,
- formaty instrukcji,
- rejestry widoczne dla programisty,
- typy i formaty danych,
- długość słowa, jeśli wpływa na model programowania,
- tryby adresowania,
- model adresowania pamięci,
- sposób reprezentacji liczb,
- mechanizmy przerwań i wyjątków widziane przez program,
- zasady wejścia-wyjścia widoczne na poziomie programu,
- endianowość, jeśli program musi ją uwzględniać.

Architektura bywa trwała. Ta sama architektura może przetrwać wiele lat i być realizowana przez różne generacje sprzętu.

Przykład: program napisany dla pewnej architektury może działać na wielu procesorach tej samej rodziny, mimo że ich wnętrze, taktowanie, cache albo potok są inne.

## Organizacja komputera

Organizacja to sposób realizacji konkretnej architektury. Dotyczy tego, jak elementy sprzętu są połączone i jak wykonują operacje opisane przez architekturę.

Typowe elementy organizacji:

- budowa jednostki sterującej,
- budowa ALU,
- potokowanie instrukcji,
- wykonanie superskalarne,
- mikroprogramowanie,
- liczba i rodzaj jednostek wykonawczych,
- taktowanie procesora,
- rozmiary i organizacja pamięci cache,
- magistrale i sposób komunikacji między modułami,
- technologia pamięci operacyjnej,
- sposób podłączenia urządzeń wejścia-wyjścia,
- szczegóły kontrolerów sprzętowych.

Organizacja zmienia się szybciej niż architektura, bo zależy od technologii i konkretnej implementacji.

## Tabela różnic

| Kryterium | Architektura komputera | Organizacja komputera |
|---|---|---|
| Główne pytanie | Co widzi programista? | Jak to jest zrealizowane? |
| Poziom opisu | Model widoczny dla programu | Wewnętrzna budowa i działanie sprzętu |
| Wpływ na program | Może wymagać innego kodu, kompilatora lub sposobu programowania | Zwykle wpływa głównie na wydajność, koszt i zużycie energii |
| Trwałość | Zwykle bardziej trwała | Zmienia się szybciej |
| Przykład | Lista rozkazów, rejestry, tryby adresowania | Potok, cache, magistrala, taktowanie |

## Najważniejsza pułapka

Nie wszystko, co wpływa na szybkość programu, jest architekturą.

Cache, potokowanie albo częstotliwość taktowania mogą bardzo mocno wpływać na wydajność, ale zwykle są elementami organizacji, bo program nie musi znać ich dokładnej realizacji, żeby być poprawny.

Z drugiej strony, jeśli jakaś cecha wpływa na to, jakie instrukcje można wykonać, jak adresuje się pamięć albo jakie rejestry są dostępne, to jest to raczej architektura.

## Jak rozpoznawać na egzaminie T/N

Jeśli zdanie mówi o tym, że coś jest:

- widzialne dla programisty,
- częścią modelu programowania,
- potrzebne do poprawnego napisania lub skompilowania programu,
- związane z listą rozkazów, rejestrami albo trybami adresowania,

to najczęściej chodzi o **architekturę**.

Jeśli zdanie mówi o tym, że coś jest:

- sposobem realizacji,
- szczegółem sprzętowym,
- zależne od technologii,
- związane z potokiem, cache, magistralą, taktowaniem albo jednostkami wykonawczymi,

to najczęściej chodzi o **organizację**.

## Pytania T/N z odpowiedziami

1. Architektura komputera obejmuje atrybuty widzialne dla programisty.  
   **T** - to jest podstawowa definicja architektury.

2. Organizacja komputera opisuje sposób realizacji konkretnej architektury.  
   **T** - organizacja odpowiada na pytanie, jak architektura jest wykonana.

3. Organizacja komputera bywa trwalsza niż architektura komputera.  
   **N** - zwykle odwrotnie: architektura jest trwalsza, organizacja szybciej zmienia się z technologią.

4. Lista rozkazów procesora jest elementem organizacji komputera.  
   **N** - lista rozkazów, czyli ISA, jest elementem architektury.

5. Potokowanie instrukcji jest elementem organizacji komputera.  
   **T** - to sposób realizacji wykonywania rozkazów.

6. Dwa procesory mogą mieć tę samą architekturę, ale różną organizację.  
   **T** - mogą wykonywać ten sam kod, ale mieć inne wnętrze.

7. Zmiana organizacji zawsze wymaga zmiany programu.  
   **N** - często zmienia się tylko wydajność, a program dalej działa tak samo.

8. Zmiana architektury może wymagać zmiany programu albo rekompilacji.  
   **T** - inna architektura może mieć inne rozkazy, rejestry lub model adresowania.

9. Rozmiar cache jest zawsze elementem architektury, bo wpływa na szybkość programu.  
   **N** - wpływ na szybkość nie wystarcza; cache zwykle należy do organizacji.

10. Tryby adresowania są elementem architektury komputera.  
    **T** - programista lub kompilator musi wiedzieć, jak można adresować dane.

11. Taktowanie procesora jest typowym elementem organizacji komputera.  
    **T** - to cecha konkretnej realizacji sprzętu.

12. Rejestry widoczne w instrukcjach procesora są elementem architektury.  
    **T** - program może się do nich bezpośrednio odwoływać.

13. Magistrala i sposób połączenia modułów komputera to typowe elementy organizacji.  
    **T** - opisują realizację sprzętową.

14. Atrybut wpływający na sposób tworzenia programu należy raczej do architektury.  
    **T** - to dokładnie kryterium z definicji.

15. Organizacja komputera zmienia się szybko wraz z rozwojem technologii.  
    **T** - nowe technologie dają nowe sposoby realizacji tej samej architektury.

## Miniściąga

| Hasło | Odpowiedź |
|---|---|
| Widzi programista | Architektura |
| Wpływa na pisanie programu | Architektura |
| Lista rozkazów | Architektura |
| Rejestry widoczne dla programu | Architektura |
| Tryby adresowania | Architektura |
| Realizacja sprzętowa | Organizacja |
| Potok | Organizacja |
| Cache | Organizacja, o ile nie zmienia modelu programowania |
| Magistrala | Organizacja |
| Taktowanie | Organizacja |

