---
title: "dusk sensor"
subtitle: "how to build dusk sensor with voltage comparator LM 311"
categories:
- article
tags:
- electronics
publishDate: 2020-12-06
---

A dusk sensor is a circuit that is designed to perform a planned action (e.g. turning on an LED) when the light intensity drops to a specific level.

The dusk sensor itself is unlikely to be used in my collaborative robot. However, I treated this circuit as an exercise in using other analogue detectors and an attempt to translate the data received by them into control signals for the robot. At this stage, I imagine, for example, installing a proximity sensor that would emergency-stop the robot when a human limb appears in its field of action.

Although versions 1.0 and 2.0 of the dusk sensor are not too complicated, it took me a while to understand the principle of the voltage divider and to select the appropriate resistor values. Moreover, during the tests, it turned out that photoresistors with theoretically the same parameters can have different resistance values under the same conditions (i.e., the same lighting). Therefore, I added a potentiometer to each model, which allows for adjusting the sensitivity of the circuit.

## dusk sensor v. 1.0