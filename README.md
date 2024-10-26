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

## Planowana funkcjonalność

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
