# mcu
microcontroller unit training course

## STM32 target scope

This repository includes STM32F103 examples, STM32F407 examples, and other course material. Device-specific STM32F103 exercises target the **STM32F103C8T6** Blue Pill when marked C8/C6-adapted:

- Cortex-M3
- STM32F103xB CMSIS/HAL selection
- 64 KB Flash at `0x08000000`
- 20 KB SRAM at `0x20000000`
- 72 MHz system clock where the project configuration intentionally uses HSE × 9

F103 C6 examples in this repository are adapted to the C8T6 target where project evidence confirms the adaptation. Existing pin assignments are preserved rather than replaced with assumptions.

### Verification requirements

- **Proteus/oscillator:** Verify the installed Proteus STM32F103 C8/B model and its crystal/HSE setting; an HSE × 9 configuration assumes a suitable populated external oscillator.
- **LED polarity:** Verify whether the onboard or external LED is active-high or active-low and confirm series resistance. Do not infer polarity solely from CubeMX pin state.
- **SWD:** Check PA13/SWDIO and PA14/SWCLK, probe voltage, NRST, and boot configuration on the real board or selected debugger.
- **External peripherals:** Verify actual buttons, ADC sources, timers, UART, RGB LEDs, and other attached wiring; this repository static adaptation does not validate physical circuitry.
