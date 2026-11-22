# STM32F103C8T6 CubeMX projects

This folder is the canonical home for the repository's 15 STM32F103C8T6 CubeMX projects. All `.ioc` files declare `Mcu.CPN=STM32F103C8T6`; CubeIDE and CMake build definitions select the STM32F103xB device family, and C8 linker scripts use 64 KB Flash and 20 KB SRAM. Static checks do not replace a firmware build or hardware validation.

Some project folder names retain historical C6/x6 labels. They are intentionally unchanged so CubeIDE project identities and existing pin assignments remain stable.

The active Proteus simulations use the STM32F103C8 model and are statically checked. `Project Backups/` contains preserved historical snapshots; some may retain C6/x6 identifiers or former absolute paths and are not current C8T6 target evidence. Proteus open/run behavior still requires validation in Proteus.

| Learning area | Project | Project type | Current example and teaching notes |
|---|---|---|---|
| GPIO output | [`STM32F103C8T6_leds_rgb_ex1`](STM32F103C8T6_leds_rgb_ex1/) | CubeIDE | Digital LED outputs on PA4–PA6 and PC13; not PWM color control. |
| GPIO input | [`STM32F103C8T6_PUSH_BUTTON`](STM32F103C8T6_PUSH_BUTTON/) | CubeIDE | Polls PB9; `GPIO_NOPULL` requires an external bias. |
| GPIO / EXTI | [`STM32F103C8T6_Interrupt_push_buuton`](STM32F103C8T6_Interrupt_push_buuton/) | CubeIDE | PB9 EXTI9 and TIM4 update callbacks; review debounce, pull configuration, and ISR work. |
| GPIO / EXTI | [`STM32F103C8T6_External_Interrupt`](STM32F103C8T6_External_Interrupt/) | CubeIDE | Multi-pin external-interrupt exercise; review input biasing and the shared callback behavior. |
| GPIO input/output | [`STM32F103x6_ReadPin_ex1`](STM32F103x6_ReadPin_ex1/) | CubeIDE | Reads PA4 and drives PB12; no internal input pull is configured. Its 72 MHz clock depends on the external HSE. |
| GPIO input/output | [`STM32_F103C6T6_projet0`](STM32_F103C6T6_projet0/) | CubeIDE | Polls PA4 and drives PB12, retaining the original HSI 8 MHz clock; provide a defined input level. |
| GPIO output | [`STM32F103C8T6_BUILT_IN_LED`](STM32F103C8T6_BUILT_IN_LED/) | CMake | Generated built-in LED project; application loop currently has no LED toggle behavior. |
| Startup / debug | [`STM32FC8T6_Startup`](STM32FC8T6_Startup/) | CubeIDE | Startup and ITM/SWO output exercise; `printf` output is not UART and requires a configured SWO/debug path. |
| Timer time base | [`STM32F103C8T_Basic_Timer`](STM32F103C8T_Basic_Timer/) | CubeIDE | TIM4 counter and custom delay helper; review the tick-to-millisecond scaling. |
| Timer / PWM / DMA | [`STM32F103C8T6_Basic_Timer_IT`](STM32F103C8T6_Basic_Timer_IT/) | CubeIDE | TIM1_CH4 PWM with update-triggered DMA changing CCR4; not a basic timer-only example. |
| PWM | [`STM32F103C8T6_TIM_PWM_Output`](STM32F103C8T6_TIM_PWM_Output/) | CubeIDE | TIM2 CH1–CH3 on PA0–PA2; primary multi-channel PWM example. |
| PWM | [`STM32F103C8T_TIM_PWM_TIM2_CH_1`](STM32F103C8T_TIM_PWM_TIM2_CH_1/) | CubeIDE | TIM2 CH1–CH3 on PA0–PA2, approximately 10 kHz with the configured clock; compare channels and output polarity. |
| ADC polling | [`STM32F103C8T6_ADC_Single_Conversion_Polling`](STM32F103C8T6_ADC_Single_Conversion_Polling/) | CubeIDE | ADC1_IN4 on PA4, one-shot polling; review timeout handling and sampling time. |
| ADC interrupt | [`STM32F103C8T_ADC_Single_Conversion_Interrupt`](STM32F103C8T_ADC_Single_Conversion_Interrupt/) | CubeIDE | ADC1_IN4 one-shot interrupt; restart only after completion. |
| ADC scan | [`STM32F103C8Txx_ADC_Conversion`](STM32F103C8Txx_ADC_Conversion/) | CubeIDE | Three-rank scan configuration; review the channel selection and one-shot start behavior. |

The GPIO, TIM, and PWM workshops in [`../workshops/`](../workshops/) use these projects as examples and explicitly distinguish implemented behavior from review exercises.
