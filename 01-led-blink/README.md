# 01 - LED Blink

This study is my first GPIO output application on the STM32F407G Discovery board.

## Objective

The objective of this study is to learn STM32CubeIDE project creation, code uploading, debugging, and basic GPIO output control by blinking the onboard LEDs.

The Karpuz.go EDU001 blinkLED repository was used as a reference, but this project was recreated as my own STM32CubeIDE project for learning and portfolio documentation.

## Topics to Learn

* STM32CubeIDE project creation
* GPIO Output
* HAL_GPIO_TogglePin
* HAL_GPIO_WritePin
* HAL_Delay
* main.c structure
* Preprocessor-based experiment selection
* Build and Debug/Run operations
* Basic hardware debugging

## Hardware

* STM32F407G Discovery Board
* USB cable
* Onboard LEDs

## Project Structure

This folder contains both the documentation and the STM32CubeIDE project files.

```text
01-led-blink/
  README.md
  project/
```

The `project/` folder contains the STM32CubeIDE project source files.

## Experiment Selection Logic

Multiple LED experiments are included in the same project. Only one experiment is active at a time by changing the `ACTIVE_LED_EXPERIMENT` definition.

```c
#define LED_EXPERIMENT_TOGGLE     1
#define LED_EXPERIMENT_WRITEPIN   2
#define LED_EXPERIMENT_SEQUENCE   3

#define ACTIVE_LED_EXPERIMENT LED_EXPERIMENT_TOGGLE
```

For example, to run the sequential LED experiment, the active experiment can be changed as follows:

```c
#define ACTIVE_LED_EXPERIMENT LED_EXPERIMENT_SEQUENCE
```

## Core Code Logic

The main loop uses conditional compilation to select the active LED experiment.

```c
while (1)
{
#if ACTIVE_LED_EXPERIMENT == LED_EXPERIMENT_TOGGLE

  HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_15);
  HAL_Delay(500);

#elif ACTIVE_LED_EXPERIMENT == LED_EXPERIMENT_WRITEPIN

  HAL_GPIO_WritePin(GPIOD, GPIO_PIN_15, GPIO_PIN_SET);
  HAL_Delay(500);

  HAL_GPIO_WritePin(GPIOD, GPIO_PIN_15, GPIO_PIN_RESET);
  HAL_Delay(500);

#elif ACTIVE_LED_EXPERIMENT == LED_EXPERIMENT_SEQUENCE

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

#endif

  MX_USB_HOST_Process();
}
```

## Experiment 1 - TogglePin

In this experiment, the onboard blue LED is controlled using `HAL_GPIO_TogglePin()`.

```c
HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_15);
HAL_Delay(500);
```

The `HAL_GPIO_TogglePin()` function changes the current state of the selected GPIO pin. If the LED is on, it turns off. If it is off, it turns on.

With `HAL_Delay(500)`, the LED changes state approximately every 0.5 seconds.

## Experiment 2 - WritePin

In this experiment, the onboard blue LED is controlled explicitly using `HAL_GPIO_WritePin()`.

```c
HAL_GPIO_WritePin(GPIOD, GPIO_PIN_15, GPIO_PIN_SET);
HAL_Delay(500);

HAL_GPIO_WritePin(GPIOD, GPIO_PIN_15, GPIO_PIN_RESET);
HAL_Delay(500);
```

`GPIO_PIN_SET` turns the LED on, and `GPIO_PIN_RESET` turns the LED off.

This experiment helped me understand the difference between toggling a pin and writing a specific logic state to a pin.

## Experiment 3 - Sequential LED Control

In this experiment, the onboard LEDs are controlled sequentially using `HAL_GPIO_WritePin()`.

The LEDs blinked sequentially in the following order:

| GPIO Pin    | LED Color |
| ----------- | --------- |
| GPIO_PIN_12 | Yellow    |
| GPIO_PIN_13 | Orange    |
| GPIO_PIN_14 | Red       |
| GPIO_PIN_15 | Blue      |

This experiment helped me understand how multiple GPIO output pins can be controlled one by one.

## Debugging Experiment

A breakpoint was placed on the first LED control line inside the `while(1)` loop.

The program was started in Debug mode and executed step by step using `F6` / Step Over. It was observed that each GPIO command directly affected the physical LED on the STM32F407G Discovery board.

### What I Observed

* The program stopped at the breakpoint before executing the LED control line.
* Using `F6`, the code was executed line by line.
* When the `GPIO_PIN_SET` line was executed, the corresponding LED turned on.
* When the `GPIO_PIN_RESET` line was executed, the corresponding LED turned off.
* This helped me understand the relationship between code execution and physical hardware behavior.

## What I Learned

* A new STM32CubeIDE project was created for the STM32F407G Discovery board.
* GPIO pins can be configured as outputs to control onboard LEDs.
* `HAL_GPIO_TogglePin()` changes the current output state of a pin.
* `HAL_GPIO_WritePin()` writes a specific state to a GPIO pin.
* `HAL_Delay()` can be used to create simple timing delays.
* The `while(1)` loop is the main execution structure in embedded systems.
* Multiple experiments can be kept in the same project using preprocessor definitions.
* Build and Debug/Run operations were used to upload and test the code on real hardware.
* Breakpoints and step-by-step debugging helped observe the connection between software instructions and physical hardware behavior.

## Reference

This study was inspired by the Karpuz.go EDU001 blinkLED example. The implementation in this repository was recreated and extended as a personal STM32F407G Discovery learning project.