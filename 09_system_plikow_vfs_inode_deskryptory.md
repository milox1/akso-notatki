# AKSO - system plików, i-węzły, VFS i deskryptory

## Najkrócej

System plików daje użytkownikowi logiczny obraz danych jako plików i katalogów.

Najważniejsze rozróżnienie:

- **i-węzeł** opisuje plik,
- **katalog** mapuje nazwę na i-węzeł,
- **deskryptor pliku** jest lokalny dla procesu,
- **tablica otwartych plików** przechowuje stan otwartego pliku, np. pozycję.

## Plik

Plik to logiczny pojemnik na dane.

System plików udostępnia operacje:

- tworzenie,
- usuwanie,
- otwieranie,
- zamykanie,
- odczyt,
- zapis,
- zmiana pozycji,
- zmiana atrybutów.

## Katalog

Katalog organizuje przestrzeń nazw.

W Uniksie katalog zawiera powiązania:

> nazwa -> i-węzeł

Pułapka:

> Nazwa pliku zwykle nie jest przechowywana w i-węźle, tylko w katalogu.

## Dowiązania

**Dowiązanie twarde** to dodatkowa nazwa wskazująca na ten sam i-węzeł.

**Dowiązanie symboliczne** to specjalny plik przechowujący ścieżkę do innego pliku.

Różnice:

| Cecha | Hard link | Symlink |
|---|---|---|
| Wskazuje na | i-węzeł | ścieżkę |
| Może wisieć po usunięciu celu | nie w typowym sensie | tak |
| Ma własny i-węzeł | nie jako nowy plik danych | tak |

## I-węzeł

I-węzeł przechowuje metadane pliku, np.:

- typ pliku,
- prawa dostępu,
- właściciela,
- rozmiar,
- czasy,
- liczbę dowiązań,
- informacje o rozmieszczeniu bloków danych.

I-węzeł zwykle nie przechowuje:

- nazwy pliku,
- pozycji aktualnego odczytu/zapisu procesu,
- numeru deskryptora w procesie.

## Deskryptor pliku

Deskryptor pliku to mała liczba w procesie, używana przez `read`, `write`, `close`.

Każdy proces ma własną tablicę deskryptorów.

Deskryptor wskazuje na systemową strukturę otwartego pliku.

## Systemowa tablica otwartych plików

Obiekt otwartego pliku przechowuje między innymi:

- aktualną pozycję w pliku,
- tryb otwarcia,
- wskaźnik do v-węzła/i-węzła.

Pułapka:

> Pozycja w pliku nie jest w i-węźle na dysku. Jest w strukturze otwartego pliku.

## `open`, `fork`, `dup`

`open` tworzy nowy wpis otwartego pliku i nowy deskryptor w procesie.

`dup` i `dup2` tworzą nowy deskryptor wskazujący na ten sam obiekt otwartego pliku, więc deskryptory współdzielą pozycję.

Po `fork` proces potomny dziedziczy deskryptory. Często wskazują one na te same obiekty otwartych plików, więc pozycja może być współdzielona.

## VFS i v-węzły

**VFS** to warstwa wirtualnego systemu plików.

Pozwala różnym systemom plików mieć wspólny interfejs.

**V-węzeł** jest abstrakcyjną reprezentacją obiektu plikowego w jądrze. Może reprezentować plik z różnych systemów plików.

Pułapka:

> VFS nie jest konkretnym systemem plików typu ext2. To warstwa abstrakcji.

## Montowanie

Montowanie dołącza system plików do istniejącego drzewa katalogów.

W Uniksie jest jedno logiczne drzewo katalogów z jednym katalogiem głównym `/`, nawet jeśli składa się z wielu zamontowanych systemów plików.

## Pliki specjalne

W systemie Unix wiele obiektów jest widocznych przez interfejs plików.

Przykłady:

- urządzenia,
- kolejki FIFO,
- dowiązania symboliczne,
- `/proc`.

## Pułapki egzaminacyjne

- I-węzeł nie przechowuje nazwy pliku.
- Katalog mapuje nazwy na i-węzły.
- Deskryptor jest lokalny dla procesu.
- Pozycja w pliku jest w strukturze otwartego pliku, nie w i-węźle.
- `dup` współdzieli obiekt otwartego pliku.
- VFS to warstwa abstrakcji, nie konkretny system plików.
- W Uniksie przestrzeń nazw plików tworzy jedno drzewo.
- Jest jeden systemowy katalog główny `/`.

## Pytania T/N z odpowiedziami

1. I-węzeł zawiera informacje o rozmieszczeniu bloków pliku na dysku.  
   **T** - to jedna z jego kluczowych ról.

2. I-węzeł na dysku zwykle przechowuje nazwę pliku.  
   **N** - nazwa jest w katalogu.

3. I-węzeł przechowuje aktualną pozycję odczytu procesu w pliku.  
   **N** - pozycja jest w obiekcie otwartego pliku.

4. Deskryptor pliku jest obiektem związanym z procesem.  
   **T** - każdy proces ma własną tablicę deskryptorów.

5. `dup` tworzy deskryptor wskazujący na ten sam otwarty plik.  
   **T** - dlatego współdzielona jest pozycja.

6. Katalog mapuje nazwy na i-węzły.  
   **T** - to podstawowa idea katalogu.

7. Dowiązanie symboliczne przechowuje ścieżkę.  
   **T** - dlatego może wskazywać na nieistniejący cel.

8. VFS jest konkretnym systemem plików MS-DOS.  
   **N** - VFS to warstwa abstrakcji systemów plików.

9. W Uniksie przestrzeń nazw plików tworzy drzewo.  
   **T** - z jednym katalogiem głównym.

10. Po wielokrotnym `open` tego samego pliku powstają niezależne obiekty otwartego pliku.  
    **T** - zwykle mają niezależne pozycje.

