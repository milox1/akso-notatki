# AKSO - bity, liczby, x86 i asembler

## Najkrócej

Ten dział jest bardziej architekturą niż systemami operacyjnymi, ale daje słownictwo potrzebne do rozumienia procesora.

Najważniejsze:

- **bit** to 0 albo 1,
- **bajt** ma zwykle 8 bitów,
- **oktet** zawsze ma 8 bitów,
- **słowo** zależy od architektury,
- kod maszynowy jest binarny i zależny od architektury,
- asembler jest tekstowym zapisem rozkazów maszynowych,
- x86 jest architekturą typu CISC,
- x86-64 używa little endian.

## Kod maszynowy i asembler

**Kod maszynowy** to binarny zapis instrukcji procesora. Jest bezpośrednio wykonywany przez CPU.

**Asembler** to czytelniejszy dla człowieka zapis kodu maszynowego.

Pułapka:

> Asembler nie jest uniwersalny. Każda architektura ma własny zestaw instrukcji i własny kod maszynowy.

## W asemblerze nie ma typów

Procesor widzi bity. To programista, kompilator albo instrukcja decyduje, jak je interpretować.

Ta sama sekwencja bitów może być:

- liczbą bez znaku,
- liczbą ze znakiem,
- znakiem,
- adresem,
- częścią instrukcji.

Egzaminowo: typ danych nie siedzi magicznie w pamięci. Interpretacja zależy od kontekstu.

## NKB i U2

**NKB** to naturalny kod binarny, czyli zapis liczby bez znaku.

**U2** to kod uzupełnień do dwóch, najczęstszy zapis liczb całkowitych ze znakiem.

W U2:

- najstarszy bit pełni rolę bitu znaku,
- `0` na początku zwykle oznacza liczbę nieujemną,
- `1` na początku zwykle oznacza liczbę ujemną,
- dodawanie działa sprzętowo podobnie jak dla liczb bez znaku.

## Przeniesienie i nadmiar

**Przeniesienie** dotyczy zwykle interpretacji bez znaku.

**Nadmiar** dotyczy interpretacji ze znakiem w U2.

Pułapka:

> Ten sam wynik bitowy może mieć przeniesienie w NKB i/lub nadmiar w U2, zależnie od interpretacji.

## Rejestr stanu

Rejestr stanu, np. FLAGS/EFLAGS/RFLAGS w x86, przechowuje znaczniki ustawiane po operacjach arytmetyczno-logicznych.

Typowe znaczniki:

| Znacznik | Sens |
|---|---|
| ZF | wynik jest zerem |
| SF | bit znaku wyniku |
| CF | przeniesienie/pożyczka, ważne dla NKB |
| OF | nadmiar, ważny dla U2 |

Skoki warunkowe często sprawdzają właśnie te znaczniki.

## Little endian

x86-64 jest architekturą **little endian**.

Oznacza to, że mniej znaczący bajt liczby jest zapisany pod niższym adresem.

Pułapka:

> Endianowość dotyczy kolejności bajtów w pamięci, nie kolejności bitów w bajcie.

## Tryby adresowania

Tryb adresowania mówi, skąd instrukcja bierze argument.

Typowe tryby:

| Tryb | Sens |
|---|---|
| natychmiastowy | wartość jest zapisana w instrukcji |
| rejestrowy | argument jest w rejestrze |
| bezpośredni pamięciowy | argument jest w pamięci pod adresem |
| indeksowy/bazowy | adres jest liczony z rejestrów i przesunięcia |

Tryby adresowania są elementem **architektury**, bo wpływają na sposób pisania i generowania programu.

## Stos

W x86 stos zwykle rośnie w dół, czyli w stronę malejących adresów.

Na stosie mogą znajdować się:

- adresy powrotu,
- zmienne lokalne,
- zachowane rejestry,
- czasem argumenty funkcji.

W x86-64 w typowym ABI linuksowym pierwsze argumenty całkowitoliczbowe funkcji są przekazywane w rejestrach, a nie od razu na stosie.

## ABI

**ABI** to Application Binary Interface.

Określa reguły zgodności na poziomie binarnym, np.:

- które rejestry przekazują argumenty,
- które rejestry funkcja musi zachować,
- jak wygląda stos,
- jak zwracany jest wynik.

ABI to nie to samo co API. API dotyczy interfejsu programistycznego, ABI dotyczy zgodności binarnej.

## Pułapki egzaminacyjne

- Bajt zwykle ma 8 bitów, ale oktet zawsze ma 8 bitów.
- Asembler nie jest przenośny między architekturami.
- W pamięci są bity; typ wynika z interpretacji.
- CF i OF nie znaczą tego samego.
- Little endian dotyczy kolejności bajtów w pamięci.
- Tryby adresowania to architektura, nie organizacja.
- Stos w x86 rośnie w dół.

## Pytania T/N z odpowiedziami

1. Oktet zawsze ma 8 bitów.  
   **T** - to definicja oktetu.

2. Kod maszynowy jest taki sam dla każdej architektury.  
   **N** - każda architektura ma własny kod maszynowy.

3. Asembler jest tekstowym zapisem instrukcji maszynowych.  
   **T** - ułatwia czytanie kodu maszynowego przez człowieka.

4. W pamięci liczba ma zawsze jedno ustalone znaczenie typu int albo char.  
   **N** - pamięć przechowuje bity, a znaczenie zależy od interpretacji.

5. CF jest szczególnie ważny przy interpretacji bez znaku.  
   **T** - przeniesienie dotyczy arytmetyki NKB.

6. OF jest szczególnie ważny przy interpretacji ze znakiem w U2.  
   **T** - mówi o nadmiarze dla liczb ze znakiem.

7. x86-64 jest typowo little endian.  
   **T** - mniej znaczący bajt jest pod niższym adresem.

8. Tryby adresowania są elementem organizacji, bo zależą tylko od sprzętu wewnętrznego.  
   **N** - są widoczne dla programisty/kompilatora, więc należą do architektury.

9. Stos w x86 zwykle rośnie w stronę rosnących adresów.  
   **N** - zwykle rośnie w dół.

10. ABI określa między innymi sposób przekazywania argumentów funkcji.  
    **T** - to część zgodności binarnej.

