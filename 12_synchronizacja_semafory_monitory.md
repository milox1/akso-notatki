# AKSO - synchronizacja, semafory, monitory i sekcja krytyczna

## Najkrócej

Synchronizacja jest potrzebna, gdy procesy lub wątki współdzielą dane albo zasoby.

Najważniejsze hasła:

- sekcja krytyczna,
- wzajemne wykluczanie,
- bezpieczeństwo,
- żywotność,
- aktywne oczekiwanie,
- semafor silny i słaby,
- monitor,
- zmienna warunkowa,
- semantyka Hoare'a i Mesy.

## Sekcja krytyczna

**Sekcja krytyczna** to fragment kodu, w którym proces korzysta ze współdzielonego zasobu.

Poprawne rozwiązanie problemu sekcji krytycznej powinno mieć:

- **bezpieczeństwo** - w sekcji krytycznej jest naraz co najwyżej jeden proces,
- **żywotność** - jeśli proces chce wejść do sekcji krytycznej, to powinien kiedyś móc wejść,
- sensowne oczekiwanie - procesy nie powinny być bez potrzeby blokowane.

## Bezpieczeństwo vs żywotność

| Własność | Sens |
|---|---|
| bezpieczeństwo | nic złego się nie stanie |
| żywotność | coś dobrego w końcu się stanie |

W sekcji krytycznej:

- bezpieczeństwo = nie ma dwóch procesów naraz w sekcji,
- żywotność = proces nie czeka wiecznie mimo możliwości wejścia.

Pułapka:

> Warunek "każdy proces w końcu wejdzie do sekcji krytycznej" jest mocniejszy niż typowa żywotność i nie zawsze jest sensowny, jeśli proces może nie chcieć wejść.

## Aktywne oczekiwanie

**Aktywne oczekiwanie** oznacza, że proces czeka w pętli, zużywając CPU.

Występuje np. przy:

- spinlockach,
- test-and-set,
- prostych algorytmach programowych typu Peterson.

Aktywne oczekiwanie bywa dobre tylko przy bardzo krótkim czekaniu albo w jądrze, gdy uśpienie byłoby droższe.

## Semafor

Semafor to zmienna synchronizacyjna z operacjami:

- `P`/`wait` - zajmij zasób albo zaśnij,
- `V`/`signal` - zwolnij zasób albo obudź czekającego.

Semafor binarny ma wartości 0/1 i może działać jak mutex.

Semafor licznikowy może reprezentować wiele egzemplarzy zasobu.

## Semafor silny i słaby

**Semafor silny** gwarantuje uczciwość budzenia, np. kolejność FIFO albo brak zagłodzenia czekających.

**Semafor słaby** nie daje takiej gwarancji.

Pułapka:

> Semafor silny nie musi być binarny. "Silny" dotyczy uczciwości budzenia, nie zakresu wartości.

## Monitory

Monitor to konstrukcja synchronizacyjna, która łączy:

- dane współdzielone,
- procedury operujące na tych danych,
- automatyczne wzajemne wykluczanie wewnątrz monitora.

W danej chwili w monitorze może wykonywać się tylko jeden proces/wątek.

## Zmienne warunkowe

Zmienne warunkowe służą do czekania na warunek wewnątrz monitora.

Operacje:

- `wait(c)` - proces zasypia na warunku `c` i zwalnia monitor,
- `signal(c)` - budzi jeden proces czekający na `c`, jeśli taki istnieje.

Samo `signal` nie oznacza magicznie, że warunek nadal jest prawdziwy w chwili wznowienia procesu. To zależy od semantyki monitora.

## Semantyka Hoare'a

W semantyce Hoare'a proces obudzony przez `signal` dostaje monitor natychmiast.

Proces sygnalizujący oddaje monitor obudzonemu i czeka.

To daje **warunek natychmiastowego wznowienia**.

## Semantyka Mesy

W semantyce Mesy `signal` tylko przenosi proces do kolejki gotowych do wejścia do monitora.

Proces sygnalizujący działa dalej.

Obudzony proces musi ponownie rywalizować o wejście do monitora, więc po wznowieniu powinien jeszcze raz sprawdzić warunek.

Dlatego w Mesie zwykle używa się:

```text
while (!warunek) wait(c);
```

a nie samego `if`.

## Brinch Hansen

W semantyce Brinch Hansena sygnalizowanie jest ograniczone zwykle do wyjścia z monitora.

Egzaminowo najważniejsze jest odróżnienie:

- Hoare - natychmiastowe wznowienie obudzonego,
- Mesa - obudzony czeka na ponowne wejście,
- Brinch Hansen - signal przy opuszczaniu monitora.

## Pułapki egzaminacyjne

- Bezpieczeństwo nie oznacza żywotności.
- Żywotność nie oznacza, że naraz jest tylko jeden proces w sekcji.
- Aktywne oczekiwanie zużywa CPU.
- Semafor silny dotyczy uczciwości budzenia, nie tego, czy jest binarny.
- Monitor zapewnia wzajemne wykluczanie procedur monitorowych.
- `wait` na zmiennej warunkowej zwalnia monitor.
- Hoare wymusza natychmiastowe wznowienie obudzonego procesu.
- Mesa nie wymusza natychmiastowego wznowienia, więc warunek trzeba sprawdzać ponownie.

## Pytania T/N z odpowiedziami

1. Bezpieczeństwo rozwiązania sekcji krytycznej oznacza, że naraz w sekcji krytycznej jest co najwyżej jeden proces.  
   **T** - to podstawowa własność safety.

2. Żywotność oznacza dokładnie to samo co bezpieczeństwo.  
   **N** - żywotność mówi, że postęp kiedyś nastąpi.

3. Aktywne oczekiwanie polega na czekaniu w pętli z użyciem CPU.  
   **T** - proces nie śpi, tylko kręci się w pętli.

4. Spinlock jest przykładem mechanizmu z aktywnym oczekiwaniem.  
   **T** - czeka przez sprawdzanie blokady.

5. Semafor silny gwarantuje pewną uczciwość budzenia procesów.  
   **T** - np. brak zagłodzenia czekających.

6. Semafor silny musi być semaforem binarnym.  
   **N** - silny/słaby dotyczy kolejki i uczciwości, nie tylko wartości 0/1.

7. Monitor pozwala wielu procesom wykonywać procedury monitorowe jednocześnie.  
   **N** - monitor zapewnia wzajemne wykluczanie.

8. `wait` na zmiennej warunkowej zwalnia monitor.  
   **T** - inaczej nikt nie mógłby zmienić warunku.

9. W semantyce Hoare'a obudzony proces wznawia działanie natychmiast po sygnale.  
   **T** - to warunek natychmiastowego wznowienia.

10. W semantyce Mesy obudzony proces powinien ponownie sprawdzić warunek.  
    **T** - inny proces mógł zmienić stan przed jego wejściem.

11. Signal w Mesie automatycznie wyrzuca proces sygnalizujący z monitora.  
    **N** - sygnalizujący zwykle działa dalej.

12. Warunek natychmiastowego wznowienia jest charakterystyczny dla semantyki Hoare'a.  
    **T** - egzaminowo to najważniejsze skojarzenie.

## Miniściąga

| Hasło | Zapamiętaj |
|---|---|
| bezpieczeństwo | co najwyżej jeden w sekcji krytycznej |
| żywotność | czekający proces nie powinien czekać wiecznie |
| aktywne oczekiwanie | pętla zużywająca CPU |
| test-and-set | sprzętowa instrukcja do blokad |
| semafor silny | uczciwe budzenie |
| semafor słaby | brak gwarancji uczciwości |
| monitor | dane + procedury + wzajemne wykluczanie |
| condition wait | śpi i zwalnia monitor |
| Hoare | obudzony wchodzi natychmiast |
| Mesa | obudzony czeka na wejście, sprawdź warunek ponownie |

