---
title: "dusk sensor"
subtitle: "how to build dusk sensor with voltage comparator LM 311"
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
customCSS:
- katex.css
customJS:
- katex.js
---
**A dusk sensor is a circuit that is designed to perform a planned action (e.g. turning on an LED) when the light intensity drops to a specific level.**
<!--more-->
The dusk sensor itself is unlikely to be used in my collaborative robot. However, I treated this circuit as an exercise in using other analogue detectors and an attempt to translate the data received by them into control signals for the robot. At this stage, I imagine, for example, installing a proximity sensor that would emergency-stop the robot when a human limb appears in its field of action.

Although versions 1.0 and 2.0 of the dusk sensor are not too complicated, it took me a while to understand the principle of the voltage divider and to select the appropriate resistor values. Moreover, during the tests, it turned out that photoresistors with theoretically the same parameters can have different resistance values under the same conditions (i.e., the same lighting). Therefore, I added a potentiometer to each model, which allows for adjusting the sensitivity of the circuit.

## dusk sensor v. 1.0

{{<image src="dusk-sensor-v10-20201205-bb.webp" caption="dusk sensor v. 1.0 – visualisation">}}
{{<image src="dusk-sensor-v10-20201205-scheme.webp" caption="dusk sensor v. 1.0 – schematic">}}
{{<image src="dusk-sensor-v10-20201205-photo.webp" caption="dusk sensor v. 1.0 – photo">}}

In the first version of my dusk sensor, the controlling element is a standard NPN transistor. The transistor conducts current in the collector-emitter line only when a voltage greater than approximately 0.6 V is applied to its base. The light intensity sensor is a photoresistor, whose resistance increases as it gets darker. In other words, I had to arrange the components in such a way that as the resistance on the photoresistor increased (as dusk fell), the voltage at the transistor's base increased, and as a result, the diode connected to the collector lit up.

To achieve this, I used a voltage divider consisting of:
* 1 kΩ resistor (R1),
* 50 kΩ potentiometer (R2),
* Photoresistor (R3).

The value of the voltage supplied to the base of transistor Q1 results from the formula:

$$ Uout = Uin * \frac{R3}{R1+R2+R3} $$

Thanks to such a connection of the photoresistor R3, which appears both in the numerator and denominator of the formula, as its resistance increases, so does the output voltage Uwyj, which is the voltage applied to the base of transistor Q1.

Connecting the potentiometer R2 exclusively in the denominator enables simple adjustment of the circuit. The higher its resistance, the higher the value of R3 must be for Uwyj to exceed the threshold value. In other words - it must be darker for the diode to light up.

In the table below, you will find different parameter combinations illustrating the operation of the entire sensor.

{{<table>}}
| time of day | Uin [V] | R3 [kΩ] | R1 [kΩ] | R2 [kΩ] | Uout [V] | LED1 |
| ----------- | ------- | ------- | ------- | ------- | -------- | ---- |
| morning     | 3,3     | 2,0     | 1,0     | 25,0    | 0,24     | OFF  |
| before noon | 3,3     | 4,0     | 1,0     | 25,0    | 0,44     | OFF  |
| afternoon   | 3,3     | 6,0     | 1,0     | 25,0    | 0,62     | ON   |
| evening     | 3,3     | 10,0    | 1,0     | 50,0    | 0,54     | OFF  |
| night       | 3,3     | 15,0    | 1,0     | 50,0    | 0,75     | ON   |
{{</table>}}

Now for an interesting fact. According to the aforementioned model, after turning the potentiometer R2 to zero and activating the sensor at night, a voltage > 3 V should be accumulated at the base, which would probably result in transistor damage. In reality, it amounts to 0.75 V – the transistor is saturated, the diode is lit, everything works, and nothing burns. I searched for an explanation of this phenomenon and found an interesting thread on elektroda.pl, which I do not want to delve into too deeply at the moment.

The dusk sensor version 1.0 is simple but functional. However, it has one fundamental flaw – the output signal (in this case, the glowing LED1 diode) is analogue. As it gets progressively darker, the diode gradually shines brighter. In the case of my robot (or digital electronics in general), a binary signal would be more useful. If it is bright, the diode does not light up; if it is dark, it does. No intermediate states in the style of "somewhat bright – somewhat lit".

## dusk sensor v. 2.0

{{<image src="dusk-sensor-v20-20201211-bb.webp" caption="dusk sensor v. 2.0 – visualisation">}}
{{<image src="dusk-sensor-v20-20201211-scheme.webp" caption="dusk sensor v. 2.0 – schematic">}}
{{<image src="dusk-sensor-v20-20201211-photo.webp" caption="dusk sensor v. 2.0 – photo">}}

To eliminate the biggest flaw of the first version of my dusk sensor, in the second approach, I used a voltage comparator LM 311. This circuit compares the voltage values applied to the so-called non-inverting input (pin 2 and "+" on the schematic) and the inverting input (pin 3 and "-" on the schematic). The result of this comparison is the voltage at the output of the circuit (pin 7):
* if the voltage at the non-inverting input is higher than at the inverting input, the voltage at the output is equal to the supply voltage (if pin 2 > pin 3, then Uout = VCC).
* if the voltage at the non-inverting input is lower than at the inverting input, the voltage at the output is equal to zero (if pin 2 < pin 3, then Uout = 0).

In my circuit, the reference point is the inverting input (pin 3), to which I apply the voltage from the voltage divider consisting of resistors R3 and R4. The whole circuit is powered by a 5 V voltage, so the reference voltage is 2.5 V (using the formula Uout = Usup * (R4 / R3 + R4). The LED1 diode will light up when the voltage at the output is 0 V. At first glance, this may seem counterintuitive, but note that the diode's anode is directly connected to the "+" of the power supply, and the cathode to the comparator output. To light it up, we need to generate a potential difference between its anode and cathode.

To generate a 0 V voltage at the comparator output, we must apply a voltage lower than the reference voltage at the non-inverting input (pin 2), i.e., < 2.5 V. To do this, I once again use a voltage divider consisting of photoresistor R7 and potentiometer P1. The voltage value obtained from this divider is derived from the formula:

$$ Uout = Uin * \frac{P1}{R7+P1} $$

Thanks to such a connection of the photoresistor R7, which appears only in the denominator of the formula, the voltage Uout decreases as its resistance increases, meaning the voltage that is compared with the reference voltage. Connecting the potentiometer P1 so that it appears in both the numerator and the denominator of the formula allows for system adjustment. The lower its resistance, the lower the resistance of R7 can be for the diode to light up.

In the table below, you will find various parameter sets illustrating the operation of the entire sensor.

{{<table>}}
| time of day | Uin [V] | R7 [kΩ] | P1 [kΩ] | Uout [V] | LED1 |
| ----------- | ------- | ------- | ------- | -------- | ---- |
| morning     | 5,0     | 2,0     | 6,0     | 3,75     | OFF  |
| before noon | 5,0     | 4,0     | 6,0     | 3,00     | OFF  |
| afternoon   | 5,0     | 6,0     | 6,0     | 2,50     | ON   |
| evening     | 5,0     | 10,0    | 10,0    | 2,50     | ON   |
| night       | 5,0     | 15,0    | 10,0    | 2,00     | ON   |
{{</table>}}

The last element worth noting in this circuit is the feedback loop, which involves connecting the comparator's output and input via resistor R6. Its purpose is to more effectively eliminate intermediate states at the circuit output, allowing the generated signals to be unambiguously interpreted as logical "1" (voltage present) or "0" (no voltage).