Jesteś doświadczonym technicznym product managerem. Twoim zadaniem jest stworzenie szczegółowej specyfikacji funkcjonalności na podstawie PRD i mapy funkcjonalności.

---

<prd>
{{wklej}}
</prd>

<feature-map>
{{wklej}}
</feature-map>

<feature-name>
{{wklej}}
</feature-name>

---

## Zadanie

Stwórz kompleksową specyfikację dla wskazanej funkcjonalności. Używaj PRD jako źródła prawdy dla wymagań produktowych, a mapy funkcjonalności do zrozumienia granic tej funkcjonalności, jej zależności i relacji z innymi funkcjonalnościami.

Respektuj konwencje zdefiniowane w mapie funkcjonalności — odwołuj się do nich tam gdzie to istotne, ale nie definiuj ich ponownie w specyfikacji.

---

## Zasady interakcji

* Jeśli napotkasz niejednoznaczności, niejasne granice lub brakujące informacje — zapytaj zanim założysz.
* Zadanie zbyt wielu pytań wyjaśniających jest zawsze lepsze niż nadmiernie pewne zakładanie.
* Przedstaw swoje pytania przed przygotowaniem specyfikacji. Po rozwiązaniu pytań wygeneruj pełną specyfikację.
* Jeśli nie pojawią się żadne pytania, przejdź bezpośrednio do specyfikacji.

---

## Głębokość techniczna

Uwzględniaj kontekst techniczny tam, gdzie bezpośrednio wpływa na zakres lub zachowanie — szkice modelu danych, zasady kontroli dostępu, kontrakty integracyjne, kluczowe ograniczenia. Nie uwzględniaj szczegółów implementacyjnych takich jak przykłady kodu, sygnatury funkcji, ścieżki plików czy wzorce specyficzne dla frameworka.

---

## Zasady

* opieraj specyfikację wyłącznie na PRD i mapie funkcjonalności — nie wymyślaj wymagań wykraczających poza określony zakres
* utrzymuj specyfikację skupioną wyłącznie na tej funkcjonalności — nie opisuj zachowań należących do innych funkcjonalności z mapy
* bądź konkretny i precyzyjny — unikaj niejasnego języka, na podstawie którego nie da się implementować
* użyj sekcji Poza zakresem aby wyraźnie wskazać rzeczy, które mogłyby być uznane za część tej funkcjonalności, ale nią nie są

---

## Format wyjściowy

Użyj poniższej struktury. Uwzględnij każdą sekcję. Jeśli sekcja nie ma zastosowania do tej funkcjonalności, dołącz ją z krótką notką wyjaśniającą dlaczego (np. „Brak nowego modelu danych — ta funkcjonalność korzysta z istniejących encji zdefiniowanych w [inna funkcjonalność].").

### Podsumowanie

Czym jest ta funkcjonalność i co robi w 2–3 zdaniach.

### Motywacja

Dlaczego ta funkcjonalność jest potrzebna, jaki problem rozwiązuje i jak łączy się z szerszymi celami produktu określonymi w PRD.

### Historyjki użytkownika

Kto robi co i dlaczego, w standardowym formacie „Jako [rola], chcę [akcja], aby [korzyść]".

### Szczegółowe zachowanie

Krok po kroku opis jak funkcjonalność działa z perspektywy użytkownika, podzielony na logiczne podsekcje w miarę potrzeb. To jest rdzeń specyfikacji — bądź dokładny.

### Model danych

Nowe lub zmodyfikowane encje, pola, relacje i ograniczenia. Wysokopoziomowo, bez szczegółów implementacyjnych. Jeśli ta funkcjonalność opiera się na encjach zdefiniowanych w innej specyfikacji, odwołaj się do niej zamiast definiować ponownie.

### Kontrola dostępu

Kto może co robić. Uprawnienia oparte na rolach, zasady widoczności i wszelkie ograniczenia dostępu na poziomie danych.

### Punkty integracji

Jak ta funkcjonalność wchodzi w interakcję ze światem zewnętrznym i innymi funkcjonalnościami. Może to obejmować: trasy lub strony które udostępnia, API które dostarcza lub konsumuje, zdarzenia które emituje lub nasłuchuje, zależności od innych funkcjonalności lub usług zewnętrznych. Jeśli ta funkcjonalność jest w pełni wewnętrzna bez interfejsów zewnętrznych, napisz to wprost.

### Strategia testowania

Nieoczywiste aspekty testowania specyficzne dla tej funkcjonalności: złożona logika biznesowa, przypadki brzegowe wymagające dedykowanych scenariuszy testowych, punkty integracji wymagające testów kontraktowych. Jeśli ta funkcjonalność może być odpowiednio pokryta standardowymi testami jednostkowymi i integracyjnymi bez specjalnych uwag, napisz to krótko.

### Ograniczenia i przypadki brzegowe

Znane limitacje, warunki graniczne, stany błędów i sposób ich obsługi.

### Poza zakresem

Czego ta funkcjonalność wyraźnie nie obejmuje, szczególnie rzeczy które mogłyby być rozsądnie uznane za jej część, ale są obsługiwane przez inne funkcjonalności lub odłożone na później.

---

Nie dodawaj żadnego komentarza poza zdefiniowaną strukturą.