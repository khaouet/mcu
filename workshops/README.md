# STM32F103C8T6 workshops

These workshops use the STM32F103C8T6 (medium-density, LQFP48), STM32CubeMX configuration, CubeIDE-generated HAL projects, and the repository examples in [`../stm32f103x8/`](../stm32f103x8/). Progress from simple GPIO behavior to timer-generated waveforms; do not begin with DMA or advanced TIM1 modes.

The workshop notebooks are English-language, hands-on companions to the 14 French weekly lecture notebooks in [`../notebooks/README.md`](../notebooks/README.md) and the French [course syllabus](../syllabus/README.md). C examples are presented as fenced Markdown snippets; edit and build them in the linked CubeIDE/CMake project rather than executing them as notebook cells.

## Progressive learning route

| Stage | Workshop | Start from repository project | Learning checkpoint |
|---|---|---|---|
| 0. Project setup | Read the target notes below | Any C8T6 `.ioc` | Confirm MCU, clock tree, pinout, generated initialization, build and debug |
| 1. Digital output | [GPIO 001](GPIO/workshop_gpio_001.ipynb) | `STM32F103C8T6_leds_rgb_ex1` | Configure/toggle PA4–PA6; identify active level and output speed |
| 2. Digital input | [GPIO 001](GPIO/workshop_gpio_001.ipynb) | `STM32F103C8T6_PUSH_BUTTON` | Explain PB9's external bias and interpret the pressed state |
| 3. Polling and EXTI | [GPIO 002](GPIO/workshop_gpio_002.ipynb) | `STM32F103C8T6_Interrupt_push_buuton` | Compare polling with EXTI9; replace ISR-side LED work with an event flag |
| 4. Timer time base | [TIM 001](TIM/workshop_tim_001.ipynb) | `STM32F103C8T_Basic_Timer` | Derive the timer tick/update period; detect and correct the delay scaling |
| 5. Output compare / capture | [TIM 002](TIM/workshop_tim_002.ipynb) | Create a CubeMX variation | Distinguish compare output from captured input timestamps |
| 6. Peripheral pin routing | [GPIO 003](GPIO/workshop_gpio_003.ipynb) | `STM32F103C8T6_TIM_PWM_Output` | Route TIM2_CH1 to PA0 and identify conflicts/remaps |
| 7. Single-channel PWM | [PWM 001](PWM/workshop_pwm_001.ipynb) | `STM32F103C8T6_TIM_PWM_Output` | Calculate the actual carrier frequency and CCR duty |
| 8. Multi-channel PWM | [PWM 002](PWM/workshop_pwm_002.ipynb) | `STM32F103C8T6_TIM_PWM_Output` / `STM32F103C8T6_leds_rgb_ex1` | Share one period across channels; contrast hardware PWM with GPIO toggling |
| 9. Timer DMA / advanced modes | [PWM 003](PWM/workshop_pwm_003.ipynb), then [TIM 003](TIM/workshop_tim_003.ipynb) | `STM32F103C8T6_Basic_Timer_IT` | Trace update-triggered DMA; identify what is and is not complementary PWM |
| 10. ADC integration | [ADC workshops](ADC/) | ADC projects listed below | Begin with one-shot polling, then interrupt; use TIM trigger lessons only after clock and timing math |

Stages 5–9 assume the learner can read CubeMX's clock tree and pinout. TIM capture, encoder, synchronization, center-aligned PWM, and power-stage features are extensions; the current project set does not provide complete, verified examples for all of them.

## CubeMX / CubeIDE workflow used throughout

1. Open a project's `.ioc`; confirm **STM32F103C8T6**, LQFP48, and the intended pin/peripheral mapping before regenerating code.
2. Verify the clock assumptions. Most projects request an 8 MHz HSE and PLL ×9 for 72 MHz SYSCLK. That requires an actual matching external clock source.
3. Check APB1 is divided by two in the common 72 MHz setup: PCLK1 is 36 MHz, while APB1 timer clocks are doubled to 72 MHz. ADC examples use a divide-by-six prescaler for 12 MHz ADCCLK.
4. Preserve CubeMX-generated files and place learner changes in `USER CODE` sections. Rebuild after changing an `.ioc`; compare generated GPIO/MSP/IRQ code with the pinout.
5. Use the debugger to inspect state; use a meter/scope/logic analyzer for pin levels and timing. A successful IDE build does not prove the board wiring or signal polarity.

## Repository project review

All 15 `.ioc` projects under `stm32f103x8/` declare the C8T6 target: 14 CubeIDE projects and one CMake project. Use these projects as evidence for what the repository currently demonstrates; several are deliberately marked below as review/repair exercises rather than reference-quality solutions.

| Project | Type | Current example | Teaching use / review note |
|---|---|---|---|
| [`STM32F103C8T6_leds_rgb_ex1`](../stm32f103x8/STM32F103C8T6_leds_rgb_ex1/) | CubeIDE | GPIO outputs on PA4–PA6 and PC13; HAL delay and GPIO lock | First output exercise. Digital on/off, not PWM color control. Verify board LED polarity. |
| [`STM32F103C8T6_PUSH_BUTTON`](../stm32f103x8/STM32F103C8T6_PUSH_BUTTON/) | CubeIDE | PB9 input polling; PA outputs control LEDs | PB9 is `GPIO_NOPULL`; it needs a defined external bias. |
| [`STM32F103C8T6_Interrupt_push_buuton`](../stm32f103x8/STM32F103C8T6_Interrupt_push_buuton/) | CubeIDE | PB9 EXTI9 and TIM4 update interrupt | Review only: unconditional timer start, application GPIO in callbacks, no pull or debounce. |
| [`STM32F103C8T6_External_Interrupt`](../stm32f103x8/STM32F103C8T6_External_Interrupt/) | CubeIDE | Multi-pin external-interrupt exercise | Review input biasing and shared callback behavior before wiring. |
| [`STM32F103x6_ReadPin_ex1`](../stm32f103x8/STM32F103x6_ReadPin_ex1/) | CubeIDE | Reads PA4 and drives PB12 | No internal input pull; 72 MHz operation depends on the external HSE. |
| [`STM32_F103C6T6_projet0`](../stm32f103x8/STM32_F103C6T6_projet0/) | CubeIDE | Polls PA4 and drives PB12 at HSI 8 MHz | Provide a defined input level; the legacy project name is retained. |
| [`STM32F103C8T6_BUILT_IN_LED`](../stm32f103x8/STM32F103C8T6_BUILT_IN_LED/) | CMake | Built-in LED project | Generated project currently has no LED toggle behavior in its application loop. |
| [`STM32FC8T6_Startup`](../stm32f103x8/STM32FC8T6_Startup/) | CubeIDE | Startup / ITM-SWO output | `printf` output is not UART; SWO/debug setup is required. |
| [`STM32F103C8T_Basic_Timer`](../stm32f103x8/STM32F103C8T_Basic_Timer/) | CubeIDE | TIM4 counter and custom polling `delay_ms` | PSC=7199 makes a 10 kHz counter; the helper waits `ms` ticks, about `ms/10`. 16-bit ARR gives a 6.5536 s update period. |
| [`STM32F103C8T6_Basic_Timer_IT`](../stm32f103x8/STM32F103C8T6_Basic_Timer_IT/) | CubeIDE | TIM1_CH4 PWM with update-triggered DMA changing CCR4 | Despite the name, this is PWM+DMA, not a basic timer-interrupt lesson; active-low output and no complementary CH4. PA11 may conflict with USB D−. |
| [`STM32F103C8T6_TIM_PWM_Output`](../stm32f103x8/STM32F103C8T6_TIM_PWM_Output/) | CubeIDE | TIM2 CH1–CH3 on PA0–PA2; live CCR updates | Primary PWM example. PSC=139 and ARR=255 yield about 2.009 kHz with a 72 MHz TIM2 clock. |
| [`STM32F103C8T_TIM_PWM_TIM2_CH_1`](../stm32f103x8/STM32F103C8T_TIM_PWM_TIM2_CH_1/) | CubeIDE | TIM2 CH1–CH3 on PA0–PA2 | Approximately 10 kHz with the configured clock; compare channels and output polarity. |
| [`STM32F103C8T6_ADC_Single_Conversion_Polling`](../stm32f103x8/STM32F103C8T6_ADC_Single_Conversion_Polling/) | CubeIDE | ADC1_IN4 on PA4, one-shot polling | Starting ADC exercise. Review timeout handling and sampling time/source impedance. |
| [`STM32F103C8T_ADC_Single_Conversion_Interrupt`](../stm32f103x8/STM32F103C8T_ADC_Single_Conversion_Interrupt/) | CubeIDE | ADC1_IN4 one-shot interrupt | Restart only after the previous conversion completes. |
| [`STM32F103C8Txx_ADC_Conversion`](../stm32f103x8/STM32F103C8Txx_ADC_Conversion/) | CubeIDE | ADC scan configured for three ranks | Review exercise: channel 4 is configured for all ranks; one non-continuous conversion starts only once. |

The workshops explain these issues and provide a safer learning sequence; this pass does not rewrite the CubeMX/CubeIDE example projects or assume their physical board wiring.
