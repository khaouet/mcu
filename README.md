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

### STM32F103 project layout

All 15 STM32F103C8T6 CubeMX projects are consolidated under [`stm32f103x8/`](stm32f103x8/): 14 CubeIDE projects and one CMake project. Their `.ioc` files identify `STM32F103C8T6`; applicable CMSIS selections and C8 linker regions are checked by the repository validator. Some directory names retain historical C6/x6 labels, but the configured MCU target is C8T6.

The STM32F407 examples remain separate, including the F407 project under `stm32-cmake/`. `cube2/` is not part of this C8T6 project set. See [`stm32f103x8/README.md`](stm32f103x8/README.md) for the project index and learning areas.

### Verification requirements

- **Proteus/oscillator:** Verify the installed Proteus STM32F103 C8/B model and its crystal/HSE setting; an HSE × 9 configuration assumes a suitable populated external oscillator.
- **LED polarity:** Verify whether the onboard or external LED is active-high or active-low and confirm series resistance. Do not infer polarity solely from CubeMX pin state.
- **SWD:** Check PA13/SWDIO and PA14/SWCLK, probe voltage, NRST, and boot configuration on the real board or selected debugger.
- **External peripherals:** Verify actual buttons, ADC sources, timers, UART, RGB LEDs, and other attached wiring; this repository static adaptation does not validate physical circuitry.

## Quality checks

The [Workspace quality gate](./.github/workflows/workspace-quality.yml) runs the STM32 target and course-material validators on pushes and pull requests, excluding `stm32f407/`. Run the same checks locally with Python 3:

```powershell
py -3 .github\modernize\stm32f103c8t6-repository-adaptation\validate_c8t6.py
py -3 scripts\validate_course_materials.py
```

These are static configuration and documentation checks; they do not compile firmware or verify behavior on a board or in Proteus.
