# eFish-Field-Logger

# _eFish Field Logger_: An electric fish logger for automated field electric signal recordings

William GR Crampton, Michael A. Haag, & Lok Poon
Department of Biology, University of Central Florida, 4100 Libra Dr., 32816, Orlando, FL, USA

## Summary

This document describes the “eFish Field Logger” (for brevity, referred to below as the “EOD logger”), an automated electronic logging device designed to amplify and record the underwater electric organ discharge (EOD) fields generated by weakly electric fish. This device comprises a dual-channel bioamplifier with an integrated microcontroller and real-time clock, connected to two pairs of submerged electrodes. The dual-channel bioamplifier employs a single-ended amplification design and is equipped with configurable low-pass and high-pass filters, allowing users to tailor the frequency bandwidth to suit specific research needs. This document provides a comprehensive description of the design of the EOD logger, include notes on its construction, and a guide to field operation in shallow habitats such as forest streams or swamps.

<br />

All resources for the construction and operation of our EOD logger are located at: [https://github.com/Crampton-Lab/eFish-Field-Logger](https://github.com/Crampton-Lab/eFish-Field-Logger)

## Logger description
Our EOD logger (Fig. 1) features a double-decker design. The upper deck’s printed circuit board (PCB) hosts the microcontroller, real-time clock contacts, and battery power socket. The lower deck’s PCB houses a dual bio-amplifier. Signals from the two bio-amplifier channels are transmitted to the microcontroller via headers connecting the upper and lower-deck PCBs. We refer to the upper board as the “microcontroller board” and the lower deck as the “dual-amp board.” Circuit schematics for both boards are provided in Fig. 2. A component list is presented in Table 1, and the bioamplifier specifications are listed in Table 2. The total cost of all components for one logger, including the battery pack, electrode assembly, and waterproof enclosure was approximately $200 in 2022. We printed our circuit boards at PCBWay, Hangzhou, China ([https://www.pcbway.com/](Logger description

<br />

Our EOD logger (Fig. 1) features a double-decker design. The upper deck’s printed circuit board (PCB) hosts the microcontroller, real-time clock contacts, and battery power socket. The lower deck’s PCB houses a dual bio-amplifier. Signals from the two bio-amplifier channels are transmitted to the microcontroller via headers connecting the upper and lower-deck PCBs. We refer to the upper board as the “microcontroller board” and the lower deck as the “dual-amp board.” Circuit schematics for both boards are provided in Fig. 2. A component list is presented in Table 1, and the bioamplifier specifications are listed in Table 2. The total cost of all components for one logger, including the battery pack, electrode assembly, and waterproof enclosure was approximately $200 in 2022. We printed our circuit boards at PCBWay, Hangzhou, China (https://www.pcbway.com/).
The Eagle files required for ordering our printed circuit boards are available at the following locations:
Dual-amp board:)).
The Eagle files required for ordering our printed circuit boards are available at the following locations:

* Dual-amp board: https://github.com/Crampton-Lab/eFish-Field-Logger/tree/main/dual-amp

* Microcontroller board: https://github.com/Crampton-Lab/eFish-Field-Logger/tree/main/microcontroller

**Figure 1.**  Photograph of the EOD logger. a) Microcontroller board connected above the dual-amp board and connected to a battery pack. RTC = real-time clock. G1 through G3 refer to gain stages 1 through 3. The “R” numbers correspond to the resistors listed in the components list (Table 1).

<img src="/resources/images/Picture1.png" width=60% height=60%>

Following Mucha et al. (2022) our EOD logger utilized the Teensy 3.5, a programmable microcontroller compatible with the Arduino Integrated Development Environment (IDE). This device, manufactured by PJRC (Sherwood, OR) allows single-ended acquisition from multiple analog-to-digital converter inputs with a nominal resolution of 16 bits, although the effective resolution is 12 bits. It operates with battery voltages ranging from 3.6 to 6V DC. The Teensy 3.5 features a 120 MHz Cortex-M4F CPU, a micro-SD card slot, and a micro-USB port for uploading Arduino IDE sketches (programs) from a computer. The Teensy 3.5 connects to and powers an external real-time clock (RTC) board (Adafruit DS3231, Adafruit Industries LLC, New York, NY) via the wired connections summarized in Fig. 3, providing precise time signal during operation. Our microcontroller board’s solder pads also accept the Teensy 4.1, which offers a faster CPU speed but is limited to 10-bit resolution, requires a 3.3V DC battery voltage, and necessitates different Arduino libraries.

## Bio-amplifier design

Our bio-amplifier features two channels, each providing single-ended amplification. Single-ended amplifiers measure the potential difference between the signal (“positive”) pole of the electrode pair and the ground electrode. The signal voltage output (Vout) is equal to the total gain of the amplifier multiplied by Vpos – Vneg, where Vpos is the voltage at the positive electrode and Vneg is voltage at the ground electrode. While single-ended amplifiers typically have a slightly inferior signal-to-noise ratio compared to differential amplifiers, they require fewer amplifier integrated circuits (ICs), and associated components, thereby reducing costs and simplifying assembly. Due to the shared grounding among all inputs on the digitizer, our single-ended design is limited to two electrode pairs, which share a common ground. The orthogonal L-shaped pattern of our electrodes, described below, is compatible with this design.

<br />

_**Virtual ground**_: We use a virtual ground as the baseline for our signals, achieved by employing the TLC2272 dual operational amplifier (TLC2271-1A for channel 1, TLC2272-1B for channel 2) in a voltage follower configuration. This setup includes 10 microF bypass capacitors to suppress noise and a voltage divider to halve the power supply voltage. As a result, the baseline for our signals is the power supply voltage divided by two. When used with the microcontroller, which regulates the dual-amp’s voltage supply to 3.3V DC, the baseline is set at 1.65V (3.3V/2). The clipping voltages around this 1.65V baseline, determined by the output swing of the gain stage 1 instrumentation amplifier (AD623ANZ), range from 0.2 V (minimum) to 3V (maximum), providing a peak-to-peak voltage of 2.8V.

<br />

The 1.65V offset is programmatically removed in the Arduino code during the saving of the digitized signal to a Microsoft signed 16-bit PCM (.wav) file format on the microcontroller’s microSD card. In this process, the original voltage range (0V to 3.3V) is converted to a range of -32,767 to +32,768, with zero representing the 1.65V baseline. This format distributes the full voltage range across the nominal 16-bit range of 216 = 65,536 positions. As a result, all electric fish EODs are centered at zero, whether they are from pulse-type fish, where there is a clear 0V baseline between pulses, or wave-type fish, where the 0V baseline is not recognizable from EOD landmarks.

<br />

**Figure 2.** Circuit schematics.
**Microcontroller board:**

<img src="/resources/images/Picture2.png" width=40% height=40%>

<br />

**Dual-amp board:**

<img src="/resources/images/Picture3.png" width=80% height=80%>

<br />

<img src="/resources/images/Picture4.png" width=80% height=80%>

<br />

<img src="/resources/images/Picture5.png" width=80% height=80%>

<br />

**Figure 3.** Schematic for the connection of the dual-amp board, microcontroller board, and battery power.

<img src="/resources/images/Picture6.png" width=80% height=80%>

<br />

_**Gain:**_ Each channel has three gain stages, G1, G2, and G3 (see Fig. 2), each equipped with bypass capacitors to suppress power supply noise. G1 utilizes the Analog Devices AD623ANZ instrumentation amplifier IC, while G2 and G3 utilize the Texas Instruments TLC2272 dual operational amplifier (op-amp) IC in a negative feedback loop configuration. The negative feedback loop in each gain stage inverts the signal. Since our dual-amp has three gain stages, the final amplified signal is inverted relative to the input. Consequently, an electric fish’s EOD will appear head positive on a given channel (when the fish is parallel to the ground-signal electrode axis) if its head is pointing toward the ground electrode.

<br />

Because the TLC2272 is a dual op-amp (i.e., the IC contains two separate amplifying circuits) we were able to perform gain stages 2 and 3 for a given channel on the same IC (see Figs. 1 and 2). Gain can be controlled at each stage by substituting resistors. The total gain of the dual-amp is calculated as the product of the gains for each gain stage. For our field work, we configured the device for a total gain of 90.36 (gain stage 1 = 5.02, stage 2 = 3, stage 3 = 6) +/- 0.3% for resistor tolerance, assuming resistors with 0.1% tolerance. Resistor substitution is straightforward because the resistors are inserted into female header sockets, allowing for easy replacement without the need for resoldering. Examples of the G1, G2, and G3 resistors configurations required for a variety of total gains, ranging from 1.21 to 600.6, are listed in Table 3.

<br />

_Gain stage 1_: We used the AD623ANZ instrumentation amplifier for gain stage 1 due to its high common-mode rejection, which maximizes the signal-to-noise ratio at the input. The gain of G1 is adjusted by substituting the gain resistor at the 2-position female connector at R5 (channel 1) and R14 (channel 2). Using resistors with 0.1% tolerance (i.e., within 0.1% of the nominal resistance value), gains of 2.0, 5.02 (default), and 10.09 are achieved with 100 kOhm, 24.9 kOhm, and 11 kOhm resistors, respectively. The minimum allowable gain is 1 (unity gain), which is achieved by leaving R5/R14 unpopulated. We recommend caution if selecting a G1 gain of more than 10 For gain values below 10 the AD623ANZ maintains a completely flat frequency response from 0 to approximately 36 kHz, preserving the full frequency content of most electric fish signals (including all gymnotiform species). However, at a G1 gain of 20.02, achieved with a 5.23 kOhm resistor, the frequency response begins to roll off around 20 kHz. This may be satisfactory for species with lower frequency contents, but not for those with higher frequency content, such as some species of Mormyridae (Osteoglossiformes). The AD623ANZ manufacturer’s manual provides further notes on gain-dependent frequency responses.

<br />

_Gain stages 2 and 3_: In a negative feedback loop the gain of the TLC2272 op-amp is calculated by the ratio Rf/Rg, where Rf is the substitutable feedback resistor and Rg is the fixed gain resistor (Ulaby and Maharbiz 2010). The TLC2272 maintains a flat frequency response across all gain levels up to 2.18 MHz, ensuring that the chosen gains for gain stages 2 and 3 do not affect the frequency content of the amplified signals. The gain of stage 2 can be adjusted by substituting the Rf resistors at R11 (channel 1) and R20 (channel 2), while the gain of stage 3 can be adjusted by substituting the Rf resistors at R13 (channel 1) and R22 (channel 2). For both gain stages 2 and 3, the gain of a given channel is determined by the nominal resistance of the resistor divided by 1000. For example, gains of 1.1, 3, and 10 are achieved using 1.1kOhm, 3kOhm, and 10kOhm resistors, respectively. We recommend setting gains no lower than 1.1 for the TLC2272, as using resistors with values less than 1.1 kOhm may cause instability. Additionally, the frequency response may change if the G1 gain exceeds x10. Distributing the gain equally between gain stages 2 and 3 does not provide a significant advantage.

<br />

**_Filters_**: Each channel of the dual amp is equipped with high-pass and low-pass 2nd order Sallen-Key filters, constructed using the TLC2272 dual operational amplifier ICs. Two internal amplifiers (A and B) of the TLC2272 are used for the low-pass and high-pass filters of a given dual amp channel, respectively. The bandpass is determined during assembly by the choice of resistors R6 and R7 (for channel 1) and R15 and R16 (for channel 2) in the high-pass filter stage, and by the choice of capacitors C8 and C9 (for channel 1) and C16 and C17 (for channel 2) in the low-pass filter stage. 
Example configurations: A relatively “narrow” bandpass is achieved by using 1 kOhm resistors in the high-pass filer (R6, R7, R15, R16), and 3,900 pF capacitors in the low-pass filter (C10, C11, C18, C19). A relatively “wide” bandpass is achieved by using 3.6 kOhm resistors in the high-pass filter and 1,300 pF capacitors in the low-pass filter. The narrow bandpass configuration has a predicted 6 dB attenuation from 1.6 Hz to 39.902 kHz (1 dB from 4.4 Hz to 13.772 kHz). The wide bandpass configuration has a predicted <6 dB attenuation from 0.4 Hz to 108.893 kHz (<1 dB from 1.3 Hz to 38.725 kHz).
At a sampling rate of 49,152 Hz—the rate at which the Teensy 3.5 microcontroller’s analog-to-digital converters are configured for our field use—frequencies below approximately 24,576 Hz are automatically filtered. Gymnotiform signals rarely contain content over 15 kHz, so the wide bandpass filter configuration described above will not attenuate any natural signal content. This configuration remains above the frequency at which the digitizer’s sampling rate automatically filters the signal, as dictated by Nyquist theory.

_Battery power_: Power for both the Teensy 3.5 and the dual-amp board is provided by a battery pack connected to the GND-VCC plug on the microcontroller board (Fig. 3). The Teensy 3.5 requires a 3.4-6V DC power supply, which is then regulated to 3.3V (with a maximum output of 250 mA) and output to the 3.3V pins. When the Teensy 3.5 is mated to the microcontroller board, a shorting connector must be placed across the ‘3.3A’ contacts on the dual-amp board. There is also a backup power input at the 3.3B contact, which is designated as “do not place components” (DNP) on the schematic. This backup input can be used instead of, but not in addition to, 3.3A. If 3.3B is used, it must be shorted, and 3.3A should be assigned as DNP.

<br />

To power the Teensy 3.5 and one dual-amp, we found the best performance and battery longevity with a power pack consisting of two 4-AA battery packs connected in parallel. This configuration provided approximately 5.5-5.7 V using fully charged rechargeable Ni-MH Panasonic Eneloop Pro batteries. We do not recommend using alkaline batteries or overcharged rechargeable batteries in this setup, as the voltage could exceed 6V, which may lead to CPU overheating. Voltages above 6V can permanently damage the Teensy 3.5. The eight-AA battery pack offers the logger 36-41 hours of autonomy during continuous two-channel acquisition of consecutive 1-minute recordings on the microSD card, with the microcontroller CPU speed set to 72 MHZ. Under the the same settings, a single four-AA battery pack provides 17-20 hours of autonomy. We often launched the EOD loggers to begin acquiring data well before the scheduled time of our field recordings. This allowed us to use the Teensy 3.5’s built-in LED blink test to verify that logging was underway. As a result, the extended autonomy and reliability of larger battery packs led us to choose the 8-AA battery pack for field use.

## Electrodes and cables
For shallow forest-stream or swamp deployment of the EOD logger the two pairs of electrodes, representing channels 1 and 2, are arranged in an “L” formation. The signal electrodes at the ends of the “L”, while the “negative” poles (ground reference electrodes) are located at the corner of the “L” (Fig. 4). This orthogonal arrangement optimizes sensitivity to electric fish with dipole fields oriented in a wide range of angular positions relative to the “L”. A single pair of electrodes shows the highest voltage response to a fish whose dipole is parallel to the pair, and the lowest voltage response to a fish whose dipole is perpendicular to the pair.

<br />

_**Electrode construction and connection to the dual-amp board**_: For the cable connecting the EOD machine to the orthogonal electrodes we used a 5 meter sections of 4-conductor cable containing four 22 AWG wires, with an aluminum shield and plastic jacket (model M22/4F, Pacer, Sarasota, FL) (Fig. 4). Our electrodes (Fig. 4) consist of 15 mm sections of 16 AWG nickel-chromium (Ni-Cr) wire, crimped to the corresponding wires in the four-channel conductor cable. We used Haisstronica BTT-0.5 heat-shrink crimps (Fig. 4) to secure the connections. To form the crimp joint, we first connected the nickel-chromium wire to one end of the heat-shrink crimp and the 22 AWG wire from the 4-conductor cable to the other. Before crimping, we slid two sections of standard heat shrink over the 22 AWG wire: one small enough to just fit over the wire, and another wide enough to cover the end of the crimp once closed. The Haisstronica heat-shrink crimps were then crimped using an OmTara crimping plier (Eurotool model PLR-537). We filled the inside of the crimp with silicone before heating it with a hot air gun to activate the heat-shrink. Additional silicone was applied to the 22 AWG wire end of the crimp, followed by heat-shrinking the two sections of standard heat shrink—first the smaller diameter one, then the wider diameter one. Finally, we applied more silicone to any area where water could potentially infiltrate the joint. Each orthogonal pair of electrodes required four Ni-Cr electrodes. 

<br />

At the logger end of the 4-conductor cable the four wires were soldered to two right-angled 2-position female connector sockets, with the following color coding (GND = ground reference): Channel 1 signal = Red; Channel 1 GND = Black; Channel 2 signal = White; Channel 2 GND = Green. The two 2-position female connector sockets were then connected to the respective male headers for the channel inputs on the dual-amp board, following the scheme outlined in Figs. 1 and 3.

<br />

**Figure. 4.** Waterproof enclosure, cable assembly, and electrodes for deployment of EOD logger in shallow forest stream habitats.

<img src="/resources/images/Picture7.png" width=80% height=80%>

<br />

<img src="/resources/images/Picture8.png" width=80% height=80%>

<br />

_**Electrode placement and maintenance in the field:**_ To tether the Ni-Cr electrodes to the stream bed we used three stakes, each made from a 40 cm section of ¼” aluminum rod. A 15 cm section of schedule 80 ¼” PVC tubing was slid over the terminal 10 cm of the rod and fixed in position with epoxy resin. Lead barrel weights, commonly used in gill net and other fishing net construction, were slid along the plastic jacket of the 5 m-long 4-conductor cable and secured in place with cable ties.

<br />

We used cable ties to attach the two ground electrodes (channel 1 ground = black wire, channel 2 ground = green wire) to the end of one stake (Fig. 4), the channel 1 signal (+) electrode (red wire) to a second stake, and the channel 2 signal (+) electrode (white wire) to the third stake. The three stakes were then driven deep into the sand using a rubber mallet, ensuring they were spaced 0.6 meters apart in the L-shaped pattern shown in Fig. 4, with the ground reference electrodes positioned at the corner of the “L”. The terminal PVC tubing portion of each stake was left projecting into the open water, approximately 3-4 cm above the sand bed. Finally, we routed the 4-conductor cable under the sand from the electrode locations to the bank, to prevent the current from tugging at the electrodes. The electrodes were inspected daily during EOD logger battery/micro-SD card replacement. Any debris obstructing the Ni-Cr electrodes was removed as needed (though this was rarely necessary), and we ensured that the 4-conductor cable remained buried under the sand substrate.

## Enclosure
For shallow forest-stream and swamp deployment of the EOD logger, we used a plastic waterproof field box (Strategy model SSFLAMBX1), typically used for storing shotgun cartridges, to enclose the logger in the field (Fig. 4). The logger and battery pack were secured inside the box using a block made of cut layers of Styrofoam, held together with elastic bands (see Fig. 5). Additional layers of Styrofoam were added to prevent the logger and battery pack from rattling inside the box during transit from the field camp to the stream site. The 4-conductor electrode cable exited the box via a PG-9 cable gland mounted in a hole drilled through the field box. The batteries must be replaced with fully charged ones and the micro-SD card swapped out daily. While it is possible to replace the battery packs and micro-SD card in situ, on the stream bank, this often risks rain or drops of water from surrounding vegetation entering the enclosure. To mitigate this, the entire enclosure can be quickly disconnected from the cable and carried to and from the stream to the field camp, if the distance is not too great. Cable gland plugs can be inserted into the cable glands during transit to ensure the enclosure remains watertight.

**Figure 5.** Field transport of EOD loggers

<span style="font-size:0.5em;">Text goes here</span>

<span style="font-size:0.1em;">At our field site we retrieved all six EOD loggers daily to replace the battery packs and micro-SD cards, and to re-launch the Arduino IDE logging schedule on the loggers, before returning to the study site to reconnect the electrode cables. Note the use of Styrofoam casing to prevent the components from rattling inside the field boxes. Lok Poon, on the right, at the field camp near Caño Yahuarcaca, Leticia, Colombia.</span>


<img src="/resources/images/Picture9.png" width=80% height=80%>




