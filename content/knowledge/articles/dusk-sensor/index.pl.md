---
title: "czujnik zmierzchu"
subtitle: "jak zbudować czujnik zmierzchu za pomocą komparatora napięcia LM 311"
categories:
- article
tags:
- electronics
image:
    name: dusk-sensor
    extension: svg
    height: 96
    width: 96
readMore: true
publishDate: 2020-12-06
---
**Czujnik zmierzchu to układ, który ma wykonać zaplanowane działanie (np. zaświecenie diody) w momencie spadku natężenia światła do określonego poziomu.**
<!--more-->
Czujnik zmierzchu jako taki raczej nie znajdzie zastosowania w moim robocie współpracującym. Układ ten potraktowałem jednak jako ćwiczenie wykorzystania innych analogowych detektorów i próbę przełożenia odbieranych przez nich danych na sygnały sterujące robotem. Na tym etapie wyobrażam sobie np. instalację czujnika zbliżeniowego, który awaryjnie wyłączy robota, gdy w jego polu działania pojawi się ludzka kończyna.

Mimo, że wersje 1.0 oraz 2.0 czujnika zmierzchu nie są zbyt skomplikowane, to chwilę mi zajęło zanim zrozumiałem zasadę działania dzielnika napięcia i dobrałem odpowiednie wartości rezystorów. Ponadto, w trakcie testów okazało się, że fotorezystory o teoretycznie tych samych parametrach mogą mieć różne wartości rezystancji w tych samych warunkach (czyli tym samym oświetleniu). Dlatego też do każdego modelu dodałem potencjometr, który umożliwia regulację czułości układu.

## czujnik zmierzchu: wersja 1.0

{{<image src="dusk-sensor-v10-20201205-bb.webp" caption="Wizualizacja czujnika zmierzchu v. 1.0">}}
{{<image src="dusk-sensor-v10-20201205-scheme.webp" caption="Schemat czujnika zmierzchu v. 1.0">}}
{{<image src="dusk-sensor-v10-20201205-photo.webp" caption="Fotografia czujnika zmierzchu v. 1.0">}}

W pierwszej wersji mojego czujnika zmierzchu elementem sterującym jest standardowy tranzystor NPN. Tranzystor przewodzi prąd na linii kolektor-emiter tylko wtedy, gdy do jego bazy będzie przyłożone napięcie o wartości większej niż ok. 0,6 V. Czujnikiem natężenia światła jest fotorezystor, którego rezystancja jest tym większa, im jest ciemniej. Innymi słowy, musiałem tak ułożyć komponenty, aby w momencie zwiększania się rezystancji na fotorezystorze (gdy zapada zmrok), zwiększało się napięcie na bazie tranzystora, a w efekcie zapalała się dioda podłączona do kolektora.

W tym celu wykorzystałem dzielnik napięcia składający się z:
* Rezystora 1 kΩ (R1),
* Potencjometru 50 kΩ (R2),
* Fotorezystora (R3).

Wartość napięcia doprowadzonego do bazy tranzystora Q1 wynika ze wzoru:

$$ Uwyj = Uzas * \frac{R3}{R1+R2+R3} $$

Dzięki takiemu podłączeniu fotorezystora R3, że pojawia się zarówno w liczniku jak i mianowniku wzoru, to wraz ze wzrostem jego rezystancji rośnie również napięcie Uwyj, czyli to, które odkładane jest na bazie tranzystora Q1.

Podłączenie potencjometru R2 wyłącznie w mianowniku sprawia, że umożliwia prostą regulację układu. Im większa jest jego rezystancja, tym większa musi być wartość R3, aby Uwyj przekroczyło wartość graniczną. Innymi słowy – musi być ciemniej, aby dioda się zaświeciła.

W poniższej tabeli znajdziesz różne zestawienia parametrów obrazujące działanie całego czujnika.

{{<table>}}
| Pora dnia       | Uzas [V] | R3 [kΩ] | R1 [kΩ] | R2 [kΩ] | Uwyj [V] | LED1 |
| --------------- | -------- | ------- | ------- | ------- | -------- | ---- |
| Rano            | 3,3      | 2,0     | 1,0     | 25,0    | 0,24     | OFF  |
| Przed południem | 3,3      | 4,0     | 1,0     | 25,0    | 0,44     | OFF  |
| Po południu     | 3,3      | 6,0     | 1,0     | 25,0    | 0,62     | ON   |
| Wieczorem       | 3,3      | 10,0    | 1,0     | 50,0    | 0,54     | OFF  |
| Noc             | 3,3      | 15,0    | 1,0     | 50,0    | 0,75     | ON   |
{{</table>}}

Teraz ciekawostka. Wg powyższego modelu, po skręceniu potencjometru R2 do zera i uruchomieniu czujnika w nocy, na bazie powinno odłożyć się napięcie > 3 V, co skutkowałoby pewnie uszkodzeniem tranzystora. W rzeczywistości wynosi ono 0,75 V – tranzystor jest nasycony, dioda świeci, wszystko gra, nic nie płonie. Szukałem wytłumaczenia tego zjawiska i znalazłem ciekawy wątek na elektroda.pl, którego na ten moment nie chcę jeszcze mocno zgłębiać.

Czujnik zmierzchu w wersji 1.0 jest prosty, ale działa. Ma jednak jedną podstawową wadę – sygnał wyjściowy (czyli w tym przypadku świecąca się dioda LED1) jest analogowy. W miarę jak robi się coraz ciemniej dioda płynnie świeci się coraz jaśniej. W przypadku mojego robota (czy w ogóle w elektronice cyfrowej) bardziej przydatny byłby sygnał zero-jedynkowy. Jeśli jest jasno to dioda się nie świeci, a jeśli jest ciemno to się świeci. Bez stanów pośrednich w stylu „trochę jasno – trochę świeci”.

## Czujnik zmierzchu: wersja 2.0

{{<image src="dusk-sensor-v20-20201211-bb.webp" caption="Wizualizacja czujnika zmierzchu v. 2.0">}}
{{<image src="dusk-sensor-v20-20201211-scheme.webp" caption="Schemat czujnika zmierzchu v. 2.0">}}
{{<image src="dusk-sensor-v20-20201211-photo.webp" caption="Fotografia czujnika zmierzchu v. 2.0">}}

W celu wyeliminowania największej wady pierwszej wersji mojego czujnika zmierzchu, w drugim podejściu zastosowałem komparator napięcia LM 311. Układ ten porównuje wartości napięć przyłożonych do tzw. wejścia nieodwracającego (pin 2 oraz „+” na schemacie) i wejścia odwracającego (pin 3 oraz „-” na schemacie). Wynikiem tego porównania jest napięcie na wyjściu układu (pin 7):
* jeżeli napięcie na wejściu nieodwracającym jest większe niż na wejściu odwracającym to napięcie na wyjściu jest równe napięciu zasilania (jeżeli pin 2 > pin 3 to Uwyj = VCC).
* jeżeli napięcie na wejściu nieodwracającym jest niższe niż na wejściu odwracającym to napięcie na wyjściu jest równie zeru (jeżeli pin 2 < pin 3 to Uwyj = 0).

W moim układzie punktem odniesienia jest wejście odwracające (pin 3), do którego przykładam napięcie z dzielnika napięcia składającego się z rezystorów R3 i R4. Cały układ zasilany jest napięciem 5 V, więc napięcie odniesienia wynosi 2,5 V (ze wzoru Uwyj = Uzas * (R4 / R3 + R4). Dioda LED1 zaświeci się wtedy, gdy napięcie na wyjściu będzie równe 0 V. Na pierwszy rzut oka jest to może mało intuicyjne, ale zwróć uwagę, że anoda diody podłączona jest bezpośrednio do „+” zasilania, a katoda do wyjścia komparatora. Aby się zaświeciła musimy wygenerować różnicę potencjałów pomiędzy jej anodą, a katodą.

Aby wygenerować na wyjściu komparatora napięcie równe 0 V, to na wejściu nieodwracającym (pin 2) musimy przyłożyć napięcie niższe niż odniesienia, czyli <2,5 V. W tym celu znów korzystam z dzielnika napięcia, składającego się z fotorezystora R7 i potencjometru P1. Wartość napięcia wyprowadzonego z tego dzielnika wynika ze wzoru:

$$ Uwyj = Uzas * \frac{P1}{R7+P1} $$

Dzięki takiemu podłączeniu fotorezystora R7, że pojawia się tylko w mianowniku wzoru, to wraz ze wzrostem jego rezystancji maleje napięcie Uwyj, czyli to które porównywane jest z napięciem odniesienia. Podłączenie potencjometru P1 tak, że występuje zarówno w liczniku jak i mianowniku wzoru umożliwia regulację układu. Im mniejsza jest jego rezystancja, tym mniejsza może być rezystancja R7, aby dioda się zaświeciła.

W poniższej tabeli znajdziesz różne zestawienia parametrów obrazujące działanie całego czujnika.

{{<table>}}
| Pora dnia       | Uzas [V] | R7 [kΩ] | P1 [kΩ] | Uwyj [V] | LED1 |
| --------------- | -------- | ------- | ------- | -------- | ---- |
| Rano            | 5,0      | 2,0     | 6,0     | 3,75     | OFF  |
| Przed południem | 5,0      | 4,0     | 6,0     | 3,00     | OFF  |
| Po południu     | 5,0      | 6,0     | 6,0     | 2,50     | ON   |
| Wieczorem       | 5,0      | 10,0    | 10,0    | 2,50     | ON   |
| Noc             | 5,0      | 15,0    | 10,0    | 2,00     | ON   |
{{</table>}}

Ostatnim elementem, na który warto zwrócić uwagę w tym układzie jest sprzężenie zwrotne, czyli podłączenie wyjścia i wejścia komparatora za pośrednictwem rezystora R6. Jego zadaniem jest jeszcze skuteczniejsze wyeliminowanie stanów pośrednich na wyjściu układu, dzięki czemu generowane sygnały będzie można jednoznacznie interpretować jako logiczne „1” (jest napięcie) lub „0” (brak napięcia).