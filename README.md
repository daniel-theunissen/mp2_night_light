


# Overview

This project is the embedded software for a nightlight. As the room gets darker, the LEDs will smoothly ramp on.

https://github.com/user-attachments/assets/e2124fdf-1287-453a-a57a-2f05e418b0d5

# Circuit Diagram

<img width="701" height="302" alt="embedded_flow_state_bg_padded" src="https://github.com/user-attachments/assets/8254941e-1c53-4f6c-b2f2-2e9500105055" />

The light-sensing element in this project is a photoresistor. I put it in a voltage divider with a 1k6 resistor to get an analog reading of the light level. This value is the geometric mean of the photoresistor extremes which are shown in the following section. This maximizes the voltage swing on the ADC input which is good for precision. I also put a 10n capacitor on that node to smooth out noise on the ADC input. The equivalent resistance of the divider is around 1k2 ohms at the high end, so this means that the ADC input requires ~120us to fully settle (~10 tau). The lights are three basic through-hole LEDs. Assuming a 3V forward drop for the green and blue LEDs and a 2V forward drop for the red LED, the resistors limit the peak current to 10mA.

# Photoresistor Characterization

<img width="2179" height="1315" alt="photoresistor" src="https://github.com/user-attachments/assets/777b7e4b-d484-4db3-a796-bc780a72ba7a" />

I measured the photoresistor at different light levels to get a reasonable characterization of the device. I used a light-meter app on my phone to get the LUX of the environment and plotted that against the resistance. I went from an almost fully dark room up to a very bright lamp shining directly on the photoresistor. I then plotted the data and fit an exponential with an R^2 = 0.9983 which characterizes the device fairly well. 

● System architecture: a diagram as well as an explanation of how data flows from the
timer through the ADC to the LEDs, plus the event-driven structure
● Design decisions: your constants, such as: sampling rate, PWM frequency, ADC
sampling time, and LED color order
● Register trace: for each ADC and timer setting you configured in CubeMX, name the
register and bit field it sets, cite the RM0390 section, and explain the value. Reading
HAL source to find these is OK, but verify against the reference manual. You may have
this as a link to another .md file from your README.md
● Testing: your self-test results and any other tests you ran, with data
● Obstacles: we’re expecting you to point out one major obstacle and how you solved it.
