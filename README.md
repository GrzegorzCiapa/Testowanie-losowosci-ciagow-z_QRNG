[![Open In Colab]([https://colab.research.google.com](https://colab.research.google.com/drive/1NBR7hV4P1m6H3ycH170iI0GABIvdsFzi)/assets/colab-badge.svg)](https://colab.research.google.com/)


# Testowanie losowości ciągów generowanych przez QRNG

**Autor:** Grzegorz Ciapa  
**Technologie:** Python 3, C, ENT, TestU01, Matplotlib, Jupyter Notebook / Google Colab  

## Cel Projektu
Celem projektu jest zapoznanie się z działaniem baterii testów statystycznych losowości oraz ich zastosowanie do analizy ciągów pochodzących z kwantowych generatorów liczb losowych (QRNG). Projekt bada również bezpieczeństwo i wiarygodność generatorów poprzez analizę przygotowanych ciągów częściowo deterministycznych. Weryfikacja jest kluczowa, ponieważ generator kwantowy nie musi być automatycznie godny zaufania.

## Metodyka
Projekt został zrealizowany poprzez wieloetapowe testowanie plików binarnych przy użyciu zróżnicowanych narzędzi statystycznych:

1. **Ewaluacja ciągów referencyjnych z QRNG:**
   * Wykorzystano surowe dane binarne pozyskane z kwantowych generatorów (m.in. platforma ANU QRNG).
   * Poddano je szybkiej analizie przy użyciu programu ENT oraz zaawansowanych baterii statystycznych.

2. **Implementacja i testowanie ciągów częściowo deterministycznych:**
   * Spreparowano specjalne ciągi posiadające ukrytą strukturę deterministyczną.
   * Zastosowano mechanizm korelacji typu XOR na zadanych odległościach, celowo projektując pliki tak, aby były niewykrywalne dla standardowych ustawień wybranych baterii testów.
   * Poprzez stopniowe zmniejszanie zasięgu korelacji XOR, dokonano określenia progu wykrywalności deterministycznej struktury przez narzędzia testujące.

3. **Autorski test częstości wzorców (Zadanie rozszerzone \*):**
   * Zaprojektowano i zaimplementowano własny test sprawdzający odchylenie częstości występowania wszystkich możliwych wzorców bitowych o długości $n$ od teoretycznej granicy.
   * Przeprowadzono wykonanie testów przy zwiększonych parametrach dokładności oraz pomiar czasu ich wykonania.

4. **Integracja z zaawansowaną baterią TestU01 (Zadanie rozszerzone \*):**
   * Zaimplementowano w języku C specjalny interfejs czytający plik bit po bicie (`FileBitGen_GetU01`), w pełni zgodny ze specyfikacją biblioteki TestU01.
   * Uruchomiono rygorystyczne baterie testowe (`BigCrush`) w celu głębokiej weryfikacji plików z QRNG.

## Wykorzystane Metryki
* **Czas egzekucji testów:** Dokładna analiza możliwości modyfikowania parametrów testów (np. zwiększanie liczby próbek) i ich wpływu na czas weryfikacji.
* **Entropia informacyjna:** Wykorzystywana w programie ENT do określenia kompresowalności i zagęszczenia informacji w pojedynczym bajcie.
* **Współczynniki P-value:** Weryfikacja statystyczna odchyleń od wartości oczekiwanej generowana przez baterię TestU01.

## Kluczowe Wnioski
* **Złudzenie losowości (Ciągi częściowo deterministyczne):** Eksperymenty potwierdziły możliwość przejścia testów losowości przez ciągi częściowo deterministyczne (spreparowane funkcją XOR). Udowadnia to, że "zaliczenie" testu statystycznego oznacza jedynie brak konkretnych, badanych algorytmicznie wzorców, a nie idealną losowość kryptograficzną.
* **Wpływ badanych wzorców na złożoność:** Wykazano eksponencjalny wzrost czasu realizacji testów przy zwiększaniu parametrów dokładności (np. długości wzorców $n$). Istnieje wyraźna bariera obliczeniowa w weryfikacji bardzo długich zależności w plikach binarnych.
* **Progi wykrywalności korelacji:** Wskazano minimalny zasięg korelacji, poniżej którego testy statystyczne jednoznacznie odrzucają hipotezę o losowości pliku wejściowego.

## Zawartość Repozytorium
* `Testowanie_QRNG_GC.ipynb` — Główny notatnik Google Colab zawierający zintegrowane skrypty w języku Python odpowiadające za przygotowanie ciągów częściowo deterministycznych, pobieranie danych oraz rysowanie wykresów złożoności.
* `testu01_wrapper.c` — Pomocniczy kod w języku C implementujący strukturę `FileBitGen` oraz interfejs dla baterii `bbattery_BigCrush`.
* `Raport_Zadanie_2.pdf` — Oficjalny raport z zadania zawierający charakterystykę wybranej baterii testów, opis instalacji, interpretację wyników, wnioski oraz odpowiedzi na pytania teoretyczne (m.in. pojęcie losowości oraz liczb normalnych).
