# STM32F103C8T6 TIM workshop classification

Timer examples target the STM32F103C8T6 medium-density device. This MCU provides advanced-control TIM1 and general-purpose TIM2, TIM3, and TIM4; they are 16-bit timers. It does not have the TIM6/TIM7 basic timers found on some other STM32 families.

## Timer feature classification

| Timer class | Main features | Representative use |
|---|---|---|
| Time base | Up-counting, prescaler, auto-reload, update event | Periodic timekeeping |
| Update event | Polling, interrupt, DMA | Periodic work, callbacks, register updates |
| Output compare | Frozen, active/inactive, toggle, timing reference | Precise output edge / event |
| Input capture | Rising/falling/both edge, prescaler, filter | Pulse width / frequency measurement |
| Encoder interface | TI1/TI2 quadrature inputs, polarity/filter | Incremental position/speed |
| Slave/master | External clock, reset/gated/trigger modes, TRGO | Synchronize timers or trigger peripherals |
| One-pulse | Single timed output pulse | Trigger / pulse generation |
| Advanced control (TIM1) | Complementary outputs, dead time, break input, repetition counter | Half-bridge / motor-control experiments |

## Progressive workshop sequence

1. `workshop_tim_001.ipynb` — periodic time base, update event, interrupt and timing math
2. `workshop_tim_002.ipynb` — output compare and input capture
3. `workshop_tim_003.ipynb` — encoder, synchronization, one-pulse, and TIM1 advanced features

## Existing project anchors and gaps

- [`STM32F103C8T_Basic_Timer`](../../stm32f103x8/STM32F103C8T_Basic_Timer/) is the introductory TIM4 counter. Its `delay_ms()` waits one timer tick per requested millisecond, but the timer tick is 0.1 ms; the current delay is therefore about ten times shorter than its name implies. Treat correcting the clock/count math as a first exercise.
- [`STM32F103C8T6_Interrupt_push_buuton`](../../stm32f103x8/STM32F103C8T6_Interrupt_push_buuton/) starts TIM4 update interrupts immediately. Its EXTI and timer callbacks both toggle the LEDs directly, so use it to identify ISR/debounce issues, then refactor around an event flag.
- [`STM32F103C8T6_Basic_Timer_IT`](../../stm32f103x8/STM32F103C8T6_Basic_Timer_IT/) is actually TIM1 CH4 PWM with update-triggered DMA. It belongs in the later PWM/DMA stage, not as the first base-timer interrupt project.
- No current C8T6 project demonstrates input capture or encoder mode end-to-end. Those remain CubeMX extension exercises rather than repository-proven projects.

## Clock and timing rules

For edge-aligned up-counting, `f_update = f_TIM / ((PSC + 1) × (ARR + 1))`. A 72 MHz HCLK with APB1 prescaled by 2 gives PCLK1 = 36 MHz, but the APB1 timer clock is doubled to 72 MHz. TIM1 is on APB2; its timer clock is 72 MHz when PCLK2 is 72 MHz.

The timer counter/ARR size is 16-bit on these F103 timers. Check overflow, capture-wrap handling, filter latency, and clock-tree settings when deriving time intervals.
