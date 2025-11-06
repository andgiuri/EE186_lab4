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

4. submitted

# 4 Musical Instrument

There are two versions in the submission.
First one (main.c) uses a timer interrupt to constantly update the DAC at 64kHz. Notes of different frequencies are achieved by taking smaller or larger steps in the LUT at each sample. This approach uses the CPU to update the DAC.
Second one (main*dma_harmonics.c) uses a timer trigger to update the DAC and the DAC fetches the data through DMA. This means that the CPU is free, but also that we cannot skip samples in the DAC because we're using DMA. Therefore, to change frequency we constantly adjust the value of the ARR register to change the frequency of the timer.
The first approach bounds our highest frequency to 32kHz (Nyquist), the second one bounds the lowest frequency to 32MHz/(LUT_SIZE * (PSC + 1) \* (2^12)). In my case PSC = 0 and LUT_SIZE = 512 so 16Hz. Both approaches cover beyond audible spectrum.

Harmonics are implemented by hard-coding them into the LUT at initialization. This means any method could reproduce them, but I only implemented them in the DMA version.
The user pushbutton gives the theremin a random timbre. When it's pressed, six random numbers in [0,1) are generated and they are used as weights for the overtones 1-6 of the fundamental. The LUT is overwritten as the coefficients are computed and the dac is playing, so you hear the transition. This is why I only implemented it in the DMA mode: when using the CPU to update the DAC at 64kHz, the transition takes soo long.
In the video with the oscilloscope, you can see an imaginary cursor "sweeping" through the waveform and updating it every time I press the button. Not gonna lie it looks very pretty.

SO, it works at pure tones, it can handle probably any headset while sounding good and it can impersonate any instrument (up to the 6th harmonic).
