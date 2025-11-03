# EE186_lab4

# 2 ADC

1. STM32L4R5xx, STM32L4R7xx and STM32L4R9xx datasheet 6.3.21:
   (MSPS = mega samples per second), FAST/SLOW channel
   Resolution = 12 bits 5.33/4.21 Msps
   Resolution = 12 bits 6.15/4.71 Msps
   Resolution = 12 bits 7.27/5.33 Msps
   Resolution = 12 bits 8.88/6.15 Msps

2. Without the voltage divider we have no midpoint to sample the voltage at.

# 3 DAC

1. Our DAC can be set to 8 or 12 bit, with 12 bit being the highest resolution (reference manual 22.1).
   If B = 9 then V_out = V_ref \* 9/(2^12) = 0.00219 V_ref.
2. submitted

3. Higher sampling rate means we can accurately represent higher frequency components of our sound. Sine waves only contain the fundamental frequency component, while sawtooth and square contain a lot of higher harmonics, so they sound more metallic and brighter in timbre. We need a capacitor to remove any DC component, which would bias our speaker and impede its movement.
