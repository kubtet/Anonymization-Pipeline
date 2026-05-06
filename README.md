# Anonymization-Pipeline
Project for college course Legal, Social and Ethical Aspects of Artificial Intelligence

This repository contains a sample dataset of 20 users in JSON format. It includes a Jupyter Notebook (pipeline.ipynb) documenting the experiments and methods used to anonymize the data, along with a markdown file providing detailed project documentation.

### Cel projektu
Pipeline ma na celu przekształcenie zbioru danych PII w formę zanonimizowaną, która minimalizuje ryzyko re-identyfikacji przy jednoczesnym zachowaniu przydatności danych do ogólnych analiz statystycznych.

### Struktura plików
*   `data.json`: Plik wejściowy z 20 syntetycznymi rekordami użytkowników.
*   `pipeline.ipynb`: Notebook zawierający implementację metod oraz obliczenia metryk.
*   `README.md`: Dokumentacja projektu.

### Instrukcja uruchomienia
*   Wymagane środowisko: Python 3.x z biblioteką `pandas`.
*   Wczytaj plik `pipeline.ipynb` i upewnij się, że `data.json` znajduje się w tym samym folderze.
*   Uruchom komórki sekwencyjnie, aby prześledzić proces od surowych danych do raportu końcowego.

### Zakres działania
*   **Co robi:** Usuwa identyfikatory bezpośrednie, maskuje domeny e-mail, generalizuje wiek i lokalizację, oblicza wskaźnik $k$-anonymity oraz błąd użyteczności danych.
*   **Czego nie robi:** Nie zabezpiecza danych przed atakami typu "Background Knowledge" (użycie wiedzy zewnętrznej do identyfikacji).
