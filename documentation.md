# Dokumentacja Projektu: Pipeline Anonimizacji Danych

## 1. Proces i Dokumentacja (40 pkt)

### Narzędzia i Prompty GenAI
W trakcie pracy nad projektem wykorzystano model Gemini 3 Flash jako asystenta do generowania danych, weryfikacji logiki kodu oraz konsultacji aspektów merytorycznych.

**Rzeczywiste prompty wykorzystane w procesie:**
*   *„Pokaż mi dane, jakie mogą być brane pod uwagę w kontekście anonimizacji: Imię, nazwisko, data urodzenia, pesel, nr tel, e-mail, narodowość, kraj zamieszkania, miasto zamieszkania”*.
*   *„Zrób JSON, 20 różnych użytkowników z sugerowanymi przez ciebie danymi”*.
*   *„Zmień dataset tak, aby po zanonimizowaniu (dekada urodzenia + kraj) k-count miał różne wartości, a nie tylko 1”*.
*   *„Czy maskowanie i generalizacja są pseudonimizacją?”*.

### Kluczowe decyzje projektowe
*   **Rezygnacja z hashowania na rzecz usuwania (Suppression):** Początkowo planowano użycie algorytmu SHA-256 dla adresów email. Po analizie merytorycznej uznano jednak, że bez usunięcia klucza jest to jedynie pseudonimizacja. Wybrano całkowite usunięcie identyfikatorów bezpośrednich (`full_name`, `national_id`), aby zapewnić pełną nieodwracalność procesu.
*   **Dobór ziarnistości generalizacji:** Daty urodzenia uogólniono do dekad, a lokalizację do poziomu kraju. Było to niezbędne, aby w małym zbiorze (20 rekordów) uzyskać grupy liczące więcej niż jedną osobę.
*   **Zastosowanie Bucketingu:** Zarobki zamieniono na kategoryczne przedziały (np. High, Medium), co skutecznie ukrywa wartości ekstremalne, które mogłyby posłużyć do identyfikacji osób o unikalnych dochodach.

### Co nie zadziałało i jak to rozwiązano?
*   **Problem:** Zachowanie kolumny `city` powodowało, że większość rekordów pozostawała unikalna ($k=1$), co czyniło anonimizację nieskuteczną.
*   **Rozwiązanie:** Zrezygnowano z danych o mieście na rzecz poziomu krajowego, co pozwoliło na stworzenie stabilnych klas równoważności.

---

## 2. Merytoryka i Analiza Wyników (40 pkt)

### Wnioski z uzyskanych wyników
Na podstawie eksperymentów opisanych w `pipeline.ipynb` wyciągnięto następujące wnioski:
*   **Analiza Bezpieczeństwa ($k$-anonymity):** Dzięki modyfikacji danych wejściowych uzyskano zróżnicowane poziomy ochrony. Grupa indyjska osiągnęła $k=6$, co zapewnia wysoką anonimowość, podczas gdy grupy szwedzka i japońska posiadają $k=2$, co jest minimalnym dopuszczalnym progiem.
*   **Analiza Użyteczności (Mean Distortion):** Odnotowano stratę precyzji zarobków na poziomie **91.62%**. Wynika to z faktu, że przypisanie środka przedziału dla osób o skrajnie wysokich dochodach (np. grupa "Very high") drastycznie zniekształca średnią arytmetyczną całego zbioru.

### Aspekty prawne i etyczne
*   **Rozróżnienie prawne:** Projekt wyraźnie odcina się od pseudonimizacji. Udowodniono, że samo "zasłonięcie" nazwiska to za mało – dopiero uzyskanie $k>1$ poprzez generalizację pozwala nadać danym status zanonimizowanych w myśl RODO.
*   **Etyka danych:** Zauważono, że mniejszości (np. rzadkie narodowości w zbiorze) są zawsze gorzej chronione. Wymusza to dylemat etyczny: uogólnić dane tak bardzo, że stracą wartość, czy zaakceptować wyższe ryzyko dla nielicznych jednostek.

### Powiązanie wyników z projektem
Zastosowana metryka **Query Accuracy** (dokładność zapytań) wyniosła **63.64%**. Potwierdza to, że pipeline spełnia swoje zadanie: dane są bezpieczne (brak $k=1$), a jednocześnie nadal pozwalają na wyciąganie poprawnych (choć mniej precyzyjnych) wniosków o trendach w populacji.