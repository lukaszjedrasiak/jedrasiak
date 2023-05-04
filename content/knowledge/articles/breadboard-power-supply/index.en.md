---
title: "breadboard power supply"
subtitle: "how to build the power supply system for prototyping boards"
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

**A power supply circuit for breadboards allows for the provision of the appropriate voltage to the remaining electronic circuits being built.**
<!--more-->
The main component of the power supply circuit for breadboards is a Graetz bridge (protection against reverse connection of the DC voltage source, e.g. battery contacts) and a voltage regulator (maintaining a constant output value of the desired voltage).

The described version 1.0 of the circuit is also the first electronic circuit I have built, which finds practical application. It is not perfect, but it is sufficient at the initial stage of learning electronics.

Subsequent versions of the power supply circuit will be supplemented with:
* An on-off switch instead of a tact switch button,
* A diode indicating power connection (instead of a diode lighting up when the circuit is turned on with the button),
* A DC connector for connecting a 12V socket power supply,
* A USB connector (if it turns out to be useful during use),
* An output voltage of 3.3V.

The final version of the circuit will be assembled on a printed circuit board equipped with pin connectors, allowing the module to be plugged directly into a prototype breadboard.

## Version 1.0

{{<image src="breadboard-power-supply-v10-20201114-bb.webp" caption="breadboard power supply v. 1.0 – visualisation">}}
{{<image src="breadboard-power-supply-v10-20201114-scheme.webp" caption="breadboard power supply v. 1.0 – schematic">}}
{{<image src="breadboard-power-supply-v10-20201114-photo.webp" caption="breadboard power supply v. 1.0 – photo">}}

The first version of my breadboard power supply circuit was almost entirely taken from Paweł Borkowski's book titled "Przygoda z elektroniką" (Adventure with Electronics). My only contribution was adding a switch and a diode to ensure all contacts were in the right places. I also replaced capacitor C3. Instead of a 470 μF, I connected a 220 μF capacitor, as that was all I had in stock and I didn't want to connect multiple ones to avoid complicating the visualisation and the photo.

To construct the circuit (a reminder - this is my first reasonably functioning circuit), I approached it with high ambition. I tried not to rely on the illustrations provided in the book and used only the schematic diagram. Additionally, to illustrate this post with something more than just a photograph, I mastered the Fritzing software and adopted a preliminary schematic for describing and archiving my subsequent projects.

### List of components

* D1-D4: 1N4148 rectifier diode,
* C1-C2: 100 nF ceramic capacitor,
* C3: 470 μF electrolytic capacitor,
* C4: 100 μF electrolytic capacitor,
* R1: 220 Ω resistor,
* 5 V 7805 voltage regulator,
* LED diode,
* tact-switch button or any other switch.

### How it works

The input voltage source is a 9 V battery connected to the breadboard's row 1 terminals. With the use of a Graetz bridge, battery polarity (order of connecting poles) doesn't matter. It is only important that one pole is connected to one side of the board (terminals 1A-1E) and the other pole to the other side (terminals 1F-1I).

The next component of the circuit is the voltage regulator, which is responsible for lowering the input voltage to the expected value (in this case, 5 V) and maintaining it at a constant level, regardless of the load applied at the output. The accompanying capacitors help prevent the regulator from oscillating or generating unwanted pulses. At the time of creating this article, I didn't delve deeper into what this phenomenon actually entails.

### Troubleshooting

During the initial tests of the circuit, it turned out that the output voltage was far from what I could call "stable". So far, I have identified two problems:

1. Too low input voltage. For proper operation, each voltage regulator requires an appropriate margin between the input and output voltage (known as dropout). In the case of the 7805 regulator used, this difference should be at least 2 V, which means that the minimum input voltage must be higher than 7 V. Additionally, the Graetz bridge causes a voltage drop of approximately 1.4 V (two rectifier diodes × 0.7 V each), so we need to record at least 8.4 V at the battery terminals. In other words, for the proper operation of the circuit in version 1.0, I would need a considerable stock of factory-new cells. Therefore, I am considering two options:
   * equipping the power supply circuit with a mains power supply connector,
   * removing the Graetz bridge :).

2. Damaged components. In my case – after spending several hours checking and replacing each component separately – the faulty part turned out to be the breadboard. After moving all the components to a new board, the multimeter readings at the output stopped at a satisfying value of 5.01 V.

## Version 2.0

{{<image src="breadboard-power-supply-v20-20201211-bb.webp" caption="breadboard power supply v. 2.0 – visualisation">}}
{{<image src="breadboard-power-supply-v20-20201211-scheme.webp" caption="breadboard power supply v. 2.0 – schematic">}}
{{<image src="breadboard-power-supply-v20-20201211-photo.webp" caption="breadboard power supply v. 2.0 – photo">}}

As planned, the second version of my breadboard power supply circuit does not differ in its operating principle from the first model. The most important modification was adding a 3.3 V voltage regulator (LD1117V33). Along the way, I learned that after purchasing new components, it is always necessary to familiarize yourself with their documentation. In the case of this component, it turned out that the pinout scheme differs from the one used in the 7805 model.

In addition, I added to the circuit:
* A 5.5/2.5 mm DC socket, allowing the circuits to be powered by a 12 V mains power supply,
* A sliding switch (instead of a push switch).

I also changed the location of the LED diodes, which now signal the correct operation of each regulator separately. The 5 V regulator is connected to the lower rail of the breadboard, while the 3.3 V is connected to the upper rail. This allows me to use both voltage levels simultaneously.

I am still wondering if connecting the regulators in series is correct. The choice of such a solution is due to the operating principle of these components, which convert the "excess" power into thermal energy when lowering the voltage. For example, consider a device powered by a 100 mA current when connected to the 3.3 V rail. The power it consumes will be 0.33 W (P = U × I). The power consumed by the 12 V power supply will be 1.2 W. The difference between 1.2 W and 0.33 W will be dissipated by the regulator and converted into heat. In a parallel connection, all the wasted power would be emitted by one regulator (as a result, the component would heat up significantly), whereas in a series connection, it is distributed across two components.

For a moment, I was convinced that life is a never-ending art of compromise and I simply have to accept the additional heat source on my desk. As a precaution, I even mounted the visible heat sinks in the photo. In the meantime, however, I read about switching converters, whose power losses due to voltage reduction are only about 10%.

This means only one thing – the breadboard power supply circuit will have at least one more version.