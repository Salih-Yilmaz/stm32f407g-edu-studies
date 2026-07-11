\# 03 - ADC Read Polling



This study focuses on reading an analog voltage using the ADC peripheral of the STM32F407G Discovery board.



\## Objective



The objective of this study is to understand how an analog voltage applied to an STM32 pin can be converted into a digital value using ADC.



In this project, a new STM32CubeIDE project was created. The ADC was configured to read the PA1 pin as an analog input, and the ADC value was observed in real time using Live Expressions during debugging.



\## Topics to Learn



\* ADC basics

\* Analog input reading

\* GPIO analog mode

\* ADC channel configuration

\* HAL\_ADC\_Start

\* HAL\_ADC\_PollForConversion

\* HAL\_ADC\_GetValue

\* HAL\_ADC\_Stop

\* Polling method

\* Live Expressions in STM32CubeIDE



\## Hardware



\* STM32F407G Discovery Board

\* USB cable

\* Jumper wires

\* Potentiometer



\## Pin Configuration



| Pin | Function | Description |

| --- | -------- | ----------- |

| PA1 | ADC1\_IN1 | Analog input |



\## Target Behavior



The voltage applied to PA1 is converted into a digital ADC value.



Expected values:



| PA1 Voltage | ADC Value |

| ---------- | --------- |

| 0V / GND   | Around 0  |

| 3V / 3.3V  | Around 4095 |

| Variable voltage from potentiometer | Between 0 and 4095 |



\## Core Code Logic



```c

volatile uint32\_t adcValue = 0;

```



The `adcValue` variable stores the latest ADC conversion result.



```c

while (1)

{

&#x20; HAL\_ADC\_Start(\&hadc1);



&#x20; if (HAL\_ADC\_PollForConversion(\&hadc1, 100) == HAL\_OK)

&#x20; {

&#x20;   adcValue = HAL\_ADC\_GetValue(\&hadc1);

&#x20; }



&#x20; HAL\_ADC\_Stop(\&hadc1);



&#x20; HAL\_Delay(100);

}

```



\## Code Explanation



`HAL\_ADC\_Start(\&hadc1)` starts ADC1 conversion.



`HAL\_ADC\_PollForConversion(\&hadc1, 100)` waits until the ADC conversion is completed. The value `100` means that the program waits up to 100 ms.



If the conversion is completed successfully, the function returns `HAL\_OK`.



`HAL\_ADC\_GetValue(\&hadc1)` reads the converted ADC value and stores it in the `adcValue` variable.



`HAL\_ADC\_Stop(\&hadc1)` stops the ADC after the conversion.



`HAL\_Delay(100)` creates a short delay before the next measurement.



\## Polling Logic



This project uses polling.



Polling means that the microcontroller repeatedly checks whether the ADC conversion is completed.



In this project, the STM32 continuously starts an ADC conversion, waits for it to complete, reads the value, and repeats this process inside the `while(1)` loop.



\## What I Observed



\* When PA1 was connected to GND, the ADC value was around 0.

\* When PA1 was connected to 3V, the ADC value reached 4095.

\* When a potentiometer was connected, the ADC value changed between low and high values as the potentiometer was rotated.

\* The ADC value was observed in real time using Live Expressions in STM32CubeIDE.



\## What I Learned



\* Analog voltage can be converted into a digital value using ADC.

\* A 12-bit ADC gives values between 0 and 4095.

\* PA1 can be configured as ADC1\_IN1.

\* `HAL\_ADC\_Start()` starts ADC conversion.

\* `HAL\_ADC\_PollForConversion()` waits for the conversion to complete.

\* `HAL\_ADC\_GetValue()` reads the converted value.

\* Live Expressions can be used to observe variables while the program is running.

\* Floating analog pins may produce unstable values if they are not connected to a defined voltage.



\## Safety Notes



\* The ADC input voltage must stay between 0V and 3.3V.

\* 5V must not be applied to PA1.

\* 3V and GND should not be shorted.

\* Connections should be checked before powering the board.



\## Project Structure



```text

03-adc-read-polling/

&#x20; README.md

&#x20; project/

```



The `project/` folder contains the STM32CubeIDE project source files.



\## Reference



This study was inspired by the Karpuz.co EDU002 ADC read polling example. The implementation in this repository was recreated and tested as a personal STM32F407G Discovery learning project.

