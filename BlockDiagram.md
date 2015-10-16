Click on the diagram below to download the source SVG file:
![![](http://opendso.googlecode.com/files/OpenDSO%20Block%20Diagram%20version%200.3.png)](http://opendso.googlecode.com/files/OpenDSO%20Block%20Diagram%20v0.3.svg)

# Analog Front-End #
The analog front end is made up of:
  * 2 symmetric channels (1 & 2) with corresponding digital attenuator
  * 1 external trigger channel with digital threshold
  * 1 dual-ADC
These elements will be discussed further in the following sections.
## Input Channel ##
There are 2 symmetric input channels.

Each channel is composed from, going from input to output:
  * An AC/DC/GND coupling circuit that select the way  the signal is coupled to the DSO. Switching must be performed by low-noise analog switches, probably like opto-MOSFet
  * A JFET transistor input stage to provide the required high input impedance
  * A Buffer (unity gain amplifier) stage required to provide the following stages with a strong enough signal
  * A Mux (multiplexer stage): this circuit is to perform a redirect of both input channels to the other one, allowing to swap them, or (more important), to scan the same signal at twice the single-channel sampling rate, effectively doubling the theoretical bandwidth
  * A VGA (Variable Gain Amplifier) that will work as a 1-2-5 sequence attenuator circuit, controlled numerically by the FPGA chip through a DAC (Digital to Analog Converter)
  * A LPF (Low-Pass Filter), required before sampling to meet the Nyquist criterion, stating that the sampled signal frequency components should not exceed half the sampling rate frequency
  * A single-to-differential operational amplifier, whose negative input receive the feedback compensation voltage from the following ADC (Analog to Digital Converter)
## External Trigger ##
There is only one single external trigger circuit, made up of:
  * The same AC/DC/GND coupling circuit as for the signal channels above
  * The same JFET transistor input stage as for the signal channels above
  * The same DAC as the ones used in the signal channels above, but used for a different purpose: it is used here to perform a digital to analog conversion of the trigger threshold value provided by the FPGA
  * A differential operational amplifier used as a comparator between the trigger signal and the threshold value
## Analog to Digital Converter ##
This is the true DSO Heart, composed of a shared dual-channel ADC, with differential inputs to get the best noise immunity
# High Speed Logic (FPGA) #
The high-speed numeric signal processing is performed using:
  * an FPGA (Field Programmable Gate Array), a chip that replace a large amount of wired logic by programmable logic, able to work at speed of at least 300 MHz
  * A low-jitter, high speed clock, used to drive both the FPGA and the ADC
  * An SDRAM memory, used by the FPGA to store the sampled data
**TODO**: More details on the FPGA internal structure needs to be added.
# CPU and Interfaces #
These circuits handle the low-speed data (compared to the sampling rate):
  * A CPU (probably a low-cost 32-bit processor) is used to performed all the low-bandwidth operation and interface to the external world
  * An LCD display with backlight is used to display the sample signal data to the user
  * A touchscreen interface is used to get the user commands and selections
  * An SD Card is used to provide a convenient trace storage for the user
  * A serial Flash memory stores the volatile FPGA configuration, so it can be reloaded at startup
# Power Supply #
Because of the high-speed and low-noise required for a measuring device like a DSO, the power supply must be carefully designed:
  * A battery is present, since the OpenDSO is a portable device
  * Thus, a battery charger is also included
  * The power supply itself is made up of three different parts:
    * The low-noise analog power supply
    * The digital power supply must be able to give power to the fast numeric circuits at all time, despite their sudden power requirements
    * The backlight supplies the high-voltage required to operate the LCD display