# \# 02 - Button Input

# 

# This study focuses on reading a digital input from the onboard USER button of the STM32F407G Discovery board.

# 

# \## Objective

# 

# The objective of this study is to learn GPIO input by reading the onboard USER button and controlling an onboard LED according to the button state.

# 

# In this project, a new STM32CubeIDE project was created instead of directly importing a ready-made repository. The project was implemented, built, uploaded, and tested on real STM32F407G Discovery hardware.

# 

# \## Topics to Learn

# 

# \* STM32CubeIDE project creation

# \* GPIO Input

# \* GPIO Output

# \* HAL\_GPIO\_ReadPin

# \* HAL\_GPIO\_WritePin

# \* Polling

# \* Button state control

# \* Basic input-output relationship

# \* Build and Debug/Run operations

# 

# \## Hardware

# 

# \* STM32F407G Discovery Board

# \* USB cable

# \* Onboard USER button

# \* Onboard blue LED

# 

# \## Pin Configuration

# 

# | Pin  | Function    | Description |

# | ---- | ----------- | ----------- |

# | PA0  | GPIO\_Input  | USER Button |

# | PD15 | GPIO\_Output | Blue LED    |

# 

# \## Target Behavior

# 

# \* When the USER button is pressed, the blue LED turns on.

# \* When the USER button is released, the blue LED turns off.

# 

# \## Core Code Logic

# 

# ```c

# while (1)

# {

# &#x20; if (HAL\_GPIO\_ReadPin(GPIOA, GPIO\_PIN\_0) == GPIO\_PIN\_SET)

# &#x20; {

# &#x20;   HAL\_GPIO\_WritePin(GPIOD, GPIO\_PIN\_15, GPIO\_PIN\_SET);

# &#x20; }

# &#x20; else

# &#x20; {

# &#x20;   HAL\_GPIO\_WritePin(GPIOD, GPIO\_PIN\_15, GPIO\_PIN\_RESET);

# &#x20; }

# }

# ```

# 

# \## Explanation

# 

# The `HAL\_GPIO\_ReadPin(GPIOA, GPIO\_PIN\_0)` function reads the current state of the USER button connected to pin PA0.

# 

# If the button is pressed, the pin state becomes `GPIO\_PIN\_SET`. In this case, the blue LED connected to PD15 is turned on using `HAL\_GPIO\_WritePin()`.

# 

# If the button is not pressed, the pin state is not `GPIO\_PIN\_SET`, so the blue LED is turned off.

# 

# \## Polling Logic

# 

# This project uses polling.

# 

# Polling means that the microcontroller continuously checks the button state inside the `while(1)` loop.

# 

# In this project, the microcontroller repeatedly asks:

# 

# ```text

# Is the button pressed?

# ```

# 

# If the answer is yes, the LED is turned on. If the answer is no, the LED is turned off.

# 

# \## What I Observed

# 

# \* The blue LED turned on when the USER button was pressed.

# \* The blue LED turned off when the USER button was released.

# \* The button state was read continuously inside the infinite loop.

# \* A physical input was used to control a physical output.

# 

# \## What I Learned

# 

# \* GPIO pins can be configured as digital inputs.

# \* GPIO pins can be configured as digital outputs.

# \* `HAL\_GPIO\_ReadPin()` is used to read a digital input pin.

# \* `HAL\_GPIO\_WritePin()` is used to write a digital output state.

# \* Polling is a simple way to continuously check an input.

# \* A button can be used to control an LED through software.

# \* The relationship between input peripherals and output peripherals was observed on real hardware.

# 

# \## Project Structure

# 

# This folder contains both the documentation and the STM32CubeIDE project files.

# 

# ```text

# 02-button-input/

# &#x20; README.md

# &#x20; project/

# ```

# 

# The `project/` folder contains the STM32CubeIDE project source files.

