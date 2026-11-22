# STM32F103C8T6 GPIO workshop classification

These workshops use the STM32F103C8T6 (medium-density, LQFP48) and STM32 HAL conventions. Follow the [progressive course map](../README.md): begin with PA4–PA6 output, then PB9 input/polling, then EXTI, and only then peripheral pin routing. Verify the board schematic before wiring: Blue Pill clones can differ.

## GPIO feature classification

| Class | Variants | Main choice / use |
|---|---|---|
| Digital input | Floating, pull-up, pull-down | Idle state and whether an external resistor is present |
| Digital output | Push-pull, open-drain | Drive both logic levels or sink current/release the line |
| Output speed | Low, medium, high | Slew rate / EMI; not the CPU clock or a software toggle rate |
| Input reaction | Polling, EXTI interrupt | Simple/slow state check or asynchronous edge notification |
| Edge selection | Rising, falling, both | Select which transition triggers EXTI |
| Alternate function | Timer, UART, SPI, I2C, etc. | Peripheral owns the pin; default mapping or AFIO remap |
| Analog mode | Analog input | Disable digital input path for ADC use |

On STM32F1 the GPIO mode encoding differs from newer STM32 families. HAL `GPIO_MODE_AF_PP` / `GPIO_MODE_AF_OD` configures alternate-function outputs; the peripheral pin mapping is selected by the F1 AFIO mapping/remap options. A pin cannot simultaneously serve unrelated GPIO and peripheral functions.

## Workshop sequence

1. `workshop_gpio_001.ipynb` — PA4 output, PB9 input bias, and push-pull/open-drain
2. `workshop_gpio_002.ipynb` — PB9 polling, EXTI9 edge selection, and debounce
3. `workshop_gpio_003.ipynb` — TIM2 alternate function, AFIO remap, and pin conflicts

## Existing project anchors

- [`STM32F103C8T6_leds_rgb_ex1`](../../stm32f103x8/STM32F103C8T6_leds_rgb_ex1/) is the first output exercise (PA4–PA6 and PC13). It toggles GPIO; it is not a PWM example.
- [`STM32F103C8T6_PUSH_BUTTON`](../../stm32f103x8/STM32F103C8T6_PUSH_BUTTON/) is the polling input exercise. PB9 has no internal pull configured; use a verified external bias or modify the exercise to enable a pull-up and wire the button to ground.
- [`STM32F103C8T6_Interrupt_push_buuton`](../../stm32f103x8/STM32F103C8T6_Interrupt_push_buuton/) is a code-review bridge to EXTI9. Its callback toggles LEDs directly, has no debounce, and should not be copied as the recommended ISR pattern.

## Board and electrical notes

- GPIO is 3.3 V logic. Do not apply 5 V unless the exact pin's datasheet characteristics permit it.
- Floating input pins have no defined logic level. Use a pull resistor or an external driver.
- Open-drain requires a pull-up to the intended logic voltage; never configure a push-pull output onto a shared wired-AND bus.
- Configure the output latch before changing a pin to output to avoid an unintended startup pulse.
- PC13 is the onboard LED pin on many Blue Pill boards, but its LED is commonly active-low and the board circuitry limits its drive. Confirm the exact board; use an external transistor for loads.
- On the F103, a given EXTI line number selects one GPIO port at a time (for example, only one of PA0/PB0/PC0 can map to EXTI0).
