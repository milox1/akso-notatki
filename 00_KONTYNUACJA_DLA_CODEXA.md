# AKSO - kontekst do kontynuacji na innym Codexie

Ten plik służy do szybkiego przeniesienia pracy na inny komputer / inną sesję Codexa.

Repo:

```text
https://github.com/milox1/akso-notatki
```

Egzaminy:

- **17 czerwca 2026** - matematyka dyskretna, cel minimum 55%.
- **19 czerwca 2026** - AKSO/SO, cel awaryjny minimum 16/30, realnie celować wyżej.

## Jak zacząć na drugim komputerze

Sklonuj repo:

```bash
git clone https://github.com/milox1/akso-notatki.git
cd akso-notatki
```

Potem w nowym Codexie napisz:

```text
Przeczytaj 00_KONTYNUACJA_DLA_CODEXA.md i pomagaj mi dalej uczyć się AKSO.
Uczę się przez slajdy: wysyłam screen/pytanie, ty tłumaczysz prosto,
dajesz pułapki Tak/Nie i jeśli trzeba dopisujesz do notatek oraz commitujesz/pushujesz.
```

## Styl nauki użytkownika

Użytkownik uczy się aktywnie:

- czyta slajdy,
- sprawdza, czy rozumie mechanizm,
- wysyła screeny i pyta “co to” albo “czy dobrze rozumiem”,
- chce krótkie, konkretne wyjaśnienia,
- lubi testy T/N i krótkie pytania otwarte,
- chce dopisywania braków do notatek,
- chce, żeby zmiany trafiały na GitHub.

Najlepszy format odpowiedzi:

- prosto po polsku,
- bez lania wody,
- z pułapkami egzaminacyjnymi,
- z przykładami,
- z odpowiedziami `T/N`,
- przy trudnym pojęciu: najpierw intuicja, potem egzaminowy skrót.

## Stan wiedzy na 8 czerwca 2026

Przerobione/ogólnie ogarnięte:

- architektura vs organizacja,
- von Neumann vs Harvard,
- ALU, szyny,
- RISC/CISC,
- potok `F/D/E/M/W`,
- hazardy, RAW, forwarding, load-use,
- superskalarność, SIMD, VLIW, EPIC, SMP, NUMA,
- proces vs program,
- przestrzeń adresowa `text/data/heap/stack`,
- PCB jako metryczka procesu,
- stany procesu: gotowy/aktywny/wstrzymany/zakończony,
- przełączenie kontekstu vs zmiana trybu,
- syscall, ring 0/ring 3,
- przerwania sprzętowe vs wyjątki/pułapki,
- `fork` vs `exec`,
- podstawy szeregowania,
- RR, SRTF, narzut przełączania kontekstu,
- I/O-bound vs CPU-bound,
- szeregowanie wielopoziomowe ze sprzężeniem zwrotnym.

W trakcie:

- szeregowanie procesów, szczególnie zadania i niuanse:
  - FCFS,
  - SJF,
  - SRTF,
  - RR,
  - priorytety,
  - kolejki wielopoziomowe,
  - czasy obrotu/oczekiwania/odpowiedzi.

Jeszcze do mocnego przerobienia:

- synchronizacja: semafory, monitory, Hoare/Mesa,
- zakleszczenia, bankier,
- pamięć: fragmentacja, buddy,
- stronicowanie, TLB, page fault,
- FIFO/LRU/OPT,
- thrashing,
- system plików: i-węzły, deskryptory, VFS, ext2,
- I/O: DMA, polling, przerwania,
- zadania obliczeniowe ze starych egzaminów.

## Najważniejsze notatki

Plan i drzewo:

- `00_START_TUTAJ_PLAN_AKSO.md`
- `00_DRZEWO_NAUKI_AKSO.md`
- `00_ZAKRES_SLAJDOW_AKSO3.md`

Teoria:

- `01_architektura_vs_organizacja.md`
- `02_architektura_von_neumanna.md`
- `03_architektura_procesora_risc_cisc_potok.md`
- `04_bity_x86_assembler.md`
- `05_procesy_stany_syscall_przerwania.md`
- `06_szeregowanie_procesow.md`
- `07_zakleszczenia_bankier.md`
- `08_io_dyski_dma.md`
- `09_system_plikow_vfs_inode_deskryptory.md`
- `10_pamiec_wiazanie_fragmentacja.md`
- `11_stronicowanie_tlb_wirtualna.md`
- `12_synchronizacja_semafory_monitory.md`

Ćwiczenia:

- `97_ZADANIA_OBLICZENIOWE_SKAD_I_CO.md`
- `98_BANK_PYTAN_TAK_NIE.md`
- `99_PULAPKI_TAK_NIE.md`

## Jak odpowiadać użytkownikowi

Gdy użytkownik wrzuca slajd:

1. Wyjaśnij, co pokazuje slajd.
2. Powiedz, z którego działu to jest.
3. Podaj pułapki `T/N`.
4. Jeśli jest dziura w notatkach, dopisz ją do właściwego pliku.
5. Jeśli użytkownik chce, zrób commit i push.

Gdy użytkownik mówi `test ...`:

1. Daj pytania `T/N` albo krótkie otwarte.
2. Sprawdź odpowiedzi.
3. Wyjaśnij błędy.
4. Zapisz często mylone rzeczy do `99_PULAPKI_TAK_NIE.md`, jeśli warto.

Gdy użytkownik pyta “czy zdążę”:

- Egzamin AKSO jest 19 czerwca 2026.
- Aktualny tryb nauki jest dobry.
- Największe ryzyko to zadania obliczeniowe, nie definicje.
- Priorytet: stare egzaminy + zadania z szeregowania, pamięci, stronicowania, i-węzłów.

## Ostatni kierunek nauki

Użytkownik jest teraz w dziale:

```text
06_szeregowanie_procesow.md
```

Ostatnio omawiane:

- RR i kwant czasu,
- narzut przełączania kontekstu,
- SRTF,
- I/O-bound vs CPU-bound,
- niewywłaszczające: dlaczego `aktywny -> gotowy` to wywłaszczenie,
- szeregowanie wielopoziomowe ze sprzężeniem zwrotnym.

Następny dobry krok:

1. Dokończyć teorię szeregowania.
2. Zrobić test z szeregowania.
3. Zrobić 2-3 zadania liczbowe: FCFS/SJF/SRTF/RR.
4. Potem przejść do synchronizacji albo pamięci.

