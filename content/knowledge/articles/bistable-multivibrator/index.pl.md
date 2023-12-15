---
title: "multiwibrator dwustanowy bistabilny"
shortTitle: "przerzutnik bistabilny"
subtitle: "układ do manualnego generowania sygnału prostokątnego"
url: baza-wiedzy/artykuly/multiwibrator-dwustanowy-bistabilny/
categories:
- article
tags:
- electronics
image:
    name: bistable-multivibrator 
    extension: svg
    height: 96
    width: 96
readMore: true
publishDate: 2020-12-27
customCSS:
- katex.css
customJS:
- katex.js
---
**Multiwibrator dwustanowy (przerzutnik) bistabilny to układ elektroniczny, który pozwala na manualne wygenerowanie na wyjściu sygnału prostokątnego.**
<!--more-->
Przerzutnik bistabilny posiada 2 wyjścia. Gdy na jednym występuje stan wysoki (logiczna „1”) to na drugim jest stan niski (logiczne „0”) i na odwrót. Nazwa układu pochodzi właśnie od dwóch stanów stabilnych („jedynek”), które pojawiają się naprzemiennie w trakcie jego pracy.

Co prawda, wygenerowanie użytecznego sygnału sterującego za pomocą ręcznego przełączania włączników jest raczej niemożliwe, jednak analiza działania tego układu pomogła mi w zrozumieniu zasady działania opisanego w osobnym artykule multiwibratora astabilnego.

> Cały czas się uczę, więc dla doświadczonych specjalistów moje opisy mogą być zbyt uproszczone, a miejscami wręcz infantylne. Zastrzegam też sobie prawo do błędów.
> 
> Wniosek: krytykuj, ale nie hejtuj; czytaj, ale raczej nie cytuj :)

## Multiwibrator bistabilny – schematy

{{<image src="bistable-multivibrator-20201227-bb.webp" caption="Multiwibrator dwustanowy bistabilny – wizualizacja">}}
{{<image src="bistable-multivibrator-20201227-scheme.webp" caption="Multiwibrator dwustanowy bistabilny – schemat">}}
{{<image src="bistable-multivibrator-20201227-photo.webp" caption="Multiwibrator dwustanowy bistabilny – fotografia">}}

## Multiwibrator bistabilny – opis działania

### Wyjścia układu

Wyjścia multiwibratora dwustanowego bistabilnego, na którym odkładają się sygnały o przebiegu prostokątnym, znajdują się na złączach kolektor-emiter (CE) tranzystorów Q1 oraz Q2. Dla lepszego zwizualizowania efektów działania układu podłączyłem do nich po jednej diodzie LED.

Dioda LED1 przewodzi (świeci), gdy tranzystor Q1 jest zatkany, a Q2 nasycony. Odpowiednio, dioda LED2 przewodzi, gdy tranzystor Q2 jest zatkany, a Q1 nasycony. Na pierwszy rzut oka takie krzyżowe generowanie sygnałów może być mylące, jednak wytłumaczenie tego zjawiska jest proste.

Jeśli popatrzymy chwilę dłużej na schemat to dostrzeżemy, że tranzystor Q1 oraz rezystor R1 (oraz odpowiednio Q2 i R4) tworzą razem dzielnik napięcia. Jeśli dany tranzystor jest zatkany (nie przewodzi) to zachowuje się jak opornik o bardzo dużej rezystancji. Wówczas na jego złączach odkłada się napięcie wynikające z poniższego wzoru:

$$ Uwyj = Uwej * \frac{Q1}{R1+Q1} $$

Napięcie odłożone na złączach kolektor-emiter tranzystora sprawia, że podłączona do nich dioda LED świeci. Analogicznie – gdy tranzystor nie przewodzi to zachowuje się jak opornik o małej rezystancji, na którego złączach odkłada się jedynie minimalne napięcie – dioda wtedy nie świeci.

### Działanie krok po kroku

W idealnych warunkach (gdyby wszystkie komponenty charakteryzowały się tymi samymi parametrami) tak skonstruowany przerzutnik bistabilny nie miałby prawa działać. W danej chwili oba tranzystory musiałyby jednocześnie przewodzić i nie przewodzić, co jest raczej niemożliwe. Poszczególne elementy układu różnią się jednak nieznacznie od siebie, dzięki czemu po uruchomieniu układu jeden tranzystor zaczyna przewodzić szybciej, niż drugi.

W zbudowanym przeze mnie modelu szybszy okazał się tranzystor Q2, więc od niego zaczniemy.

#### Faza 1

- Oba przyciski rozwarte,
- Do bazy tranzystora Q2 płynie prąd przez R1 i R2 – Q2 przewodzi,
- Q1 jest zatkany – dioda LED1 świeci.

{{<image src="bistable-multivibrator-20201227-mode-1.webp" caption="Multiwibrator dwustanowy bistabilny – działanie – faza 1">}}

#### Faza 2

- Przycisk S2 zwarty – prąd „skraca drogę” i nie dopływa do bazy Q2,
- Q2 jest zatkany – prąd zaczyna płynąć do bazy Q1 przez R4 i R3.

{{<image src="bistable-multivibrator-20201227-mode-2.webp" caption="Multiwibrator dwustanowy bistabilny – działanie – faza 2">}}

#### Faza 3

- Oba przyciski rozwarte,
- Do bazy Q1 płynie prąd przez R4 i R3 – Q1 przewodzi,
- Q2 jest zatkany – dioda LED2 świeci.

{{<image src="bistable-multivibrator-20201227-mode-3.webp" caption="Multiwibrator dwustanowy bistabilny – działanie – faza 3">}}

#### Faza 4

- Przycisk S1 zwarty – prąd „skraca drogę” i nie dopływa do bazy Q1,
- Q1 jest zatkany – prąd zaczyna płynąć do bazy Q2 przez R1 i R2.

{{<image src="bistable-multivibrator-20201227-mode-4.webp" caption="Multiwibrator dwustanowy bistabilny – działanie – faza 4">}}

Kolejna faza jest powtórzeniem fazy 1, więc w tym miejscu następuje koniec cyklu pracy układu (który oczywiście można powtarzać dowolną liczbę razy).