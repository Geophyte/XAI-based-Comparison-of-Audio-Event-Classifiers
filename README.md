# XAI-based-Comparison-of-Audio-Event-Classifiers
Implementacja i ewaluacja artykułu [XAI-based Comparison of Audio Event Classifiers with different Input Representations](https://dl.acm.org/doi/fullHtml/10.1145/3617233.3617265)

## Harmonogram
Tygodniowy harmonogram przebiegu projektu:

04.11.2024 Konfiguracja środowiska, zapoznanie się z datasetem i bibliografią

11.11.2024 Eksploracja i obróbka danych, skrypty wizualizacji

18.11.2024 Implementacja i testy DCNN

25.11.2024 Implementacja i testy YAMNet

02.12.2024 Wdrożenie LRP, DFT-LRP i wskaźników do porównań

09.12.2024 Odtworzenie eksperymentu i prace nad potencjalnymi błędami

## Planowany zakres eksperymentów

### Przygotowanie i obróbka danych
Pliki audio o różnych długościach, częstościach próbkowania i liczbie kanałów są ujednolicone do 1-sekundowych fragmentów z częstością próbkowania fS = 16 kHz, z 50% nakładaniem się, a następnie konwertowane do mono. W ten sposób tworzymy 38 000 próbek. Zbiór danych UrbanSound8k jest dostarczany z zakresem sygnału $[−1,1]$, ale aby uwzględnić różne typy dźwięków, normalizujemy każdą próbkę na podstawie jej pierwiastka średniego kwadratowego. Dzięki temu ciche, ciągłe dźwięki mają inny zakres dynamiczny niż krótkie, głośne zdarzenia.
Dodatkowo, podczas treningu modelu, stosujemy następujące techniki augmentacji dźwięku, aby zwiększyć odporność i poprawić wydajność generalizacji:
- Zwiększenie głośności w zakresie od −12 dB do −1 dB.
- Szum o stosunku sygnału do szumu (SNR) w zakresie od 0.0001 do 0.1.
- Opóźnienie w zakresie od 1 ms do 300 ms.
- Filtr pasmowy z częstotliwościami odcięcia w przedziałach:
  - Dolnoprzepustowy (LP): [1400 Hz; 4000 Hz]
  - Górnoprzepustowy (HP): [500 Hz; 1200 Hz]

### Wytrenowanie modelu `1DCNN`
`1DCNN` to architektura sieci konwolucyjnej, przetwarza próbki audio jako jednowymiarowe surowe fale dźwiękowe w domenie czasu.
Trenujemy ją na zadaniu klasyfikacji zdarzeń dźwiękowych UrbanSound8k, używając optymalizatora Adadelta z szybkością uczenia wynoszącą 1,0.
Używamy następującego podziału danych: 80% danych jest używane do treningu, 10% do walidacji, a pozostałe 10% jest zarezerwowane do testowania.
Według artykułu model osiąga dokładność na zbiorze treningowym wynoszącą 75,22 ± 0,44 %, a dokładność na zbiorze testowym wynoszącą 62,48 ± 0,72 % w ramach 10-krotnej walidacji krzyżowej.

### Przekształcenie sygnału z fal dźwiękowych na spektrogramy
Wykorzystujemy do tego STDFT - Short Time Discrete Fourier Transform (Krótkoterminową Dyskretną Transformatę Fouriera)

### Wytrenowanie modelu `YAMNet`
`YAMNet (Yet Another Merging NETwork)` to konwolucyjna sieć neuronowa, która przetwarza próbki audio jako dwuwymiarowe logmel-spektrogramy w domenie czasu-częstotliwości.
Ponownie używamy optymalizatora Adadelta z szybkością uczenia 1,0 do trenowania modelu. W ramach 10-krotnej walidacji krzyżowej, według artykułu, model osiąga dokładność na zbiorze treningowym wynoszącą 91,21±0,21% oraz dokładność na zbiorze testowym 62,03±0,72%.

### Analiza jakościowa strategii klasyfikacji z wykorzystaniem map cieplnych istotności LRP
Aby umożliwić porównanie strategii klasyfikacji między tymi dwoma modelami, które działają w różnych domenach wejściowych, wykorzystujemy DFT-LRP.

### Ilościowe porównanie strategii klasyfikacji oparte na XAI
Z wykorzystaniem map cieplnych istotności DFT-LRP w jednolitej domenie.

## Planowana funkcjonalność
W ramach projektu planowana jest implementacja i ewaluacja dwóch konwolucyjnych sieci neuronowych przeznaczonych do klasyfikacji zdarzeń dźwiękowych, z których jedna operuje na surowych sygnałach w dziedzinie czasu, a druga na reprezentacjach czasowo-częstotliwościowych (spektrogramach). Kluczowym elementem będzie zastosowanie metod LRP oraz DFT-LRP do obliczania map istotności w dziedzinie czasowo-częstotliwościowej, które umożliwią ocenę, jak poszczególne komponenty sygnału wpływają na prawdopodobieństwo przypisania próbki do określonej klasy. Celem tego etapu będzie opracowanie metryk umożliwiających ilościową ocenę różnic i podobieństw między mapami istotności dla obu typów modeli. Analiza wyników pozwoli nie tylko na wybór modelu o najlepszych wynikach klasyfikacyjnych, ale również na uwzględnienie procesów decyzyjnych, jakie zachodzą w modelu.

## Planowany stack technologiczny

### Język
Python 3.9+

### Struktura projektu
- [Szablon CookieCutter Data Science](https://cookiecutter-data-science.drivendata.org/)

### Zbiór danych
[Kaggle: UrbanSound8K](https://www.kaggle.com/datasets/chrisfilo/urbansound8k)  
W celu jak najdokładniejszej reprodukcji wyników badań oryginalnej pracy, użyty zostanie dokładnie ten sam zbiór danych.

### Podział zbioru danych
- [Scikit-learn Model selection](https://scikit-learn.org/stable/model_selection.html)

### Przetwarzanie danych audio

#### NumPy
- Generalne operacje na zbiorze danych
- Generacja hałasu do modyfikacji danych

#### SciPy
- Modyfikacja ścieżek ze zbioru danych poprzez nakładanie filtrów i efektów, np: delay, odcinanie pasm z zadanymi częstotliwościami

#### Librosa
- Dokładniejsza analiza plików audio
- Tworzenie spektrogramów

#### Pydub
- Modyfikacja ścieżek ze zbioru danych poprzez manipulację intensywnością dźwięku 

### Frameworki i narzędzia do uczenia maszynowego

#### Pytorch
- Zaimplementowane bibliotecznie 1-warstwowe konwolucyjne sieci neuronowe jako pierwszy z modeli, będących obiektem badania
- Algorytm Adadelta zastosowany w przypadku obydwu modeli

#### YAMNet
- Drugi z modeli nad którymi praca się pochyla [YAMNet](https://github.com/tensorflow/models/tree/master/research/audioset/yamnet)

### Narzędzia eXplainable AI
#### Zennit
- [Zennit](https://zennit.readthedocs.io/en/latest/)
- W celu jak najdokładniejszej reprodukcji wyników badań, do zbadania propagacji istotności między warstwami użyte zostanie to samo narzędzie co w oryginalnej pracy

## Użyteczne linki
* [YAMNet](https://github.com/tensorflow/models/tree/master/research/audioset/yamnet)
* [Kaggle: UrbanSound8K](https://www.kaggle.com/datasets/chrisfilo/urbansound8k)

## Bibliografia
* [XAI-based Comparison of Audio Event Classifiers with different Input Representations](https://dl.acm.org/doi/fullHtml/10.1145/3617233.3617265)
* [AudioMNIST: Exploring Explainable Artificial Intelligence for audio analysis on a simple benchmark](https://www.sciencedirect.com/science/article/pii/S0016003223007536)
* [End-to-End Environmental Sound Classification using a 1D Convolutional Neural Network](https://arxiv.org/abs/1904.08990)
* [MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications](https://arxiv.org/abs/1704.04861)
* [Software for Dataset-wide XAI: From Local Explanations to Global Insights with Zennit, CoRelAy, and ViRelAy](https://arxiv.org/abs/2106.13200)
* [A Dataset and Taxonomy for Urban Sound Research](https://www.justinsalamon.com/uploads/4/3/9/4/4394963/salamon_urbansound_acmmm14.pdf)
