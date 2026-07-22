# 07 - Knight Rider Timer

This study focuses on creating a Knight Rider style LED pattern using a hardware timer interrupt on the STM32F407G Discovery board.

## Project Name

STM32CubeIDE project name:

Edu07-Knight_Rider_Timer

Repository folder name:

07-knight-rider-timer

## Objective

The objective of this project is to create a moving LED pattern without using HAL_Delay inside the main loop.

The Karpuz.co Knight Rider example uses an LED array, for loops, and HAL_Delay to turn LEDs on and off sequentially.

In this project, the same LED pattern idea is implemented with a timer interrupt. This makes the project a continuation of the timer interrupt LED blink study.

## Main Idea

The system works as follows:

TIM1 runs in the background.

TIM1 generates an interrupt approximately every 200 milliseconds.

The HAL library calls HAL_TIM_PeriodElapsedCallback.

Inside the callback, the currently active LED is turned off.

The LED index is updated according to the current direction.

If the index reaches the last LED, the direction is reversed.

If the index reaches the first LED, the direction is reversed again.

The new LED is turned on.

This creates a moving LED effect.

## Topics to Learn

* LED array usage
* Index-based LED control
* Direction variable
* Timer interrupt
* Callback-based state update
* Non-blocking LED pattern
* Simple state machine logic
* Difference between HAL_Delay based pattern and timer based pattern

## Hardware

* STM32F407G Discovery Board
* USB cable
* On-board LEDs

## Pin Configuration

| Pin | LED Color | Function |
| --- | --------- | -------- |
| PD12 | Green | GPIO Output |
| PD13 | Orange | GPIO Output |
| PD14 | Red | GPIO Output |
| PD15 | Blue | GPIO Output |

## Important STM32CubeIDE Settings

GPIO settings:

* PD12: GPIO_Output
* PD13: GPIO_Output
* PD14: GPIO_Output
* PD15: GPIO_Output

TIM1 settings:

* Clock Source: Internal Clock
* Prescaler: 16799
* Counter Period: 1999
* Counter Mode: Up

NVIC settings:

* TIM1 update interrupt and TIM10 global interrupt: Enabled

## Timer Calculation

The STM32F407G Discovery board runs TIM1 with a timer clock around 168 MHz in this configuration.

Prescaler is set to 16799.

This means the timer clock is divided by 16800.

168000000 / 16800 = 10000 Hz

So the timer counts 10000 times per second.

Counter Period is set to 1999.

The timer counts from 0 to 1999, which means 2000 counts.

2000 counts at 10000 Hz gives approximately 0.2 seconds.

Therefore, the timer interrupt occurs approximately every 200 milliseconds.

## Core User Code Sections

The LED pins are stored in an array:

`uint16_t ledPins[4] = {GPIO_PIN_12, GPIO_PIN_13, GPIO_PIN_14, GPIO_PIN_15};`

The current LED index is stored with:

`volatile int8_t currentLedIndex = 0;`

The movement direction is stored with:

`volatile int8_t ledDirection = 1;`

The number of pattern steps is observed with:

`volatile uint32_t knightRiderStepCount = 0;`

The timer interrupt is started once before the main loop:

`HAL_TIM_Base_Start_IT(&htim1);`

The LED pattern is updated inside:

`HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)`

## Code Logic

First, all LEDs are turned off.

Then the first LED is turned on.

TIM1 is started in interrupt mode.

The while loop does not contain the LED movement logic.

Every timer interrupt moves the LED pattern one step forward or backward.

The movement sequence is:

PD12 to PD13 to PD14 to PD15 to PD14 to PD13 to PD12

This repeats continuously.

## Why Timer Interrupt Is Used

In a HAL_Delay based approach, the CPU waits during each delay.

In this timer interrupt based approach, the timer keeps track of time in the background.

The LED pattern is updated only when the timer interrupt occurs.

This makes the structure closer to real embedded systems design.

## What I Observed

* The LEDs moved in a Knight Rider style pattern.
* The LED pattern worked without HAL_Delay inside the while loop.
* The timer callback updated the LED state periodically.
* currentLedIndex changed between 0 and 3.
* ledDirection changed between 1 and -1 at the edges.
* knightRiderStepCount increased at each timer interrupt.

## What I Learned

* Arrays can be used to manage multiple GPIO pins more cleanly.
* An index variable can decide which LED is active.
* A direction variable can control forward and backward movement.
* Timer interrupts can be used to update a pattern periodically.
* A moving LED pattern can be implemented as a simple state machine.
* The important point is not only variable names, but understanding what the code does and how the system state changes.

## Project Structure

* README.md: project documentation
* project/: STM32CubeIDE project source files

## Reference

This project was developed after studying the Knight Rider LED pattern idea from the Karpuz.co STM32CubeIDE education repositories. The original idea was adapted into a timer interrupt based personal learning project for STM32F407G Discovery.
