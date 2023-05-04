---
title: "układ zasilający do płytek stykowych"
shortTitle: "zasilacz do płytek stykowych"
subtitle: "jak zbudować zasilacz do płytek prototypowych"
url: baza-wiedzy/artykuly/uklad-zasilajacy-do-plytek-stykowych/
categories:
- article
tags:
- electronics
image:
    name: breadboard-power-supply
    extension: svg
    height: 72
    width: 72
readMore: true
publishDate: 2020-11-11
---
**Układ zasilający do płytek stykowych pozwala na doprowadzenie odpowiedniego napięcia do pozostałych budowanych obwodów elektronicznych.**
<!--more-->
Głównym elementem układu zasilającego do płytek stykowych jest mostek Graetza (zabezpieczenie przed odwrotnym podłączeniem źródła napięcia stałego, np. styków baterii) oraz stabilizator napięcia (utrzymywanie na wyjściu stałej wartości pożądanego napięcia).

Opisana niżej wersja 1.0 układu jest jednocześnie pierwszym zbudowanym przeze mnie obwodem elektronicznym, który znajduje praktyczne zastosowanie. Nie jest doskonały, ale na początkowym etapie nauki elektroniki w zupełności wystarczający.

Kolejne wersje układu zasilającego uzupełnione będą o:
* Wyłącznik on-off zamiast przycisku tact switch,
* Diodę sygnalizującą podłączenie zasilania (zamiast diody świecącej w momencie włączenia układu przyciskiem),
* Złącze DC do podłączenia gniazdkowego zasilacza 12 V,
* Złącze USB (jeśli w trakcie użytkowania okaże się, że byłoby przydatne),
* Napięcie na wyjściu 3,3 V.

Finalna wersja układu będzie zmontowana na płytce drukowanej wyposażonej w złącza pin, dzięki czemu moduł będzie można wpiąć bezpośrednio w prototypową płytkę stykową.

## Wersja 1.0

{{<image src="breadboard-power-supply-v10-20201114-bb.webp" caption="Wizualizacja układu zasilającego do płytek stykowych v. 1.0">}}
{{<image src="breadboard-power-supply-v10-20201114-scheme.webp" caption="Schemat układu zasilającego do płytek stykowych v. 1.0">}}
{{<image src="breadboard-power-supply-v10-20201114-photo.webp" caption="Fotografia układu zasilającego do płytek stykowych v. 1.0">}}

Pierwsza wersja mojego układu zasilającego do płytek stykowych niemal w całości została zaczerpnięta z książki Pawła Borkowskiego pt. Przygoda z elektroniką. Moim autorskim wkładem było jedynie dodanie włącznika i diody, żeby mieć pewność, że wszystkie styki są we właściwych miejscach. Podmieniłem też kondensator C3. Zamiast 470 μF podłączyłem 220 μF, ponieważ tylko taki miałem akurat w zasobach, a nie chciałem łączyć kilku, żeby nie komplikować wizualizacji i zdjęcia.

Do skonstruowania układu (przypominam – to mój pierwszy w miarę sensownie działający obwód) podszedłem mocno ambitnie. Starałem się nie posiłkować zamieszczonymi w książce ilustracjami i korzystać wyłącznie ze schematu ideowego. Ponadto, aby zilustrować ten post czymś więcej niż tylko fotografią, opanowałem obsługę programu Fritzing i przyjąłem wstępny schemat opisywania i archiwizowania swoich kolejnych projektów.

### Lista podzespołów

* D1-D4: dioda prostownicza 1N4148,
* C1-C2: kondensator ceramiczny 100 nF,
* C3: kondensator elektrolityczny 470 μF,
* C4: kondensator elektrolityczny 100 μF,
* R1: rezystor 220 Ω,
* stabilizator 5 V 7805,
* dioda LED,
* przycisk tact-switch lub jakikolwiek inny wyłącznik.

### Opis działania

Źródłem napięcia wejściowego jest bateria 9 V, którą podłącza się do pól w rzędzie nr 1 płytki stykowej. Dzięki zastosowaniu mostka Graetza polaryzacja baterii (kolejność podłączania biegunów) nie ma znaczenia. Ważne jest tylko to, aby jeden biegun był podłączony po jednej stronie płytki (pola 1A-1E), a drugi po drugiej (pola 1F-1I).

Kolejnym komponentem układu jest stabilizator, którego rolą jest obniżenie napięcia wejściowego do oczekiwanej wartości (w tym przypadku 5 V) i utrzymywanie go na stałym poziomie, niezależnie od przyłożonego na wyjściu obciążenia. Zadaniem towarzyszących mu kondensatorów jest zapobieganie wzbudzaniu się stabilizatora, czyli generowaniu niepożądanych impulsów. W momencie tworzenia tego artykułu nie wnikałem jednak głębiej, na czym to zjawisko tak na prawdę polega.

### Rozwiązywanie problemów

Przy pierwszych testach układu okazało się, że napięcie wyjściowe jest dalekie od stanu, który mógłbym nazwać „stabilnym”. Póki co zidentyfikowałem dwa problemy:

1. Zbyt niskie napięcie na wejściu. Do prawidłowej pracy każdy stabilizator potrzebuje odpowiedniego marginesu między napięciem wejściowym, a wyjściowym (tzw. dropout). W przypadku użytego stabilizatora 7805 różnica ta powinna wynosić co najmniej 2 V, co oznacza że minimalne napięcie wejściowe musi być wyższe niż 7 V. Dodatkowo, mostek Graetza powoduje spadek napięcia o ok. 1,4 V (dwie diody prostownicze × 0,7 V każda), czyli na stykach baterii musimy odnotowywać już co najmniej 8,4 V. Innymi słowy, do prawidłowego działania obwodu w wersji 1.0 potrzebowałbym niemałego zapasu fabrycznie nowych ogniw. Rozważam więc dwie opcje:
   * wyposażenie układu zasilającego w złącze zasilacza gniazdkowego,
   * usunięcie mostku Graetza :).

2. Uszkodzone podzespoły. W moim przypadku – po kilku godzinach spędzonych na sprawdzaniu i wymienianiu każdego elementu z osobna – wadliwa okazała się płytka stykowa. Po przełączeniu wszystkich elementów na nową płytkę, wskazania multimetru na wyjściu z układu zatrzymały się na cieszącej oko wartości 5,01 V.

## Wersja 2.0

{{<image src="breadboard-power-supply-v20-20201211-bb.webp" caption="Wizualizacja układu zasilającego do płytek stykowych v. 2.0">}}
{{<image src="breadboard-power-supply-v20-20201211-scheme.webp" caption="Schemat układu zasilającego do płytek stykowych v. 2.0">}}
{{<image src="breadboard-power-supply-v20-20201211-photo.webp" caption="Fotografia układu zasilającego do płytek stykowych v. 2.0">}}

Zgodnie z planem, druga wersja mojego układu zasilającego do płytek stykowych nie różni się zasadą działania od pierwszego modelu. Najważniejszą modyfikacją było dodanie stabilizatora 3,3 V (LD1117V33). Przy okazji nauczyłem się, że po zakupie nowych podzespołów zawsze trzeba zapoznać się z ich dokumentacją. W przypadku tego elementu okazało się, że schemat wyprowadzeń na nóżkach różni się od zastosowanego w modelu 7805.

Ponadto, do układu dodałem:
* gniazdo DC 5,5/2,5 mm, umożliwiające zasilanie układów z gniazdkowego zasilacza 12 V,
* przełącznik suwakowy (zamiast wciskanego).

Zmieniłem też umiejscowienie diod LED, które teraz sygnalizują prawidłową pracę każdego stabilizatora z osobna. Stabilizator 5 V podłączony jest do dolnej szyny płytki, a 3,3 V do górnej. Dzięki temu mogę jednocześnie korzystać z obu poziomów napięcia.

Zastanawiam się jeszcze, czy szeregowe podłączenie stabilizatorów jest prawidłowe. Za wyborem takiego rozwiązania przemawia zasada działania tych elementów, które podczas obniżania napięcia „nadmiarową” moc zamieniają w energię cieplną. Weźmy dla przykładu urządzenie, które podłączone do szyny 3,3 V będzie zasilane prądem 100 mA. Pobierana przez niego moc wynosić będzie 0,33 W (P = U × I). Moc pobierana przez zasilacz 12 V będzie wynosić 1,2 W. Różnica między 1,2 W a 0,33 W będzie się odkładała na stabilizatorze i zamieniała w ciepło. Przy podłączeniu równoległym cała marnowana moc byłaby emitowana przez jeden stabilizator (w efekcie czego komponent mocno by się nagrzewał), a przy podłączeniu szeregowym rozkłada się na dwa podzespoły.

Przez chwilę byłem przekonany, że życie to nieustająca sztuka kompromisu i muszę się po prostu pogodzić z dodatkowym źródłem ciepła na biurku. Zapobiegawczo zamontowałem nawet widoczne na zdjęciu radiatory. W międzyczasie przeczytałem jednak o przetwornicach impulsowych, których straty mocy wynikające z obniżania napięcia wynoszą jedynie ok. 10%. 

Oznacza to tylko jedno – układ zasilający do płytek stykowych doczeka się co najmniej jeszcze jednej wersji.