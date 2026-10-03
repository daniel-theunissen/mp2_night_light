# Overview

# Circuit Diagram

<img width="701" height="302" alt="embedded_flow_state_bg_padded" src="https://github.com/user-attachments/assets/8254941e-1c53-4f6c-b2f2-2e9500105055" />

# Photoresistor Characterization

<img width="2179" height="1315" alt="photoresistor" src="https://github.com/user-attachments/assets/777b7e4b-d484-4db3-a796-bc780a72ba7a" />

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
