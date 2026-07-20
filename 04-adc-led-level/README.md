# 04 - ADC LED Level Indicator

This study combines analog input reading and digital output control on the STM32F407G Discovery board.

## Objective

The objective of this study is to read an analog voltage using ADC and control the onboard LEDs according to the ADC value.

In this project, a potentiometer is connected to PA1. The STM32 reads the analog voltage from the potentiometer and converts it into a digital value between 0 and 4095. According to this value, different numbers of onboard LEDs are turned on.

## Main Idea

The system works as follows:

Potentiometer position changes the voltage on PA1.

The ADC converts this voltage into a digital value.

The software checks the adcValue range.

The STM32 turns on 1, 2, 3, or 4 LEDs according to the ADC value.

## Topics to Learn

* ADC reading
* Polling method
* GPIO output control
* Analog input to digital output logic
* Conditional statements
* Threshold-based decision making
* Live Expressions in STM32CubeIDE
* Basic embedded system behavior

## Hardware

* STM32F407G Discovery Board
* USB cable
* Jumper wires
* Potentiometer

## Pin Configuration

| Pin | Function | Description |
| --- | -------- | ----------- |
| PA1 | ADC1_IN1 | Potentiometer analog output |
| PD12 | GPIO_Output | Yellow LED |
| PD13 | GPIO_Output | Orange LED |
| PD14 | GPIO_Output | Red LED |
| PD15 | GPIO_Output | Blue LED |

## Potentiometer Connection

| Potentiometer Pin | STM32F407G Discovery Connection |
| ----------------- | ------------------------------- |
| One outer pin | 3V |
| Middle pin | PA1 / ADC1_IN1 |
| Other outer pin | GND |

The middle pin of the potentiometer must be connected to PA1.

## Target Behavior

| ADC Value Range | LED Behavior |
| --------------- | ------------ |
| 0 - 1023 | 1 LED on |
| 1024 - 2047 | 2 LEDs on |
| 2048 - 3071 | 3 LEDs on |
| 3072 - 4095 | 4 LEDs on |

## Core Code Logic

The ADC value is stored in this variable:

`volatile uint32_t adcValue = 0;`

The main loop performs these steps repeatedly:

1. Start ADC conversion with `HAL_ADC_Start(&hadc1);`
2. Wait for conversion using `HAL_ADC_PollForConversion(&hadc1, 100);`
3. Read the ADC value using `HAL_ADC_GetValue(&hadc1);`
4. Stop ADC using `HAL_ADC_Stop(&hadc1);`
5. Turn off all LEDs
6. Check the ADC value range
7. Turn on LEDs according to the measured level
8. Wait 100 ms and repeat

## LED Decision Logic

If the ADC value is less than 1024, only the yellow LED is turned on.

If the ADC value is between 1024 and 2047, the yellow and orange LEDs are turned on.

If the ADC value is between 2048 and 3071, the yellow, orange, and red LEDs are turned on.

If the ADC value is 3072 or higher, all four LEDs are turned on.

## What I Observed

* The ADC value changed when the potentiometer was rotated.
* The number of active LEDs changed according to the ADC value.
* Low potentiometer values turned on fewer LEDs.
* High potentiometer values turned on more LEDs.
* The STM32 successfully converted an analog input into a visible digital output behavior.

## What I Learned

* ADC values can be used not only for reading data but also for making decisions.
* A 12-bit ADC produces values between 0 and 4095.
* The ADC range can be divided into threshold regions.
* GPIO outputs can be controlled according to sensor or analog input values.
* This project demonstrates a basic embedded system structure: input, processing, and output.

## Safety Notes

* PA1 must not receive more than 3.3V.
* 5V must not be connected to the ADC input.
* 3V and GND must not be shorted.
* Connections should be checked before powering the board.
* If jumper wires become loose, the ADC value may become unstable.

## Project Structure

* README.md: project documentation
* project/: STM32CubeIDE project source files

## Reference

This project was developed after studying the ADC read polling concept. The Karpuz.co ADC read polling example was used as a reference, but this project was recreated and extended as a personal STM32F407G Discovery learning project.
