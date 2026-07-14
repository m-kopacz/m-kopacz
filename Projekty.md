# Projekty
---

## Spis Treści
1. [📈 Głębokie Uczenie ze Wzmocnieniem (DRL) w Handlu Algorytmicznym na WIG20](#1-głębokie-uczenie-ze-wzmocnieniem-drl-w-handlu-algorytmicznym-na-wig20)
2. [💳 Przewidywanie i Analiza Ryzyka Niewypłacalności Kart Kredytowych](#2-przewidywanie-i-analiza-ryzyka-niewypłacalności-kart-kredytowych)
3. [👥 Klasyfikacja Dochodów Demograficznych (UCI Adult Census Income)](#3-klasyfikacja-dochodów-demograficznych-uci-adult-census-income)
4. [🇪🇺 Analiza Ekonometryczna Determinant Stopy Inwestycji w Europie](#4-analiza-ekonometryczna-determinant-stopy-inwestycji-w-europie)
5. [🎗️ Kompleksowa Analiza Przeżycia Pacjentek z Rakiem Piersi w SAS](#5-kompleksowa-analiza-przeżycia-pacjentek-z-rakiem-piersi-w-sas)

---

### [Głębokie uczenie ze wzmocnieniem w optymalizacji obrotu akcjami spółek WIG20](https://github.com/m-kopacz/DRL_handel_algorytmiczny_WIG20)

### 📌 Opis Projektu & Rozwiązywany Problem
Projekt zrealizowany w ramach pracy magisterskiej w Szkole Głównej Handlowej w Warszawie (SGH) na kierunku Analiza Danych - Big Data. Bada on efektywność zaawansowanych algorytmów głębokiego uczenia ze wzmocnieniem (DRL) z rodziny Aktor-Krytyk w automatycznym zarządzaniu portfelem akcji na polskim rynku giełdowym (indeks WIG20) w warunkach bezprecedensowej zmienności roku 2020 (pandemia COVID-19). Głównym celem była weryfikacja zdolności adaptacyjnej agentów do drastycznych zmian faz rynkowych (hossy, krachu, trendów bocznych).

### 🛠️ Istotne Metody i Przebieg Procesu
* **Porównywane Algorytmy:** Zaimplementowano 5 modeli obsługujących ciągłe przestrzenie akcji: **PPO** (Proximal Policy Optimization), **A2C** (Advantage Actor-Critic), **DDPG** (Deep Deterministic Policy Gradient), **TD3** (Twin Delayed DDPG) oraz **SAC** (Soft Actor-Critic).
* **Walidacja Krocząca (Walk-Forward Validation):** Aby zapobiec wyciekowi informacji z przyszłości (*data leakage*), zastosowano kroczący podział na 25 kwartałów kalendarzowych (23 kwartały – trening, 1 kwartał – walidacja, 1 kwartał – test), przesuwany cyklicznie co 3 miesiące.
* **Potok Preselekcji Cech (Zmniejszenie klątwy wymiarowości ze 110 do 13–14 zmiennych):**
  1. Wygenerowano ponad 110 wskaźników (techniczne, fundamentalne, makroekonomiczne). Dane fundamentalne poddano kroczącej transformacji Z-Score (okno 252 sesji) z przycięciem do przedziału $[-5.5, 5.5]$.
  2. Obliczono wewnątrzgrupową macierz korelacji rangowej Spearmana.
  3. Zastosowano klasteryzację hierarchiczną (*complete linkage*) z odcięciem na poziomie 0.3 (korelacja wewnątrzklastrowa $\ge 0.7$), grupując zmienne w 39–40 klastrów.
  4. Wyłoniono liderów klastrów za pomocą wskaźnika Informacji Wzajemnej (Mutual Information).
  5. Przeprowadzono analizę istotności marginalnej SHAP na regresorze XGBoost, pozostawiając cechy o skumulowanym wpływie predykcyjnym na poziomie 80% (ostatecznie 13–14 zmiennych).
* **Urealnione Środowisko Transakcyjne (Dostosowane do Regulaminu Brokerów, np. XTB):**
  * *Opóźnienie wykonania (Execution Delay):* Egzekucja transakcji po cenie otwarcia dnia $t+1$ na bazie sygnału z cen zamknięcia dnia $t$.
  * *Skalowanie budżetu (Budget Scaling):* Proporcjonalne obniżanie wolumenów zakupowych w przypadku przekroczenia dostępnego salda, zapobiegając faworyzacji pierwszych spółek z pętli.
  * *Handel ułamkowy:* Transakcje z dokładnością do 4 miejsc po przecinku (min. wartość zlecenia to 10 PLN).
  * *Koszty:* Prowizja 0.10% od każdej operacji.
  * *Logarytmiczna funkcja nagrody:* Zastąpienie bezwzględnej zmiany wyceny logarytmiczną stopą zwrotu pomnożoną przez parametr 100 w celu uregulowania wariancji gradientów.
* **Strojenie Bayesowskie (Optuna & TPE):** Optymalizacja na 3 niezależnych ziarnach losowości równolegle. Funkcja celu opierała się na medianie wskaźnika Sortino pomniejszonej o jego odchylenie standardowe (premiowanie modeli o wysokiej powtarzalności i niskiej wariancji).
* **Uczenie Zespołowe (Ensemble Learning):** Budowa komitetów wieloagentowych (warianty WIN, AVG, SMOOTHED) badanych metodą bootstrappingu dla rozmiarów $N \in [1, 50]$.

### 🏆 Wyniki i Wnioski
* **Porażka strategii klasycznych w 2020 roku:** Portfel pasywny *Buy&Hold* odnotował stratę **-8.87%** (Max Drawdown = **-44.86%**), a portfel minimalnej wariancji Markowitza stracił **-13.57%**.
* **Znakomita skuteczność komitetów DRL:** Wdrożenie uśrednionych sygnałów wyeliminowało szum stochastyczny. Przy liczebności komitetu $N=50$, aż **99.6% komitetów pokonało rynek (Buy&Hold)**, a **97.6% wypracowało dodatni wynik finansowy**.
* **Zwycięski komitet (WIN):** Łączący modele **A2C, PPO i TD3** przy $N=50$ wypracował medianę skumulowanej stopy zwrotu na poziomie **86.00%** oraz wskaźnik Sortino równy **2.10**.
* **Zachowanie w kryzysie:** Agenci wykazali opóźnienie w ucieczce do gotówki podczas gwałtownego krachu z marca 2020 r. (partycypacja w obsunięciach rzędu 32–61%). Jednak w fazie odbicia wykazali się genialną, selektywną alokacją środków w najbardziej perspektywiczne spółki, odrabiając straty z potężną nawiązką.

---

### [💳 Przewidywanie i Analiza Ryzyka Niewypłacalności Kart Kredytowych](https://github.com/m-kopacz/Predykcja_defaultu)

### 📌 Opis Projektu & Rozwiązywany Problem
Celem projektu było zbudowanie modelu klasyfikacji binarnej prognozującego prawdopodobieństwo niewypłacalności (*default*) klientów kart kredytowych w kolejnym miesiącu. Badanie oparto na zbiorze danych z Tajwanu (30 000 klientów, 6 miesięcy obserwacji). Głównym wyzwaniem było podporządkowanie całego procesu modelowania biznesowej, asymetrycznej macierzy kosztów ryzyka finansowego w stosunku **5:1**. 
* *False Negative (FN) - Koszt = 5:* Zaklasyfikowanie dłużnika jako klienta rzetelnego (bezpośrednia utrata kapitału przez bank).
* *False Positive (FP) - Koszt = 1:* Zaklasyfikowanie rzetelnego klienta jako niewypłacalnego (odrzucenie wniosku i utrata potencjalnych zysków odsetkowych).

### 🛠️ Istotne Metody i Przebieg Procesu
* **Inżynieria Cech:** Stworzono kluczowe zmienne syntetyczne o wysokiej mocy predykcyjnej:
  * `AVG_PAY_DELAY` – średnie opóźnienie w spłatach z ostatnich 6 miesięcy (najsilniejsza zmienna, ROC AUC = 0.725).
  * `UTILIZATION_RATIO` – stopień wykorzystania przyznanego limitu kredytowego.
  * `BILL_GROWTH` – trend przyrostu zadłużenia w krótkim okresie.
* **Preprocessing i Etyka (Fairness):** Podział danych ze stratyfikacją (80% trening, 20% test) w celu zachowania asymetrii klas (78% terminowych, 22% niewypłacalnych). Ze względów prawnych i etycznych zmienną płeć (`SEX`) wykluczono z modelowania. Do modelu Regresji Logistycznej zastosowano skalowanie odporne `RobustScaler`.
* **Dostrajanie Progów Klasyfikacji (Cut-off Tuning):** Zamiast domyślnego progu 0.5, próg odcięcia był optymalizowany indywidualnie dla każdego algorytmu z wykorzystaniem predykcji *Out-of-Fold* (10-fold CV) w celu bezpośredniej minimalizacji średniego kosztu ryzyka na klienta.
* **Ewaluowane Modele:** Drzewo Decyzyjne (z przycinaniem *Cost Complexity Pruning*), Regresja Logistyczna (GridSearchCV z regularyzacją L1/L2) oraz XGBoost (z RandomizedSearchCV i wagami `scale_pos_weight`).

### 🏆 Wyniki i Wnioski
* **Rekomendacja produkcyjna – XGBoost:** Model XGBoost osiągnął bezkonkurencyjny, najniższy średni koszt na klienta równy **0.556** (spadek o **41%** względem klasyfikatora losowego, którego koszt wynosił 0.940).
* Model XGBoost z powodzeniem przezwyciężył problem przeuczenia (różnica kosztów train vs. test to tylko 8%) i osiągnął najwyższą czułość **Recall = 78%** na zbiorze testowym.
* **W pełni przejrzysta alternatywa:** Jeśli nadzór bankowy wymaga w pełni transparentnych reguł *if-then*, optymalnym rozwiązaniem jest **Drzewo Decyzyjne** (średni koszt = **0.579**, Recall = 74%, ROC AUC = 0.759).

---

### [👥 Klasyfikacja Dochodów Demograficznych (UCI Adult Census Income)](https://github.com/m-kopacz/Predykcja_dochodow)

### 📌 Opis Projektu & Rozwiązywany Problem
Projekt dotyczył klasyfikacji binarnej mającej na celu przewidzenie, czy roczny dochód mieszkańca USA przekracza próg **50 000 USD** na podstawie jego cech demograficznych i społecznych. Badanie bazowało na klasycznym zbiorze *UCI Adult Dataset* (48 842 obserwacje). Wyzwaniami były: zaburzenie proporcji klas (jedynie 23.9% zarabia powyżej 50K), obecność braków danych oraz nieliniowe relacje cech demograficznych z poziomem zamożności.

### 🛠️ Istotne Metody i Przebieg Procesu
* **Preprocessing i Czyszczenie Danych:**
  * Braki danych w zmiennych kategorialnych (kodowane jako "?") uznano testami Chi-kwadrat za nielosowe (MNAR) i przekształcono w jawną, nową kategorię "Missing".
  * Wykluczono zmienną wagi statystycznej `fnlwgt` jako redundantną i grożącą przeuczeniem.
  * Zastosowano winsoryzację (przycięcie na poziomach 1. i 99. percentyla) dla stabilizacji cech ciągłych z ekstremalnymi obserwacjami odstającymi (np. zyski/straty kapitałowe).
  * Przeprowadzono kodowanie kategorialne: *Target Encoding* z wygładzaniem (dla modeli drzewiastych) oraz *One-Hot Encoding* (dla modeli liniowych i SVM).
* **Segmentacja nienadzorowana (PCA + KMeans):** Przed modelowaniem dokonano redukcji wymiarowości do dwóch składowych głównych (PC1, PC2) i wyodrębniono 3 naturalne segmenty społeczne, w tym "elitę dochodową" (starszą, wysoce wykształconą, pracującą najwięcej), w której 100% badanych osiągało dochody >50K USD.
* **Modelowanie i Optymalizacja Metryki F1-Score:**
  * Przetestowano pięć modeli: Drzewo Decyzyjne (zoptymalizowane pod kątem *Minimal Cost-Complexity Pruning* przy $\alpha = 0.000061$), XGBoost (z selekcją zmiennych), Lasy Losowe, Regresję Logistyczną oraz SVM (RBF z parametrem C=10.0 dobranym w GridSearchCV).
  * Każdy model przeszedł optymalizację progów decyzyjnych na podstawie krzywej *Precision-Recall* w celu maksymalizacji końcowego wskaźnika **F1-Score**.

### 🏆 Wyniki i Wnioski
Zestawienie końcowe modeli na zbiorze testowym:

| Model | Optymalny Próg | F1-Score (Test) | Accuracy (Test) | AUC (Test) |
| :--- | :---: | :---: | :---: | :---: |
| **XGBoost** | 0.5000 (standard) | **0.7290** | **86.62%** | **0.9254** |
| **Random Forest** | 0.6093 (tuned) | **0.7131** | 85.40% | 0.9145 |
| **Drzewo Decyzyjne** | 0.3432 (tuned) | **0.6962** | 84.89% | 0.9059 |
| **Regresja Logistyczna** | 0.3153 (tuned) | **0.6918** | 83.63% | 0.9042 |
| **SVM** | 0.1779 (tuned) | **0.6811** | 84.66% | 0.8773 |

* **Główny Wniosek:** **XGBoost** okazał się bezdyskusyjnym liderem, znakomicie radząc sobie ze skomplikowanymi interakcjami nieliniowymi. 
* **Strategiczna kalibracja progów:** Obniżenie progu odcięcia (w przedziały 0.17–0.34) dla klasycznych modeli (SVM, Regresja Logistyczna, Drzewo Decyzyjne) pozwoliło skompensować niezbalansowanie klas i przełożyło się na skokowy wzrost czułości bez istotnego uszczerbku dla precyzji.

---

### [Ekonometria panelowa w analizie determinant stopy inwestycji](https://github.com/m-kopacz/Ekonometria_panelowa_determinanty_inwestycji)

### 📌 Opis Projektu & Rozwiązywany Problem
Projekt zrealizowany w ramach pracy licencjackiej w Szkole Głównej Handlowej w Warszawie (SGH). Celem było zidentyfikowanie oraz ilościowe określenie siły i kierunku wpływu kluczowych determinant makroekonomicznych, społecznych oraz demograficznych na stopę inwestycji w Europie. Badanie przeprowadzono na bazie danych panelowych dla **20 państw europejskich** w latach **1995–2021** (niezbalansowany panel obejmujący 502 obserwacje).

### 🛠️ Istotne Metody i Przebieg Procesu
* **Badanie Stacjonarności:** Przeprowadzono testy stacjonarności **Im-Pesaran-Shin (IPS)**. Wykryto, że zmienne `rd`, `produktywność` oraz `urbanizacja` są niestacjonarne, natomiast `patenty` oraz `dependency` wykazują trendostacjonarność. Wykazano zjawisko wysokiej persystencji (autokorelacja rzędu >0.9).
* **Teoria Modelowania i Selekcja:** Estymowano modele Pooled OLS, Fixed Effects (FE) oraz Random Effects (RE).
  * *Test Walda* (Pooled OLS vs FE, F = 39.891, p-value < 2.2e-16) odrzucił homogeniczność na rzecz efektów indywidualnych.
  * *Test Hausmana* (FE vs RE, $\chi^2 = 23.406$, p-value = 0.00288) wykazał obecność endogeniczności w modelu RE.
  * **Ostateczny Wybór:** Model z efektami stałymi (FE, Within).
* **Szczegółowa Diagnostyka Składnika Losowego Modelu FE:**
  * *Współliniowość:* Współczynniki VIF dla wszystkich zmiennych były drastycznie niższe od 10 (brak zagrożenia).
  * *Autokorekcja:* Test Breuscha-Godfreya ($\chi^2 = 263.25$, p-value < 2.2e-16) potwierdził silną autokorelację pierwszego rzędu.
  * *Heteroskedastyczność:* Test Breuscha-Pagana ($BP = 60.2$, p-value < 1% ) wykazał silną niejednorodność wariancji.
  * *Zależność przekrojowa:* Test Breuscha-Pagana LM oraz CD Peserana ($CD = 9.0541$, p-value < 1%) potwierdziły obecność zależności przekrojowej.
* **Korekta Statystyczna:** Zastosowano odporną macierz wariancji-kowariancji **HC3**, uodparniając oceny na zdiagnozowane zaburzenia składnika losowego.
* **Ocena Prognostyczna Ex-post:** Podział na zbiór treningowy (1995–2016) oraz testowy (2017–2021).

### 🏆 Wyniki i Wnioski
* **Konsekwencje korekty HC3:** Zmienna `urbanizacja` całkowicie straciła istotność. Istotność zmiennych `bilans` i `patenty` uległa osłabieniu, natomiast pozostałe zmienne utrzymały silną istotność statystyczną.
* **Zidentyfikowane zależności (przy założeniu *ceteris paribus*):**
  * *Wzrost PKB* (+0.267) stymuluje stopę inwestycji (potwierdzenie **teorii akceleratora inwestycyjnego**).
  * *Wydatki na R&D* (+0.004 per capita) oraz *Patenty* (+0.001 na milion os.) silnie stymulują inwestycje (**teoria endogenicznego wzrostu**).
  * *Wzrost deficytu budżetowego* (bilans: -0.122) stymuluje inwestycje prywatne (występowanie **efektu przyciągania - crowding-in**).
  * *Bezrobocie* (-0.567) oraz *Obciążenie demograficzne* (-0.373) silnie hamują procesy inwestycyjne.
  * *Produktywność pracy* (-0.233) ma ujemny wpływ – wyższa efektywność obecnego kapitału pozwala na realizację celów bez konieczności ponoszenia nowych nakładów trwałych.
* **Własności predykcyjne:** Symulacja *ex-post* wykazała wysoki stopień trafności: **MAPE = 12.59%**, **MAE = 2.49%**, **RMSE = 2.96%**. Różnica RMSE-MAE (zaledwie 0.47 p.p.) potwierdziła doskonałą stabilność modelu (brak rzadkich, lecz rażących błędów predykcji).

---

### [🎗️ Analiza Przeżycia Pacjentek z Rakiem Piersi](https://github.com/m-kopacz/Analiza_przezycia_pacjentek_z_rakiem_piersi)

### 📌 Opis Projektu & Rozwiązywany Problem
Projekt obejmował rygorystyczne i wszechstronne badanie statystyczne czynników klinicznych i demograficznych determinujących przeżywalność pacjentek z inwazyjnym rakiem piersi. Badanie zrealizowano na bazie danych programu SEER obejmujących **4024 pacjentki** zdiagnozowane w latach **2006–2010** z czasem obserwacji do 10 lat. Zaimplementowano pełne spektrum metod analizy czasu trwania (Survival Analysis) w systemie SAS.

### 🛠️ Istotne Metody i Przebieg Procesu
* **Inżynieria Cech:** Stworzono ciągłą zmienną `Regional_Node_Positive_Ratio` (RNPR) określającą udział zajętych węzłów chłonnych do wszystkich zbadanych oraz jej dyskretny odpowiednik `rnpr_c` (kwartyle). Stan cywilny przekształcono w zmienną binarną (mężatka vs inne). Wykluczono zmienne redundantne (`differentiate`, `T_Stage`, `Stage_6th`).
* **Modele Nieparametryczne (PROC LIFETEST):**
  * *Metoda aktuarialna (tabele trwania życia):* Wykryto, że hazard nie jest stały i posiada dwa lokalne szczyty (wokół 50. miesiąca oraz najsilniejszy w przedziale 80–100 miesięcy od diagnozy).
  * *Estymator Kaplana-Meiera:* Wyznaczenie krzywej przeżycia dla całej próby (stabilizacja po 100 miesiącach na poziomie ok. 80%).
  * *Analiza porównawcza w warstwach:* Testy Log-Rank ($\chi^2 = 196.41$) i Wilcoxona ($\chi^2 = 173.80$) potwierdziły drastyczne różnice w przeżywalności w zależności od zaawansowania węzłowego.
* **Modele Parametryczne (PROC LIFEREG):**
  * Odrzucono model wykładniczy testem mnożnika Lagrange'a (hazard zmienny w czasie).
  * Wyselekcjonowano **model log-logistyczny** jako najlepiej dopasowany na podstawie AIC (3799.29) oraz BIC (3900.09).
  * Przeprowadzono **Bayesowską estymację MCMC** (10 000 próbek) z nieinformacyjnym rozkładem *a priori*. Zbieżność łańcuchów została w pełni zweryfikowana testami diagnostycznymi (Gelman-Rubin $\approx 1$, Geweke, Heidelberger-Welch).
* **Model Semiparametryczny Coxa (PROC PHREG):**
  * Dobór zmiennych metodą regresji krokowej (kryterium SLENTRY/SLSTAY = 0.05) wykluczył jedynie zmienną `A_Stage`.
  * *Weryfikacja założenia proporcjonalności:* Analiza graficzna oraz **ilościowy test reszt Schoenfelda** wykazały silne naruszenie założenia proporcjonalności dla statusu receptorów estrogenowych (`Estrogen_Status`) i progesteronowych (`Progesterone_Status`) oraz stanu cywilnego.
  * *Diagnostyka reszt:* Analiza reszt martyngałowych, odchyleń oraz punktowych. Usunięcie obserwacji odstających o reszcie odchyleń $> 3.0$ doprowadziło do znacznej poprawy wskaźników dopasowania (spadek AIC).
  * Do ostatecznej estymacji modelu Coxa włączono wyłącznie zmienne spełniające założenie proporcjonalności hazardów.

### 🏆 Wyniki i Wnioski
Ostateczne wyniki modelu proporcjonalnych hazardów Coxa:

| Zmienna w Modelu | Współczynnik ($\beta$) | p-value | Współczynnik Hazardu (HR) | Interpretacja Medyczna |
| :--- | :---: | :---: | :---: | :--- |
| **Regional_Node_Positive_Ratio** | 1.54721 | < 0.0001 | **4.698** | Wzrost udziału chorych węzłów z 0 do 100% zwiększa ryzyko zgonu **niemal 5-krotnie**. |
| **Age** | 0.01941 | < 0.0001 | **1.020** | Każdy rok życia pacjentki podnosi ryzyko zgonu o **2%**. |
| **Tumor_Size** | 0.00830 | < 0.0001 | **1.008** | Każdy milimetr wielkości guza zwiększa ryzyko zgonu o **0.8%**. |
| **Grade 1** (ref: Grade 3) | -1.13025 | < 0.0001 | **0.323** | Niski stopień złośliwości nowotworu to o **67.7% niższe** ryzyko zgonu. |
| **Grade 2** (ref: Grade 3) | -0.58965 | < 0.0001 | **0.555** | Umiarkowany stopień złośliwości to o **44.5% niższe** ryzyko zgonu. |
| **Race (Black)** (ref: White) | 0.48806 | 0.0001 | **1.629** | Pacjentki rasy czarnej mają o **62.9% wyższe** ryzyko zgonu. |
| **Race (Other)** (ref: White) | -0.37959 | 0.0350 | **0.684** | Pacjentki innych ras mają o **31.6% niższe** ryzyko zgonu. |

* **Zależność hormonalna słabnie w czasie:** Chociaż obecność receptorów estrogenowych i progesteronowych wykazuje początkowo silne działanie ochronne (w modelach parametrycznych opóźnia zgon o odpowiednio 42% i 32%), to z uwagi na naruszenie proporcjonalności ich rola ochronna słabnie w miarę upływu czasu. Pacjentki te wymagają ciągłego monitorowania hormonalnego w długim horyzoncie czasowym.
* **Rola czynników społecznych:** Bycie mężatką statystycznie istotnie poprawia prognozy i opóźnia moment zgonu pacjentki (mediana dłuższą o 14% względem kobiet samotnych).


### [Porównanie klasycznego i kwantowego SVM w klasyfikacji](https://github.com/m-kopacz/qSVM_klasyfikacja)
Porównanie wydajności klasycznego SVM oraz qSVM w zadaniu klasyfikacji chorób serca na zbiorze heart-statlog. 
* **Technologie:** `Python`, `PennyLane`, `scikit-learn`, `XGBoost`
* **Wynik:** Uzyskano identyczne predykcje w najlepszych wariantach modeli, z marginalną przewagą klasycznego SVM w kwestii ROC AUC (0.90 vs 0.89).
