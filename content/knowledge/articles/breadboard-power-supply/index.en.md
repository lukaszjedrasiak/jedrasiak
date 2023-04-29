---
title: "breadboard power supply"
subtitle: "how to build the power supply system for prototyping boards"
categories:
- article
tags:
- eletronics
publishDate: 2020-11-11
---

**A power supply circuit for breadboards allows for the provision of the appropriate voltage to the remaining electronic circuits being built.**

The main component of the power supply circuit for breadboards is a Graetz bridge (protection against reverse connection of the DC voltage source, e.g. battery contacts) and a voltage regulator (maintaining a constant output value of the desired voltage).

The described version 1.0 of the circuit is also the first electronic circuit I have built, which finds practical application. It is not perfect, but it is sufficient at the initial stage of learning electronics.

Subsequent versions of the power supply circuit will be supplemented with:
* An on-off switch instead of a tact switch button,
* A diode indicating power connection (instead of a diode lighting up when the circuit is turned on with the button),
* A DC connector for connecting a 12V socket power supply,
* A USB connector (if it turns out to be useful during use),
* An output voltage of 3.3V.

The final version of the circuit will be assembled on a printed circuit board equipped with pin connectors, allowing the module to be plugged directly into a prototype breadboard.