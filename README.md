


# Overview

This project is the embedded software for a nightlight. As the room gets darker, the LEDs will smoothly ramp on.

https://github.com/user-attachments/assets/e2124fdf-1287-453a-a57a-2f05e418b0d5

# Circuit Diagram

<img width="701" height="302" alt="embedded_flow_state_bg_padded" src="https://github.com/user-attachments/assets/8254941e-1c53-4f6c-b2f2-2e9500105055" />

The light-sensing element in this project is a photoresistor. I put it in a voltage divider with a 1k6 resistor to get an analog reading of the light level. This value is the geometric mean of the photoresistor extremes which are shown in the following section. This maximizes the voltage swing on the ADC input which is good for precision. I also put a 10n capacitor on that node to smooth out noise on the ADC input. The equivalent resistance of the divider is around 1k2 ohms at the high end, so this means that the ADC input requires ~120us to fully settle (~10 tau). The lights are three basic through-hole LEDs. Assuming a 3V forward drop for the green and blue LEDs and a 2V forward drop for the red LED, the resistors limit the peak current to 10mA.

# Photoresistor Characterization

<img width="2179" height="1315" alt="photoresistor" src="https://github.com/user-attachments/assets/777b7e4b-d484-4db3-a796-bc780a72ba7a" />

I measured the photoresistor at different light levels to get a reasonable characterization of the device. I used a light-meter app on my phone to get the LUX of the environment and plotted that against the resistance. I went from an almost fully dark room up to a very bright lamp shining directly on the photoresistor. I then plotted the data and fit an exponential with an R^2 = 0.9983 which characterizes the device fairly well. 

# System Architecture
<img width="1311" height="681" alt="asdfasdf_fixed" src="https://github.com/user-attachments/assets/3a06623c-0521-44e0-a417-03e7a77c303a" />

This is an event-driven architecture. The photoresistor divider and ADC are controlled by TIM1, which fires at 20Hz. Putting the divider on a timer reduces static power consumption, but requires some settling time before the ADC takes a reading. The divider is enabled on channel 1 for 275us (a bit more conservative than the 120us that I calculated) and the ADC trigger fires at 250us. The ADC is configured for a 56-cycle sample time, so the 25us overlap is more than enough. The ADC writes to a circular DMA buffer that is 16 words long. TIM2 runs at 10KHz controls the LED PWM outputs. It is configured to fire an interrupt that samples the DMA buffer, converts the average ADC reading to LUX, and adjusts the PWM duty cycles accordingly.

# Testing
There is a built-in DAC loopback test that runs before the main program which can be disabled with a preprocessor flag. The point of the test is to determine whether the ADC is performing outside of spec by measuring how many LSBs of error it returns for each input DAC code. I will note that the DAC has a significantly larger tolerance band than the ADC, so the results of the test might not accurately model the ADC performance. However, for this application, it should be adequate. Per the datasheet, the ADC can be expected to exhibit +-4LSB of tolerance. The DAC offset is +-12LSB, the DAC INL is +-4LSB, and the DAC Gain Error is 0.5%. This means that the static tolerance band is at most +-20LSB with a code dependent gain error. If the test fails with this tolerance, it prints out diagnostic message over UART and turns on the user LED. In my testing, it has not failed though. If a tighter tolerance is required, this can be configured. I observed passing results at ~5-6LSB of tolerance.

# Opportunities for Further Work
One challenge with this design is that the LEDs influence the reading of the photoresistor, thus creating a feedback loop. I observed some damped oscillations when the light level changed suddenly, which is expected. One way to potentially deal with this is to estimate the LUX contribution of each LED and subtract it out from the reading. This would require some additional characterization of the LEDs and a better idea of where they would be physically placed in relation to the photoresistor.

# Register Trace
