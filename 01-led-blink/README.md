# 01 - LED Blink

This study is my first GPIO output application on the STM32F407G Discovery board.

## Objective

To learn STM32CubeIDE, project creation, code uploading, and basic GPIO usage by blinking the onboard LED at specific time intervals.

## Topics to Learn

* STM32CubeIDE project creation

* GPIO Output

* HAL_GPIO_WritePin

* HAL_Delay

* main.c structure

* Build and Debug/Run operations

## Hardware

* STM32F407G Discovery Board

* USB cable

## First Application Result

The Karpuz.go EDU001 blinkLED project was imported into STM32CubeIDE. The project was built and uploaded to the STM32F407G Discovery board. When the code was executed, the onboard blue LED was observed blinking.

## Core Code Logic

```c
while (1)
{
  HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_15);
  HAL_Delay(500);
  MX_USB_HOST_Process();
}
```

In this code, the `HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_15)` function toggles the state of pin 15 on GPIO port D. Since the blue LED is connected to this pin, it turns off if it is on and turns on if it is off.

The `HAL_Delay(500)` function creates a 500 ms delay. When the delay value is changed, the blinking speed of the LED also changes.

## Experiments

* With `HAL_Delay(500)`, the LED changed state approximately every 0.5 seconds.

* With `HAL_Delay(5000)`, the LED changed state much more slowly, approximately every 5 seconds.

## What I Learned

* A ready-made project was imported into STM32CubeIDE.

* The project was built successfully.

* The code was uploaded to the STM32F407G board using Debug/Run.

* It was observed that the `while(1)` infinite loop is the main execution structure in embedded systems.

* It was observed that the behavior of a physical LED can be changed by controlling a GPIO pin through software.

## Sequential LED Experiment

In this experiment, the onboard LEDs were controlled sequentially using `HAL_GPIO_WritePin()`.

```c
while (1)
{
  HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_SET);
  HAL_Delay(500);
  HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOD, GPIO_PIN_13, GPIO_PIN_SET);
  HAL_Delay(500);
  HAL_GPIO_WritePin(GPIOD, GPIO_PIN_13, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOD, GPIO_PIN_14, GPIO_PIN_SET);
  HAL_Delay(500);
  HAL_GPIO_WritePin(GPIOD, GPIO_PIN_14, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOD, GPIO_PIN_15, GPIO_PIN_SET);
  HAL_Delay(500);
  HAL_GPIO_WritePin(GPIOD, GPIO_PIN_15, GPIO_PIN_RESET);

  MX_USB_HOST_Process();
}
```

The LEDs blinked sequentially in the following order:

| GPIO Pin    | LED Color |
| ----------- | --------- |
| GPIO_PIN_12 | Yellow    |
| GPIO_PIN_13 | Orange    |
| GPIO_PIN_14 | Red       |
| GPIO_PIN_15 | Blue      |

