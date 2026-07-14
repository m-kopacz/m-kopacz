# Projekty

---

W tej sekcji przedstawiam skondensowane podsumowanie zrealizowanych przeze mnie projektów analitycznych i programistycznych. Pełna dokumentacja oraz kod źródłowy znajdują się w dedykowanych repozytoriach.

* [Głębokie uczenie ze wzmocnieniem w optymalizacji obrotu akcjami spółek WIG20](https://github.com/m-kopacz/DRL_handel_algorytmiczny_WIG20)
* [Przewidywanie i Analiza Ryzyka Niewypłacalności Kart Kredytowych](https://github.com/m-kopacz/Predykcja_defaultu)
* [Klasyfikacja Dochodów (UCI Adult Census Income)](https://github.com/m-kopacz/Predykcja_dochodow)
* [Ekonometria panelowa w analizie determinant stopy inwestycji](https://github.com/m-kopacz/Ekonometria_panelowa_determinanty_inwestycji)
* [Analiza Przeżycia Pacjentek z Rakiem Piersi](https://github.com/m-kopacz/Analiza_przezycia_pacjentek_z_rakiem_piersi)
* [Porównanie klasycznego i kwantowego SVM w klasyfikacji](https://github.com/m-kopacz/qSVM_klasyfikacja)
* [Analiza Koszykowa w R](https://github.com/m-kopacz/Analiza_asocjacji_zakupy/blob/main/README.md)
* [Segmentacja Klientów Galerii Handlowej za pomocą Algorytmu K-Średnich (K-Means)](https://github.com/m-kopacz/Klasteryzacja_klientow_galerii_handlowej)
* [Optymalizacja trasy pracownika InPost metodą Tabu Search](https://github.com/m-kopacz/Optymalizacja_trasy_kuriera)

---

### [Głębokie uczenie ze wzmocnieniem w optymalizacji obrotu akcjami spółek WIG20](https://github.com/m-kopacz/DRL_handel_algorytmiczny_WIG20)

**🎯 Cel projektu:** Praca magisterska (SGH, Big Data) badająca efektywność algorytmów Deep Reinforcement Learning (DRL) z rodziny Aktor-Krytyk w automatycznym alokowaniu kapitału na rynku WIG20 w warunkach ekstremalnej zmienności (krach COVID-19 w 2020 r.).

**🛠️ Architektura i metodyka:**

* **Algorytmy i Walidacja:** Implementacja 5 modeli dla ciągłych przestrzeni akcji (**PPO**, **A2C**, **DDPG**, **TD3**, **SAC**). Zastosowano rygorystyczną walidację kroczącą (*Walk-Forward*) na 25 kwartałach, eliminując wyciek danych (*data leakage*).
* **Redukcja Wymiarowości:** Kaskadowy potok redukujący klątwę wymiarowości ze 110 do 14 zmiennych: transformacja Z-Score -> korelacja Spearmana -> klasteryzacja hierarchiczna -> *Mutual Information* -> selekcja na bazie **SHAP** z modeli XGBoost.
* **Urealnione Środowisko:** Implementacja prowizji maklerskich (0.10%), opóźnień egzekucji (sygnał w dniu $t$, zakup po cenie otwarcia w dniu $t+1$), handlu ułamkowego oraz logarytmicznej stopy zwrotu jako funkcji nagrody.
* **Optymalizacja:** Bayesowskie strojenie hiperparametrów (**Optuna TPE**) pod kątem mediany wskaźnika Sortino oraz budowa wieloagentowych komitetów decyzyjnych (*Ensemble Learning* do $N=50$).

**🏆 Kluczowe wyniki:**

| Strategia / Model | Skumulowana Stopa Zwrotu | Wskaźnik Sortino | Max Drawdown |
| --- | --- | --- | --- |
| **Buy & Hold (Rynek WIG20)** | **-8.87%** | - | **-44.86%** |
| **Portfel Markowitza** | **-13.57%** | - | - |
| **DRL Komitet WIN ($N=50$)** | **+86.00%** | **2.10** | **Znacznie zredukowany** |

* Aż **99.6% komitetów DRL** pokonało rynek, wykazując się selektywną i agresywną alokacją kapitału w najbardziej perspektywiczne spółki podczas fazy odbicia po krachu.

---

### [💳 Przewidywanie i Analiza Ryzyka Niewypłacalności Kart Kredytowych](https://github.com/m-kopacz/Predykcja_defaultu)

**🎯 Cel projektu:** Budowa modelu oceny ryzyka kredytowego (30 000 klientów z Tajwanu) z bezpośrednim podporządkowaniem procesu modelowania **asymetrycznej macierzy kosztów biznesowych w stosunku 5:1**, gdzie błąd *False Negative* (udzielenie limitu dłużnikowi) kosztuje bank 5-krotnie więcej niż odrzucenie rzetelnego klienta (*False Positive*).

**🛠️ Architektura i metodyka:**

* **Zaawansowany Preprocessing:** Inżynieria cech behawioralnych (np. `AVG_PAY_DELAY`, `UTILIZATION_RATIO`), obsługa wartości odstających (winsoryzacja), stratyfikowany podział zbiorów oraz skalowanie `RobustScaler`.
* **Modelowanie i XAI:** Porównanie modeli **XGBoost**, Regresja Logistyczna oraz Drzewa Decyzyjne z przycinaniem (*Cost Complexity Pruning*). Selekcja kluczowych cech metodą **Permutation Importance**.
* **Threshold Tuning:** Kalibracja progu odcięcia predykcji indywidualnie dla każdego algorytmu (Out-of-Fold CV) pod kątem bezpośredniej minimalizacji oczekiwanych strat finansowych banku.

**🏆 Kluczowe wyniki:**

* **Rekomendacja produkcyjna (XGBoost):** Osiągnął najniższy średni koszt ryzyka na klienta równy **0.556** (spadek strat o **41%** względem klasyfikatora losowego) oraz czułość **Recall = 78%** na zbiorze testowym przy braku oznak przeuczenia.
* **Alternatywa *White-Box*:** Zoptymalizowane Drzewo Decyzyjne dostarczyło w pełni transparentne reguły *if-then* dla nadzoru bankowego przy zachowaniu wysokiej skuteczności (koszt: **0.579**, ROC AUC: **0.759**).

---

### [👥 Klasyfikacja Dochodów (UCI Adult Census Income)](https://github.com/m-kopacz/Predykcja_dochodow)

**🎯 Cel projektu:** Klasyfikacja binarna przewidująca roczny dochód powyżej 50 000 USD na podstawie cech demograficzno-społecznych (*UCI Adult Dataset*, 48 842 obserwacje) w warunkach znacznego niezbalansowania klas (tylko 23.9% populacji zarabia >50k USD) oraz obecności braków danych.

**🛠️ Architektura i metodyka:**

* **Zaawansowany Preprocessing:** Obsługa nielosowych braków danych (MNAR -> testy Chi-kwadrat i wyodrębnienie kategorii "Missing"), winsoryzacja wartości odstających (1. i 99. percentyl), usunięcie redundantnej zmiennej `fnlwgt` oraz hybrydowe kodowanie zmiennych (*Target Encoding* z wygładzaniem vs *One-Hot Encoding*).
* **Segmentacja Nienadzorowana:** Redukcja wymiarowości (PCA) i klasteryzacja KMeans – wyodrębnienie 3 naturalnych segmentów społecznych, w tym „elity dochodowej”, w której 100% badanych osiągało dochody >50k USD.
* **Modelowanie i Threshold Tuning:** Ewaluacja 5 klasyfikatorów (Drzewo Decyzyjne z *Minimal Cost-Complexity Pruning*, XGBoost, Random Forest, Regresja Logistyczna, SVM-RBF). Kalibracja progów decyzyjnych na podstawie krzywej *Precision-Recall* pod kątem maksymalizacji wskaźnika **F1-Score**.

**🏆 Kluczowe wyniki:**

| Model | Optymalny Próg | F1-Score (Test) | Accuracy (Test) | AUC (Test) |
| --- | --- | --- | --- | --- |
| **XGBoost** | 0.5000 | **0.7290** | **86.62%** | **0.9254** |
| **Random Forest** | 0.6093 (tuned) | **0.7131** | 85.40% | 0.9145 |
| **Drzewo Decyzyjne** | 0.3432 (tuned) | **0.6962** | 84.89% | 0.9059 |

* **Lider predykcji (XGBoost):** Bezkonkurencyjnie poradził sobie z nieliniowymi interakcjami w danych, osiągając najwyższe wskaźniki we wszystkich badanych metrykach.
* **Efekt kalibracji progów:** Obniżenie progu odcięcia w modelach klasycznych (np. do 0.18 dla SVM i 0.32 dla Regresji Logistycznej) skompensowało nierównowagę klas, pozwalając na skokowy wzrost czułości bez utraty precyzji.

---

### [🇪🇺 Ekonometria panelowa w analizie determinant stopy inwestycji](https://github.com/m-kopacz/Ekonometria_panelowa_determinanty_inwestycji)

**🎯 Cel projektu:** Praca licencjacka (SGH) mająca na celu ilościową identyfikację makroekonomicznych i demograficznych determinant stopy inwestycji w 20 krajach Europy w latach 1995–2021 (502 obserwacje panelowe).

**🛠️ Architektura i metodyka:**

* **Specyfikacja Modelu:** Badanie stacjonarności testami Im-Pesaran-Shin (IPS). Na podstawie testów Walda i Hausmana wyselekcjonowano model z efektami stałymi (**Fixed Effects - Within**).
* **Zaawansowana Diagnostyka:** Wykryto silną autokorelację 1. rzędu (test Breuscha-Godfreya), heteroskedastyczność (test Breuscha-Pagana) oraz zależność przekrojową (test Peserana CD).
* **Korekta Statystyczna:** Zastosowano odporną macierz wariancji-kowariancji **HC3**, uodparniając estymacje na zdiagnozowane zaburzenia składnika losowego.

**🏆 Kluczowe wnioski ekonomiczne i prognostyczne:**

* **Potwierdzenie teorii wzrostu:** Wykazano silny, pozytywny wpływ wzrostu PKB (**teoria akceleratora**), wydatków na R&D i patentów (**teoria endogenicznego wzrostu**) oraz występowanie efektu przyciągania prywatnego kapitału przez deficyt budżetowy (*crowding-in*).
* **Czynniki hamujące:** Bezrobocie, wysoka produktywność (efektywniejsze wykorzystanie obecnego kapitału) oraz obciążenie demograficzne statystycznie istotnie ograniczają stopę inwestycji.
* **Trafność ex-post:** W symulacji prognoz na latach 2017–2021 model osiągnął wysokie wskaźniki dokładności (**MAPE = 12.59%**, **RMSE = 2.96%**, **MAE = 2.49%**).

---

### [🎗️ Analiza Przeżycia Pacjentek z Rakiem Piersi](https://github.com/m-kopacz/Analiza_przezycia_pacjentek_z_rakiem_piersi)

**🎯 Cel projektu:** Wszechstronne badanie statystyczne w systemie **SAS** klinicznych i demograficznych czynników determinujących czas przeżycia 4024 pacjentek z inwazyjnym rakiem piersi (baza programu SEER, 10 lat obserwacji).

**🛠️ Architektura i metodyka:**

* **Podejście Nieparametryczne:** Tabele trwania życia (wykrycie dwuszczytowego ryzyka zgonu wokół 50. i 90. miesiąca) oraz estymator **Kaplana-Meiera** z testami warstwowymi Log-Rank i Wilcoxona.
* **Podejście Parametryczne i Bayesowskie:** Wybór modelu log-logistycznego (kryterium AIC/BIC) oraz estymacja parametrów metodą **MCMC** (10 000 próbek) z pełną weryfikacją zbieżności łańcuchów (testy Gelmana-Rubina, Geweke).
* **Model Semiparametryczny Coxa:** Selekcja krokowa zmiennych oraz rygorystyczna weryfikacja założenia proporcjonalności hazardów za pomocą **ilościowego testu reszt Schoenfelda** i analizy graficznej reszt martyngałowych.

**🏆 Kluczowe wyniki kliniczne:**

* **Najsilniejszy predyktor:** Wzrost udziału zajętych węzłów chłonnych (`RNPR`) z 0 do 100% zwiększa ryzyko zgonu niemal 5-krotnie (**HR = 4.698**, $p < 0.0001$).
* **Stopień złośliwości:** Niski stopień złośliwości nowotworu (Grade 1 vs Grade 3) obniża ryzyko zgonu aż o **67.7%**.
* **Dynamika receptorów hormonalnych:** Ochronna rola obecności receptorów estrogenowych i progesteronowych słabnie w miarę upływu czasu (naruszenie proporcjonalności hazardu), co klinicznie uzasadnia konieczność wieloletniego monitorowania pacjentek.

---

### [Porównanie klasycznego i kwantowego SVM w klasyfikacji](https://github.com/m-kopacz/qSVM_klasyfikacja)

**🎯 Cel projektu:** Porównanie wydajności klasyfikatorów Support Vector Machine (SVM) w architekturze klasycznej oraz kwantowej (qSVM) w medycznym zadaniu klasyfikacji chorób serca na zbiorze *heart-statlog*.

**🛠️ Tech stack:** `Python`, `PennyLane`, `scikit-learn`, `XGBoost`.

**🏆 Wynik:** Uzyskano niemal identyczną precyzję predykcyjną w obu wariantach modelowania, z marginalną przewagą klasycznego SVM pod względem wskaźnika ROC AUC (**0.90** dla SVM vs **0.89** dla qSVM).

---

### [🛒 Analiza Koszykowa w R](https://www.google.com/search?q=link-do-repozytorium)

**🎯 Cel projektu:** Analiza asocjacyjna na zbiorze danych transakcyjnych `groceries` (9 835 transakcji z okresu jednego miesiąca). Celem było odkrycie ukrytych powiązań zakupowych między produktami i stworzenie na ich podstawie rentownych strategii marketingowych oraz operacyjnych dla sklepu spożywczego.

**🛠️ Architektura i metodyka:**

* **Tech stack:** Analizę zrealizowano w środowisku R przy użyciu biblioteki `arules` do wydobywania reguł asocjacyjnych oraz pakietu `tidyverse` do przetwarzania i wizualizacji danych.
* **Dobór parametrów:** Wykorzystano algorytm Apriori z matematycznie uzasadnionymi progami: minimalne wsparcie na poziomie 0,015 (wzorzec występujący minimum 5 razy dziennie / 150 transakcji miesięcznie), ufność na poziomie 0,5 (klient kupuje produkt z prawej strony reguły w minimum 50% przypadków) oraz minimalna długość reguły równa 2 (wykluczenie reguł jednoelementowych).

**🏆 Kluczowe wyniki i wnioski:**

* **Wygenerowane reguły:** Model Apriori wyłonił dwie dominujące reguły o silnym przyroście (Lift > 2): `{tropical fruit, yogurt} => {whole milk}` (Lift = 2,02; Ufność = 51,7%; Wsparcie = 1,5%) oraz `{other vegetables, yogurt} => {whole milk}` (Lift = 2,01; Ufność = 51,3%; Wsparcie = 2,2%).
* **Interpretacja biznesowa:** Prawdopodobieństwo zakupu mleka pełnego jest ponad dwukrotnie wyższe w obecności jogurtu i owoców tropikalnych lub warzyw. Wskazuje to na komplementarność produktów w śniadaniach, deserach (np. smoothie) czy posiłkach wytrawnych (np. zupy krem i warzywa z dipami).
* **Rekomendacje operacyjne:** Sformułowano konkretne wskazówki biznesowe obejmujące visual merchandising (umieszczenie powiązanych produktów blisko siebie na trasie klienta), tworzenie promocji pakietowych (np. tańsze mleko przy zakupie jogurtu i owoców) oraz content marketing (udostępnianie przepisów kulinarnych przy stoiskach).

---

### [👥 Segmentacja Klientów Galerii Handlowej za pomocą Algorytmu K-Średnich (K-Means)](https://www.google.com/search?q=link-do-repozytorium)

**🎯 Cel projektu:** Segmentacja 200 klientów galerii handlowej (zbiór `mallcustomers`) w języku R na podstawie rocznego dochodu (`Income`) oraz wskaźnika wydatków (`SpendingScore`). Celem było wyodrębnienie spójnych profili zakupowych umożliwiających optymalne dopasowanie strategii marketingowych.

**🛠️ Architektura i metodyka:**

* **Preprocessing:** Oczyszczenie danych z symboli walut i przecinków w zmiennej dochodowej (konwersja do typu liczbowego) oraz normalizacja (skalowanie) zmiennych w celu uniknięcia dominacji jednej cechy.
* **Dobór liczby klastrów:** Zastosowano trzy niezależne metody matematyczne: metodę łokcia, metodę średniego zarysu (Silhouette) oraz statystykę odstępu (Gap Statistic). Wszystkie metody spójnie potwierdziły, że optymalna liczba klastrów wynosi $k=6$.
* **Modelowanie:** Uruchomienie algorytmu K-Means z parametrem `nstart = 25` (25 losowych inicjalizacji centroidów) w celu zapobiegania zatrzymaniu się algorytmu w minimum lokalnym.

**🏆 Kluczowe wyniki i wnioski:**

* **Wyodrębnione profile zakupowe:** Zidentyfikowano 6 spójnych segmentów konsumentów:
* **Klaster 1 i 2 (Kluczowi Klienci - VIP):** Młode osoby (średnio 32-33 lata) o wysokich dochodach i wysokich wydatkach.
* **Klaster 4 (Klasa Średnia):** Najliczniejsza grupa (81 osób, średnio 43 lata) o przeciętnych dochodach i wydatkach.
* **Klaster 5 (Młodzi Trendsetterzy):** Bardzo młodzi klienci (średnio 25 lat) o niskich dochodach, ale bardzo wysokich wydatkach.
* **Klaster 6 (Oszczędni Profesjonaliści):** Osoby o wysokich zarobkach i niskich wydatkach (średnio 41 lat).
* **Klaster 3 (Oszczędni Konserwatyści):** Starsza grupa o niskim budżecie i niskich wydatkach.


* **Rekomendacje marketingowe:** Sformułowano celowane strategie: oferty premium i eventy dla VIP-ów, płatności odroczone (BNPL) i kampanie w social mediach dla Młodych Trendsetterów, akcje lojalnościowe i produkty na lata dla Oszczędnych Profesjonalistów oraz wyprzedaże dla Oszczędnych Konserwatystów.

---

### [🚚 Optymalizacja trasy pracownika InPost metodą Tabu Search](https://www.google.com/search?q=link-do-repozytorium)

**🎯 Cel projektu:** Opracowanie najkrótszej ciągłej trasy bez powtórzeń (wariant zagadnienia komiwojażera - TSP) dla kuriera InPost doręczającego przesyłki z centralnego oddziału w Radomiu do 13 paczkomatów i wracającego do bazy. Optymalizację przeprowadzono w Pythonie za pomocą algorytmu Przeszukiwania tabu (Tabu Search).

**🛠️ Architektura i metodyka:**

* **Kalkulacja dystansu:** Wykorzystano bibliotekę `geopy` (funkcja `distance()`) do obliczania odległości geograficznej po linii prostej (great-circle distance) na podstawie współrzędnych geograficznych punktów.
* **Operatory sąsiedztwa:** Zaimplementowano i porównano trzy warianty modyfikacji trasy: `swap` (losowa zamiana dwóch punktów), `2-opt` (odwrócenie kolejności odwiedzania punktów na wybranym odcinku, eliminujące przecięcia w trasie) oraz `insert` (usunięcie losowego punktu i wstawienie go w nowe miejsce).
* **Eksperymenty i hiperparametry:** Przetestowano 108 konfiguracji modelu, badając wpływ liczby iteracji, długości listy tabu (pamięci krótkoterminowej algorytmu) oraz typu sąsiedztwa.

**🏆 Kluczowe wyniki i wnioski:**

* **Najlepsze rozwiązanie:** Najkrótsza znaleziona trasa wyniosła **194,94 km**. Wynik ten osiągnięto przy użyciu strategii `insert` oraz `2-opt` (dla parametrów: `iteracje=1000`, `dlugosc_tabu=100`).
* **Przewaga metody Insert:** Operator `insert` wykazał najwyższą stabilność i odporność na warunki początkowe, odnajdując optymalną trasę najczęściej (27 razy). Średnia odległość dla metod `insert` (195,76 km) i `2-opt` (195,15 km) była znacząco lepsza niż dla operatora `swap` (222,12 km), który okazał się mało skuteczny w złożonych problemach trasowania.
* **Wpływ pamięci algorytmu:** Dłuższa lista tabu poprawiała jakość rozwiązania dzięki skuteczniejszej ochronie przed zapętlaniem się algorytmu i utknięciem w lokalnym optimum.
* **Kierunki rozwoju:** Zaproponowano rozbudowę modelu o rzeczywisty czas przejazdu z publicznych API, restrykcje drogowe (np. zakazy skrętu, remonty) oraz integrację z systemami GIS i grafami drogowymi.
