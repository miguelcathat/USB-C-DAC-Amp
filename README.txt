Miguel personal headphone DACAMP:

Requirements:
	- Able to drive Hifiman Sundara (approx. 300ohms headphone)
	- Low distortion and high fidelity across spectrum (15Hz to 22kHz)
	- Standalone device, driveable via USB
	- Supports USB3.0/2.0
	- Relatively cheap and available parts
	- Highly manufacturable
	
Main Components:

	DAC: PCM2900C
	- USB audio support - No necessity for programmable microcontroller to USB audio interface.
	- 16 bit audio @ 48kHz. Not as high as modern DAC's however can still achieve good quality with circuit design choices.
	- High SNR and dynamic range, 96dB and 93dB each.
	
	OP-AMP: OPA1622
	- Recommended for high power headphones
	- *1688 may be better choice as it uses a FET input instead of bipolar. Would work better for high impedance headphones.
	
	3.3V LDO: ADP714
	- Fixed 3.3V output voltage
	- Low noise and quiescent current
	- 500mA current suitable for PCM2900C power pins
	
System level plan:
USB-C 5V ---> LDO 3.3v ---> USB CODEC ---> Power amplifier ----> Headphone Jack
USB-C 5V ----------------> USB CODEC
USB-C 5V ----------------> Power amplifier
       

---------------------------------------------------------------------------------------------------------------

Design Choices:
USB:
Although we are using usb-c we will still remain using USB 2.0 as we do not need the extra functionaltiy or power 3.0 provides.
CC1 and CC2 require a 5.1k resistor to ground. We are not using the current advertisement mechanism.
Using USB 2.0, it is acceptable to connect the two differential outputs together.

Fuse and high pass filter placed right after Vbus to prevent and sudden high frequency noise and large sudden currents.

Ferrite beads more appropriate than inductor in this situation as it supresses only high frequency signal (>100MHz).

Audio Codec:
External crystal resonator required with 12MHz clock rate. Using (CL1 & CL2) and 2pF stray = (CS).
Digital 3.3 V supplied from LDO ciruit.


3.3V LDO:
Nothing special, simply following datasheet setup.

	
Amplifier calculations w/ LM4810 & Audio Technica M50x:

At this point, we have given up on powering a large impedance headphone and 
have switched to a less powerful design headphone amplifier design.
The OPA1622/1612 has been swapped out for the LM4810 which is able to have
its supply rail connected directly to the the USB-C 5V bus with some
filtering. 

M50x headphone specification:
	-38ohm impedance
	-1,600 mW @ 1KHz max output power
	-Frequency response - 15Hz to 28,000Hz
	
Power dissipation = (Vdd^2/(2pi^2*RL))*2 = 68mW

Using our +5 supply voltage, and output power of 70mW is achievable with 0.1%THD.

Desired low frequency cutoff:
Cmin = 1/(2pi*fc*RL)

According to the datasheet, fc should be five times lower than our desired cutoff 
but this might cause problems with clipping and noise with such a low cut off frequency of 3Hz.
Large capcitors are also quite expensive.
fc will be 10Hz as a middle ground. 
Cmin = 410uF

For the upper cutoff, a similar equation is used
C = 1/(2pi*fc*Ri)

Todo:
- Solving input and feedback resistors for LM4810
- Running EMC rules and output simulations for noise and expected waveform
- PCB Layout and exact component pick

Future goals:
- Replace LM4810 for higher amplification and more sophisticated design
- Adding volume control
- Adding boost converter circuit for high supply rail for amplifiers



	