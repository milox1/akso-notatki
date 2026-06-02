# AKSO - wejście-wyjście, dyski, przerwania i DMA

## Najkrócej

Wejście-wyjście jest wolne w porównaniu z procesorem i pamięcią, dlatego system operacyjny musi dobrze ukrywać szczegóły urządzeń oraz nie marnować CPU na czekanie.

Najważniejsze hasła:

- urządzenia blokowe i znakowe,
- porty I/O i memory-mapped I/O,
- polling,
- przerwania,
- DMA,
- sterowniki urządzeń,
- planowanie dostępu do dysku.

## Urządzenia blokowe i znakowe

| Typ | Sens | Przykład |
|---|---|---|
| blokowe | dane w blokach o stałym rozmiarze | dysk |
| znakowe | strumień bajtów/znaków | klawiatura, terminal |

Urządzenie blokowe zwykle pozwala adresować konkretne bloki. Urządzenie znakowe jest bardziej strumieniowe.

## Dostęp sekwencyjny i swobodny

**Dostęp sekwencyjny**: dane czyta się po kolei, np. taśma.

**Dostęp swobodny**: można przejść do wybranego bloku, np. dysk.

## Komunikacja ze sterownikiem

Sterownik urządzenia udostępnia rejestry, np.:

- rejestr danych,
- rejestr stanu,
- rejestr poleceń.

CPU może komunikować się z nimi przez:

- specjalne porty I/O,
- odwzorowanie rejestrów urządzenia w pamięć, czyli memory-mapped I/O.

## Porty I/O

Przy komunikacji przez porty procesor używa specjalnych instrukcji wejścia-wyjścia do numerowanych portów.

To nie jest zwykły odczyt z RAM, tylko osobna przestrzeń I/O.

## Memory-mapped I/O

Rejestry urządzenia są odwzorowane w przestrzeń adresową.

Procesor komunikuje się z urządzeniem przez zwykłe instrukcje odczytu/zapisu pamięci, ale adres fizycznie prowadzi do rejestru urządzenia, nie do RAM.

Pułapka:

> Memory-mapped I/O nie oznacza, że dane urządzenia są zwykłą pamięcią operacyjną.

## Polling

**Polling**, czyli odpytywanie, polega na tym, że procesor regularnie sprawdza, czy urządzenie jest gotowe.

Zalety:

- proste,
- dobre dla szybkich urządzeń albo krótkiego czekania.

Wady:

- może marnować czas CPU,
- CPU aktywnie czeka zamiast wykonywać inne procesy.

## Przerwania

Przerwania pozwalają urządzeniu zgłosić procesorowi, że coś się stało.

Przykład:

1. Proces zleca operację I/O.
2. Proces może zostać wstrzymany.
3. Urządzenie pracuje.
4. Po zakończeniu sterownik zgłasza przerwanie.
5. System obsługuje przerwanie i może przenieść proces do gotowych.

Przerwania zmniejszają potrzebę ciągłego odpytywania.

## DMA

**DMA** to Direct Memory Access, czyli bezpośredni dostęp do pamięci.

Kontroler DMA przesyła większy blok danych między urządzeniem a pamięcią bez kopiowania każdego bajtu przez CPU.

CPU zwykle:

1. ustawia parametry transferu,
2. uruchamia DMA,
3. robi inną pracę,
4. dostaje przerwanie po zakończeniu transferu.

Pułapka:

> DMA nie oznacza, że CPU w każdym takcie sprawdza koniec transmisji. Koniec zwykle zgłasza przerwanie.

## Warstwy systemu I/O

Typowe warstwy:

- oprogramowanie użytkownika,
- warstwa systemu niezależna od urządzenia,
- sterowniki urządzeń,
- procedury obsługi przerwań,
- sprzęt.

Sterownik urządzenia zna konkretny sprzęt, a warstwa niezależna daje jednolity interfejs.

## Dyski

Dysk jest zwykle widziany jako tablica bloków logicznych.

Ważne czasy:

- czas szukania ścieżki,
- opóźnienie obrotowe,
- czas transferu.

Nowoczesny system często nie zna dokładnego fizycznego położenia bloków, bo dysk sam mapuje bloki logiczne.

## Planowanie dostępu do dysku

| Strategia | Sens |
|---|---|
| FCFS | obsługa w kolejności zgłoszeń |
| SSTF | najpierw żądanie wymagające najkrótszego ruchu głowicy |
| SCAN | głowica idzie w jedną stronę jak winda, potem zawraca |
| C-SCAN | obsługa w jednym kierunku, potem szybki powrót |
| LOOK | jak SCAN, ale tylko do skrajnego żądania |
| C-LOOK | jak C-SCAN, ale tylko do skrajnego żądania |

SSTF może powodować zagłodzenie odległych żądań.

## Pułapki egzaminacyjne

- Polling nie wymaga kontrolera przerwań, ale może marnować CPU.
- Przerwania pozwalają CPU robić coś innego do czasu zdarzenia.
- DMA jest dobre dla dużych transferów.
- Koniec DMA zwykle sygnalizuje przerwanie.
- Urządzenie blokowe nie jest tym samym co znakowe.
- Memory-mapped I/O używa adresów, ale nie jest zwykłym RAM.
- SSTF może głodzić dalekie żądania.

## Pytania T/N z odpowiedziami

1. Odpytywanie polega na sprawdzaniu stanu urządzenia przez procesor.  
   **T** - CPU sprawdza bit gotowości/status.

2. Odpytywanie zawsze wymaga wsparcia kontrolera przerwań.  
   **N** - polling może działać bez przerwań.

3. Przerwania pozwalają uniknąć ciągłego sprawdzania urządzenia przez CPU.  
   **T** - urządzenie samo zgłasza zdarzenie.

4. DMA służy do przesyłania danych między urządzeniem a pamięcią z mniejszym udziałem CPU.  
   **T** - CPU ustawia transfer, ale nie kopiuje każdego bajtu.

5. Koniec transmisji DMA jest zwykle sygnalizowany przerwaniem.  
   **T** - to klasyczny mechanizm.

6. W DMA procesor musi w każdym takcie sprawdzać, czy transmisja się zakończyła.  
   **N** - od tego jest przerwanie albo status sprawdzany później.

7. Urządzenie blokowe przechowuje dane w blokach o ustalonym rozmiarze.  
   **T** - typowy przykład to dysk.

8. SSTF zawsze gwarantuje brak zagłodzenia.  
   **N** - może faworyzować bliskie żądania.

9. SCAN jest znany jako algorytm windy.  
   **T** - głowica porusza się w jednym kierunku i potem zawraca.

10. Memory-mapped I/O oznacza, że rejestry urządzeń są widoczne pod adresami.  
    **T** - dostęp wygląda jak odczyt/zapis pod adres.

