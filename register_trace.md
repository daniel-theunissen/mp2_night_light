# Register Trace

## ADC1

| CubeMX setting | Register | Field (bits) | Value | RM0390 location | Description |
|---|---|---|---|---|---|
| ClockPrescaler PCLK2/8| `ADC_CCR` | `ADCPRE[1:0]` (17:16) | `11` | 13.13.16 | ADCCLK = PCLK2/8 = **10.5 MHz**. |
| ExternalTrigConvEdge RISING| `ADC_CR2` | `EXTEN[1:0]` (29:28) | `01` | 13.13.3, 13.6 | Trigger on a rising edge. |
| ExternalTrigConv ENABLE| `ADC_CR2` | `EXTSEL[3:0]` (27:24) | `0000` | 13.13.3, 13.6 | Selects the TIM1 CC1 event as the trigger. |
| DMAContinuousRequests ENABLE| `ADC_CR2` | `DMA` (8) | `1` | 13.13.3 | Enables DMA requests. |
| DMAContinuousRequests ENABLE| `ADC_CR2` | `DDS` (9) | `1` | 13.13.3, 13.8 | ADC keeps issuing DMA requests after the last transfer. Needed for circular DMA. |
| SamplingTime 56 cycles | `ADC_SMPR2` | `SMP6[2:0]` (20:18) | `011` | 13.13.5, 13.5 | 56 cycles = 5.33 us at 10.5 MHz. |

## TIM1

| CubeMX setting | Register | Field (bits) | Value | RM0390 location | Description |
|---|---|---|---|---|---|
| Prescaler 84 | `TIM1_PSC` | `PSC[15:0]` | `84` | 16.4.11 | Counter clock = 84 MHz / 84 = **1 MHz**. |
| Period 49999 | `TIM1_ARR` | `ARR[15:0]` | `49999` | 16.4.12 | 50,000 counts, so the period is 50 ms (20 Hz). |
| CH1 OCMode PWM2 | `TIM1_CCMR1` | `OC1M[2:0]` (6:4) | `111` | 16.4.7 | OC1REF low while CNT < CCR1, high once CNT >= CCR1. |
| CH1 Pulse 250 | `TIM1_CCR1` | `CCR1[15:0]` | `250` | 16.4.14 | OC1REF rises about 253 us after period start, which triggers the ADC. |
| CH2 OCMode PWM1 | `TIM1_CCMR1` | `OC2M[2:0]` (14:12) | `110` | 16.4.7 | Output high while CNT < CCR2. |
| CH2 Pulse 275 | `TIM1_CCR2` | `CCR2[15:0]` | `275` | 16.4.15 | Divider supply (CH2) high for about 278 us. |

## TIM2

| CubeMX setting | Register | Field (bits) | Value | RM0390 location | Description |
|---|---|---|---|---|---|
| Prescaler 83 | `TIM2_PSC` | `PSC[15:0]` | `83` | 17.4.11 | 84 MHz / 84 = **1 MHz** counter (1 us/tick). |
| Period 99 | `TIM2_ARR` | `ARR[31:0]` | `99` | 17.4.12 | 100 counts = 100 us. PWM is **10 kHz** and the update interrupt is 10 kHz. |
| CH1 / CH2 PWM1 | `TIM2_CCMR1` | `OC1M[2:0]` (6:4), `OC2M[2:0]` (14:12) | `110` | 17.4.7 | Output high while CNT < CCRx. |
| CH3 PWM1 | `TIM2_CCMR2` | `OC3M[2:0]` (6:4) | `110` | 17.4.8 | Same as above. |
| TIM2 global interrupt ENABLE | `NVIC_ISER0` | bit 28 (IRQ 28) | `1` | 10.2, PM0214 | | Turn on the TIM2 interrupt.|
