# 06 - Timer Interrupt LED Blink

This study focuses on blinking an LED using a hardware timer interrupt on the STM32F407G Discovery board.

## Project Name

STM32CubeIDE project name:

Edu06-Timer_Interrupt_LED

Repository folder name:

06-timer-interrupt-led-blink

## Objective

The objective of this project is to understand how to use a hardware timer interrupt instead of using HAL_Delay for LED blinking.

In the previous LED blink projects, the CPU waited inside the main loop with HAL_Delay.

In this project, TIM1 runs in the background. When the timer period elapses, an interrupt occurs and the LED is toggled inside the timer callback function.

## Main Idea

The system works as follows:

TIM1 counts in the background.

When TIM1 reaches the configured period value, an update interrupt is generated.

The HAL library calls HAL_TIM_PeriodElapsedCallback.

Inside the callback, the code checks whether the interrupt came from TIM1.

If the interrupt came from TIM1, the blue LED on PD15 is toggled.

timerInterruptCount is increased to observe how many times the callback has run.

lastInterruptTick stores the latest HAL_GetTick value when the interrupt occurred.

## Topics to Learn

* Hardware timer basics
* Timer interrupt
* Difference between HAL_Delay and timer interrupt
* Callback function
* TIM_HandleTypeDef usage
* htim->Instance check
* HAL_TIM_Base_Start_IT
* HAL_GetTick
* Live Expressions in STM32CubeIDE

## Hardware

* STM32F407G Discovery Board
* USB cable
* On-board blue LED

## Pin Configuration

| Pin | Function | Description |
| --- | -------- | ----------- |
| PD15 | GPIO Output | On-board blue LED |

## Important STM32CubeIDE Settings

TIM1 settings:

* Clock Source: Internal Clock
* Prescaler: 16799
* Counter Period: 9999
* Counter Mode: Up

NVIC settings:

* TIM1 update interrupt and TIM10 global interrupt: Enabled

GPIO settings:

* PD15: GPIO_Output

## Timer Calculation

The STM32F407G Discovery board runs TIM1 with a timer clock around 168 MHz in this configuration.

Prescaler is set to 16799.

This means the timer clock is divided by 16800.

168000000 / 16800 = 10000 Hz

So the timer counts 10000 times per second.

Counter Period is set to 9999.

The timer counts from 0 to 9999, which means 10000 counts.

10000 counts at 10000 Hz gives approximately 1 second.

Therefore, the timer interrupt occurs approximately once every second.

Because the LED is toggled every interrupt, the LED stays on for about 1 second and off for about 1 second.

## Core User Code Sections

The observation variables are defined in the Private Variables section:

`volatile uint32_t timerInterruptCount = 0;`

`volatile uint32_t lastInterruptTick = 0;`

The timer interrupt is started once before the main loop:

`HAL_TIM_Base_Start_IT(&htim1);`

The timer callback function is used to toggle the LED:

`HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)`

Inside the callback, the timer source is checked:

`if (htim->Instance == TIM1)`

Then the interrupt counter is increased:

`timerInterruptCount++;`

The latest system tick is saved:

`lastInterruptTick = HAL_GetTick();`

Finally, the blue LED is toggled:

`HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_15);`

## Code Logic

The timer is configured in STM32CubeIDE.

The timer interrupt is started once before the while loop.

The while loop does not contain LED blinking code.

TIM1 continues counting in the background.

When the timer period elapses, an interrupt occurs.

The HAL library calls the callback function automatically.

The callback checks whether the interrupt came from TIM1.

If it came from TIM1, PD15 is toggled.

## Important Code Note

The callback function name must be written exactly as follows:

`HAL_TIM_PeriodElapsedCallback`

If it is written incorrectly, for example as CallBack instead of Callback, the HAL library will not call the function.

In this project, the callback name was an important debugging point.

## What I Observed

* The blue LED toggled without using HAL_Delay inside the while loop.
* timerInterruptCount increased continuously.
* lastInterruptTick changed approximately every 1000 milliseconds.
* The callback function was called automatically by the HAL library.
* The LED did not blink correctly when the callback function name was written incorrectly.
* The LED blink interval became too long when the counter period was left at 65535.

## What I Learned

* A hardware timer can keep time in the background.
* Interrupts allow the microcontroller to react to events without constantly checking them inside the while loop.
* HAL_TIM_Base_Start_IT starts a timer in interrupt mode.
* Callback functions are called by the HAL library when a related event occurs.
* TIM_HandleTypeDef is a structure type that carries timer-related information.
* The htim->Instance check is used to understand which timer caused the interrupt.
* Variable names are less important than understanding what the function does, what it returns, and when it runs.

## HAL_Delay vs Timer Interrupt

HAL_Delay approach:

The CPU waits inside the main loop.

Timer interrupt approach:

The timer counts in the background and notifies the CPU when the period elapses.

This makes timer interrupts a better foundation for more advanced embedded systems applications.

## Project Structure

* README.md: project documentation
* project/: STM32CubeIDE project source files

## Reference

This project was developed after studying the timer interrupt example from the Karpuz.co STM32CubeIDE education repositories. The implementation was recreated and documented as a personal STM32F407G Discovery learning project.
