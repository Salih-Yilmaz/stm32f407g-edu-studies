# 05 - ADC Read DMA

This study focuses on reading an analog voltage using ADC with DMA on the STM32F407G Discovery board.

## Objective

The objective of this project is to understand the difference between ADC polling and ADC DMA.

In the previous ADC polling project, the CPU started the ADC conversion, waited for the conversion to complete, and then read the ADC value manually.

In this project, ADC1 reads the analog voltage from PA1 and DMA transfers the ADC result directly into memory. This allows the ADC value to be updated in the background without continuously polling the ADC inside the while loop.

## Main Idea

The system works as follows:

Potentiometer changes the voltage on PA1.

ADC1 converts this analog voltage into a digital value between 0 and 4095.

DMA transfers the ADC result into adcDmaBuffer[0].

The latest ADC value is copied into adcDmaValue inside the conversion complete callback.

adcDmaConversionCount increases each time an ADC conversion is completed.

## Topics to Learn

* ADC with DMA
* Direct Memory Access
* Continuous ADC conversion
* DMA circular mode
* DMA continuous requests
* ADC conversion complete callback
* Buffer usage
* Live Expressions in STM32CubeIDE
* Difference between polling and DMA

## Hardware

* STM32F407G Discovery Board
* USB cable
* Jumper wires
* Potentiometer

## Pin Configuration

| Pin | Function | Description |
| --- | -------- | ----------- |
| PA1 | ADC1_IN1 | Potentiometer analog output |

## Potentiometer Connection

| Potentiometer Pin | STM32F407G Discovery Connection |
| ----------------- | ------------------------------- |
| One outer pin | 3V |
| Middle pin | PA1 / ADC1_IN1 |
| Other outer pin | GND |

The middle pin of the potentiometer must be connected to PA1.

## Important STM32CubeIDE Settings

ADC1 settings:

* Resolution: 12 bits
* Continuous Conversion Mode: Enabled
* DMA Continuous Requests: Enabled
* External Trigger Conversion Source: Software Start
* External Trigger Conversion Edge: None

DMA settings:

* DMA Request: ADC1
* Direction: Peripheral to Memory
* Mode: Circular
* Peripheral Increment: Disabled
* Memory Increment: Enabled
* Peripheral Data Width: Half Word
* Memory Data Width: Half Word

## Core User Code Sections

The buffer size is defined in the Private Define section:

`#define ADC_DMA_BUFFER_SIZE 1`

The DMA buffer and observation variables are defined in the Private Variables section:

`volatile uint16_t adcDmaBuffer[ADC_DMA_BUFFER_SIZE] = {0};`

`volatile uint16_t adcDmaValue = 0;`

`volatile uint32_t adcDmaConversionCount = 0;`

The ADC DMA process is started once before the main loop:

`HAL_ADC_Start_DMA(&hadc1, (uint32_t*)adcDmaBuffer, ADC_DMA_BUFFER_SIZE);`

The ADC conversion complete callback is used to observe the DMA update process:

`adcDmaValue = adcDmaBuffer[0];`

`adcDmaConversionCount++;`

## Code Logic

The ADC and DMA are started once before the infinite loop.

After that, the ADC keeps converting the analog voltage from PA1.

DMA automatically transfers the ADC result into adcDmaBuffer[0].

The CPU does not need to call HAL_ADC_PollForConversion or HAL_ADC_GetValue repeatedly inside the while loop.

The callback function runs whenever an ADC conversion is completed.

## Polling vs DMA

Polling:

The CPU starts the ADC conversion, waits until the conversion is finished, reads the value, and repeats this process manually.

DMA:

The ADC conversion result is transferred automatically to memory by DMA. The CPU does not need to wait for each conversion manually.

## What I Observed

* The ADC value changed when the potentiometer was rotated.
* adcDmaBuffer[0] was updated by DMA.
* adcDmaValue showed the latest ADC value.
* adcDmaConversionCount increased continuously when DMA continuous requests and circular mode were enabled.
* When DMA Continuous Requests was disabled, the ADC value was transferred only once.

## What I Learned

* DMA stands for Direct Memory Access.
* DMA can transfer peripheral data directly into memory.
* ADC can work with DMA to update a buffer automatically.
* DMA circular mode allows the same buffer to be updated continuously.
* DMA Continuous Requests must be enabled for continuous ADC-DMA operation.
* Callback functions can be used to observe when ADC conversions are completed.
* DMA reduces the need for CPU-based waiting compared to polling.

## Safety Notes

* PA1 must not receive more than 3.3V.
* 5V must not be connected to the ADC input.
* 3V and GND must not be shorted.
* Connections should be checked before powering the board.
* If jumper wires become loose, ADC readings may become unstable.

## Project Structure

* README.md: project documentation
* project/: STM32CubeIDE project source files

## Reference

This project was developed after studying the ADC polling and ADC DMA concepts. The Karpuz.co ADC DMA example was used as a reference, but this implementation was recreated and documented as a personal STM32F407G Discovery learning project.
