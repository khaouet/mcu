# STM32F103C8T6 PWM workshop classification

PWM is a timer output-compare mode. The STM32F103C8T6 has PWM-capable channels on TIM1–TIM4. TIM1 provides advanced-control features; TIM2–TIM4 provide general-purpose PWM. PWM resolution/frequency is bounded by the 16-bit timer counter and timer clock.

## PWM feature classification

| Axis | Variants | Use / trade-off |
|---|---|---|
| Alignment | Edge-aligned up-counting; center-aligned up/down | Simpler timing vs. symmetric switching and doubled count traversal |
| Output compare mode | PWM mode 1 / PWM mode 2 | Select active portion of the counter cycle |
| Polarity | Active-high / active-low | Match LED, driver, servo, or load polarity |
| Channel arrangement | One channel; multiple channels on one timer | Shared period/frequency; independent compare duty per channel |
| Timer class | TIM2–TIM4; advanced TIM1 | TIM1 supports complementary outputs, dead time and break |
| Update method | Fixed CCR, CPU update, preload/shadow update, DMA | Static duty or smoothly sequenced waveform |
| Application | LED dimming, servo command, tone, motor control | Select frequency, pulse range, driver, and safety requirements |

## Progressive workshop sequence

1. `workshop_pwm_001.ipynb` — edge-aligned PWM, frequency/duty calculation, polarity
2. `workshop_pwm_002.ipynb` — multiple channels, runtime duty update, preload
3. `workshop_pwm_003.ipynb` — center-aligned and TIM1 complementary/dead-time features

## Existing project anchors

- [`STM32F103C8T6_TIM_PWM_Output`](../../stm32f103x8/STM32F103C8T6_TIM_PWM_Output/) is the first hardware-PWM project. It uses TIM2 CH1–CH3 on PA0–PA2 with PSC=139 and ARR=255, giving about 2.009 kHz for a 72 MHz timer clock. The three channels share that frequency and receive separate compare values.
- [`STM32F103C8T6_leds_rgb_ex1`](../../stm32f103x8/STM32F103C8T6_leds_rgb_ex1/) is a useful contrast: it toggles three GPIO outputs and does not produce variable-duty PWM.
- [`STM32F103C8T6_Basic_Timer_IT`](../../stm32f103x8/STM32F103C8T6_Basic_Timer_IT/) is the advanced DMA exercise: TIM1_CH4 on PA11, 10 kHz edge-aligned carrier, update-triggered CCR sequence, and active-low polarity. CH4 has no complementary CH4N output; the current configuration also disables break and sets dead time to zero. Do not treat it as a safe power-stage example.

The later center-aligned and complementary-output material is an extension beyond these existing projects. Verify timer mode and pin routing in CubeMX before wiring.

## Pin mapping reminders (default mapping)

| Timer channel | Default pin |
|---|---|
| TIM1_CH1..CH4 | PA8..PA11 |
| TIM1_CH1N..CH3N | PB13..PB15 |
| TIM2_CH1..CH4 | PA0..PA3 |
| TIM3_CH1..CH4 | PA6, PA7, PB0, PB1 |
| TIM4_CH1..CH4 | PB6..PB9 |

AFIO remapping can change mappings and may consume pins used by SWD/JTAG or other peripherals. Check the exact CubeMX route and board schematic.
