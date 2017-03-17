---
layout: default
---

### Abstract

> In this report I describe the design, simulation and characterisation of a novel, high performance,
medium power ($\approx$ 50 W) class-B audio amplifier. Following a general discussion of amplifier characteristics
and design goals, the amplifier schematic is presented and described in detail. SPICE simulation results verify the design and provide further understanding of circuit behaviour. A brief discussion on the methodologies of good PCB design is followed by
a presentation of the key experimental characteristics of the amplifier along with a comparison to the characteristics
derived by simulation. An amplifier control board which responds to thermal and short-circuit faults and adds radio and alarm
clock features is briefly discussed. The final chapter `recommendations for future work' is a reflection upon the
strengths and weaknesses of the amplifier design and suggest possible means of improvement.

<br>

#### [Amplifier characteristics and design goals](#amplifier-characteristics-and-design-goals)
##### Amplifier design methodology
##### Miller capacitance

#### Amplifier Schematic description

<br>
<hr>

### Amplifier characteristics and design goals

Conceptually, an audio amplifier has a rather simple task to perform; it must faithfully reproduce an input signal  at a larger output power.
Audio amplifier parameters of particular interest from a user perspective are:

 * Cost
 * Continuous/momentary output power capability
 * Noise and hum
 * Thermal performance and reliability
 * Speaker short-circuit tolerance/detection
 * Closed-loop frequency response
 * Closed-loop gain
 * Electrical efficiency
 * Distortion

From a design perspective the following six parameters are also of particular interest:

 * Open-loop gain and phase delay as a function of frequency
 * Margin of electrical stability under various load conditions
 * Slew rate
 * Power supply rejection ratio
 * Thermal stability of quiescent currents
 * Open-loop linearity

Ultimately, the main design goals of the amplifier described here are: low-noise, low distortion, fast slew rate and good margin of electrical stability. It must be noted that the average human ear can not identify the distortion content of sound for distortion values
below about 1%. In addition, typical distortion values of a speaker are of the order of 1%. For these reasons the
pursuit of very low distortion audio amplifiers is usually of academic interest rather than of practical importance. Nothing
more will be mentioned regarding topics of an audiophile nature.


#### Amplifier design methodology

Open-loop non-linearity can be minimised to some degree, however, the inherent non-linearity of transistors and other components prevent a high degree of open-loop linearity. Also, load impedances usually have a frequency dependence and a possible degree of non-linearity. Therefore, the design of a low distortion amplifier mandates the implementation of a negative feedback circuit. A negative feedback circuit with a large loop gain minimises the impact of both non-linearity and noise sources by correcting for the effects of these perturbations. Take into consideration <a href="#01_negative_feedback">Fig. 1</a>, which is a block diagram of a negative feedback system that consist of linear sub-systems H(s) and F(s). $V_{d}$ is a disturbance voltage which represents the effects of non-linearity or electrical noise.


<center>
<figure id="01_negative_feedback">
<a href="{{ "/figures/01_negative_feedback.jpg" | prepend: site.baseurl }}">
<img src="{{ "/figures/01_negative_feedback.jpg" | prepend: site.baseurl }}" width="300">
</a>
<figcaption>Figure 1. Single-input, single-output negative feedback control loop where ${V}_{d}$ represents the effects of non-linearity or noise.</figcaption>
</figure>
</center>
<br>

Considering the influence of $V_{d}$, there is a change in the output voltage which is dependent upon the loop-gain,

<div id="feedback">
\begin{equation}
\frac{\Delta V_{out}}{V_{d}}=\frac{1}{1+F(s)H(s)}
\end{equation}
</div>

The disturbance voltage, $V_{d}$, can be considered to be the result of noise or
non-linearity. In either case the output voltage will reproduce the input voltage to a high degree of fidelity provided the loop-gain, $\textrm{H(s)F(s)}$, is large. As a consequence of the <a href="#feedback">feedback equation</a>, the noise of an amplifier is usually dominated by the noise of those sources that fall outside of the loop, i.e. the input components. The noise characteristics of the amplifier described in this report are measured and analysed in the <a href="#characterisation">characterisation section</a>. Another consequence of the <a href="#feedback">feedback equation</a> is the weak dependence of the closed-loop gain, A, on frequency,
\begin{equation}
\textrm{A} = \frac{V_{\textrm{out}}}{V_{\textrm{in}}}=\frac{H(s)}{1+H(s)F(s)}
\end{equation}
provided $\textrm{H(s)F(s)}>>1$, and the feedback function, $\textrm{F(s)}$ is
independent of frequency, which is usually the case where $\textrm{F(s)}$ is
simply a voltage divider consisting of resistors.
The above equations suggest the cascading of several amplifier stages such that a huge open-loop gain is achieved,
thereby, a low-noise, low distortion amplifier will result. However, the electrical stability of a negative feedback circuit needs to be ensured, otherwise it may oscillate or have excessive amounts of overshoot. The stability of a negative feedback circuit is dependent on both the loop-gain and phase delay. With increasing frequency the loop
phase delay inevitably increases and the loop-gain decreases. A condition for stability is a loop-gain,
$\textrm{H(s)F(s)}$, of less than 1 at the frequency where the phase delay has increased to 180 degrees. The frequency
at which the loop-gain decreases to unity is defined as the cross-over frequency, $\omega_{c}$, and the phase margin
is defined as $180-\phi_{delay}(\omega_{c})$. As an example see <a href="#02_OPA350">Fig. 2</a> which is a plot of gain and phase of
the OPA350 opamp which is unity gain stable since the phase delay is about 120 degrees (phase margin of 60 degrees) at the cross-over frequency of 38 Mhz.

<center>
<figure id="02_OPA350">
<a href="{{ "/figures/02_OPA350.jpg" | prepend: site.baseurl }}">
<img src="{{ "/figures/02_OPA350.jpg" | prepend: site.baseurl }}" width="500">
</a>
<figcaption>Figure 2. Open-loop gain and phase delay as a function of frequency of the OPA350 opamp.</figcaption>
</figure>
</center>

Stabilisation of an amplifier is usually achieved by including a compensation capacitor which reduces the amplifier gain
with increasing frequency such that at the crossover frequency the phase margin is somewhere about 50 degrees or more.
A high-performance amplifier has a large loop-gain for the frequencies of interest, and therefore the crossover frequency
should be relatively high so that the gain of the amplifier is not excessively curtailed by the action of the
compensation capacitance within the frequency band of interest. As a consequence of the necessity of electrical
stability, adding an arbitrarily large number of gain stages to an amplifier is not a strategy that produces good
results since this will result in excessive phase delay. A larger phase delay requires a smaller cross-over frequency to maintain stability. Therefore, the gain within the audio band will be excessively limited by the compensation capacitance.


#### Miller capacitance

<center>
<figure id="03_miller_schematic">
<a href="{{ "/figures/03_miller_schematic.jpg" | prepend: site.baseurl }}">
<img src="{{ "/figures/03_miller_schematic.jpg" | prepend: site.baseurl }}" width="350">
</a>
<figcaption>Figure 3.
Circuit schematic that is referred to in the discussion of miller capacitance with SPICE simulations shown in <a href="#04_miller_plot">Fig. 4</a>.
</figcaption>
</figure>
</center>

Before discussing the amplifier schematic the effect of miller capacitance
will be quantified since it has a considerable impact upon amplifier design and performance through the phase delay that it induces.
Referring to <a href="#03_miller_schematic">Fig. 3</a>, and labelling the AC component of the voltages,
\begin{eqnarray}
  v_{s}(t) = V_{S}e^{i\omega t}\\\
  v_{b}(t) = V_{B}e^{i\omega t}e^{i\phi_{B}}\\\
  v_{e}(t) = v_{b}(t)\\\
  v_{c}(t) = V_{C}e^{i\omega t}e^{i\phi_{C}}
\end{eqnarray}
the output voltage, $v_{c}(t)$, as a function of frequency is,
\begin{equation}
v_{c}(t) = v_{S}(t)\frac{-R_{L}\left(\frac{\beta}{\beta+1}\frac{1}{R_{E}}-i\omega C\right)}{1+\frac{R_{S}}{R_{E}}\frac{1}{\beta+1} - i\omega\left[R_{L}C\left(1+\frac{R_{S}}{R_{E}}\right)+R_{S}C\right]-2\omega^{2}C^{2}R_{L}R_{s}}
\end{equation}
For the above equation it is assumed that the transistor has a finite current gain, $\beta$, but is otherwise ideal and has an infinite frequency response. As such, the miller effect and frequency response is modelled
by the external capacitor, C as shown in the schematic of <a href="#03_miller_schematic">Fig. 3</a>. For small $\omega$,
\begin{equation}
v_{c}(t) = -v_{s}(t)\frac{\frac{\beta}{\beta + 1}\frac{R_{L}}{R_{E}}}{1 + \frac{R_{S}}{R_{E}}\frac{1}{\beta + 1}}
\end{equation}
For $\beta>>1$ and $R_{S}<<\beta R_{E}$ the gain is closely described by the familiar relationship, $R_{L}/R_{E}$.
The cut-off frequency is,
\begin{equation}
\omega = \frac{1}{R_{L}C\left(1+\frac{R_{S}}{R_{E}}\right)+R_{S}C}
\end{equation}
If $R_{L}>>R_{E}$, i.e. the single transistor has significant low-frequency gain the cut-off
is approximately inversely proportional to the voltage gain multiplied by the capacitance and input resistance, $R_{S}$. The phase delay is,
\begin{equation}
\phi=\tan^{-1}\frac{\omega R_{L}C\left(\frac{1}{R_{E}}\left(R_{L}\left(1+\frac{R_{S}}{R_{E}}\right)+R_{S}\right)+2\omega^{2}C^{2}R_{L}R_{S}-1\right) }{\frac{R_{L}}{R_{E}}\left(1+\frac{R_{S}}{R_{E}}\frac{1}{\beta}-\omega^{2}C^{2}R_{L}R_{S}\right)+R_{L}\omega^{2}C^{2}(R_{L}+R_{S})}
\end{equation}
Taking the 1st order $\omega$ terms the above equation approximates to,
\begin{equation}
\phi = \tan^{-1}\omega C\left(R_{L}\left(1+\frac{R_{S}}{R_{E}}\right)+R_{S}\right)
\end{equation}
The bandwidth of the single transistor amplifier is determined by $C$, which is transistor dependent, the voltage gain and input resistance, R$_{\textrm{S}}$.
The important point to note is that phase delay increases with increasing voltage gain and input resistance.
It is important to consider the phase properties of the voltage amplification stage of an amplifier and eliminate any unnecessary voltage amplification of all other stages. SPICE simulations of the single transistor amplifier shown in <a href="#03_miller_schematic">Fig. 3</a> with and without additional external capacitance are plotted in <a href="#04_miller_plot">Fig. 4</a>, demonstrating the impact of voltage gain on phase delay and bandwidth.

<center>
<figure id="04_miller_plot">
<a href="{{ "/figures/04_miller_plot.png" | prepend: site.baseurl }}">
<img src="{{ "/figures/04_miller_plot.png" | prepend: site.baseurl }}" width="500">
</a>
<figcaption>Figure 4.
SPICE simulations that demonstrate the impact of miller capacitance on the bandwidth and phase of the circuit shown in <a href="#03_miller_schematic">Fig. 3</a>. Solid lines plot the current signal through RL and dashed lines plot the phase delay.
</figcaption>
</figure>
</center>


### Amplifier Schematic description

The schematic of the amplifier as constructed and characterised is shown in Fig \ref{sch}.
It consists of four stages, two differential, an output driver, and output stage. The differential gain stages operate in a transconductance mode, i.e. neither are required to generate a significant voltage signal, rather, they are viewed as producing a current signal in response to a small magnitude input voltage signal. The output driver stage consisting of transistors KSA1220A (Q19) and KSC2690A (Q21) is the voltage amplification stage (VAS) and the output stage is a unity gain buffer. C16 (47 pF) connected between the output and the 1st differential stage output is the compensation capacitance. The closed-loop gain is 27k/(470$\times$2) = 28.7 = 29 dB, hence, full-power corresponds to input voltages of 700 mV RMS.

The first differential stage incorporates a current mirror which allows straight forward coupling to the 2nd stage input. In addition, the current mirror doubles the transconductance of the 1st stage and hence the low-frequency open-loop gain. Both differential stages are biased by constant current sources and the quiescent current of the 2nd differential stage biases the output driver stage which in-turn biases the output transistors. Therefore, the quiescent current of the 2nd differential
stage determines the quiescent current of the output driver stage. Hereafter, the quiescent currents of the 1st differential, 2nd differential, output driver and output stages are labelled $I_{1}, I_{2}, I_{D}, $
and $I_{O}$ respectively. The two constant current sources of the 2nd stage are assumed to be near equal in magnitude. These constant current sources are adjusted by trimmer potentiometers (R21 and R28) such that $I_{D}$ is set to a desired value.
$I_{O}$ is limited by a BD139 transistor placed across the bases of the output transistors. The BD139 should be in close thermal contact with the output transistors so that there are minimal variations to the relative $V_{BE}$ across temperature,
therefore, the sensitivity of $I_{O}$ to temperature change is minimised. The sensitivity of $I_{O}$ to temperature has been measured, see the characterisation section.

<center>
<figure id="05_amp_schematic">
<a href="{{ "/figures/05_amp_schematic.png" | prepend: site.baseurl }}">
<img src="{{ "/figures/05_amp_schematic.png" | prepend: site.baseurl }}">
</a>
<figcaption>Figure 5. Amplifier schematic diagram.
</figcaption>
</figure>
</center>


The two differential stages operate from supply voltages that are different to that of the VAS and output stage. These supply voltages are generated by zener diode based reference voltages that are buffered by transistors. The positive and negative supplies have
nominal voltages of 15V and -7V respectively. Separate supplies keep the thermal dissipation of the differential stage transistors to within acceptable limits, reduces voltage stress and broadens the set of transistors that can be utilised
by the differential stages.

The input is filtered by a simple RC network (R9, C9) which serves two purposes; firstly
it attenuates frequency components of the input signal that lie beyond the upper end of the audio band, secondly it blocks high frequency signals on the input that originate from radiated emissions. The second point is important since a possible amplifier oscillation mode results from the emission of EM radiation via the output cabling, which is picked up by the input cabling. Therefore, the simple RC network may be required to prevent high-frequency oscillations.

Ground currents have the potential to induce hum on the amplifier output, an issue that may be the result of a poor PCB layout. The amplifier input is driven by a separate control board which contains a volume control I.C. (PGA2311). If ground currents produce
a small potential difference between the ground planes of the amplifier and control boards, this will result in hum since the control board output signal is with respect to a potential that is slightly different to the amplifier ground potential.
The amplifier contains input circuitry which essentially eliminates the possibility of the above mentioned source of hum. The ground potential of the control board is feed into the inverting input of the amplifier, and a portion of it which
comes from the R17, R18 voltage divider is feed into the non-inverting input. As such, any ground potential differences do not result in an amplifier output voltage with respect to its ground potential.

As mentioned previously, maintaining a large gain within the audio band while minimising phase delay is a necessary requirement for the design of a high performance amplifier. The amplifier phase delay is minimised about the unity loop-gain point through 2 main mechanisms; the minimisation of the miller capacitance effect and the phase lead resulting from capacitors placed within the second differential stage and the output driver transistor emitter capacitance. C2 and C3 of the 1st differential stage increase the open-loop gain at about 10 kHz. The impedance ratio of C3 in parallel with R7 (C3$\\parallel$R7) and C2$\\parallel$R5 becomes significant at about 10 kHz, therefore the current signal
of Q5 is greater than Q3. At higher frequencies the dynamic impedance of the emitter-base junctions of Q3 and Q5 become greater than C3$\\parallel$R7 and C2$\\parallel$R5, therefore the current signals in the current mirror become more symmetrical. C19 and C22 of the 2nd differential stage increase the phase margin about the cross-over frequency. Likewise, C25 and C33 (output driver stage) improve the phase margin, but to a leaser extent. Miller phase delay is minimised by the direct coupling of the two differential stages and the voltage amplification stage at higher frequencies through C18, C21, C28 and C31.

The 1.2pF capacitor (C10) that is in parallel with the feedback resistor (R8) greatly improves the stability margin of the amplifier. It is expected that the base capacitance of the input transistor, Q6, in addition to any parasitic capacitance induces a phase delay at its base. This contribution to the phase delay can be eliminated by including a small capacitance in parallel with the feedback resistor. The MMBT6429 data sheet indicates that the typical collector-base capacitance is less than 2 pF at V$_{CB}$ = 5 V. Given the small voltage signal on the collector of Q6, the 1.2 pF capacitor must enhance stability primarily because of a generated phase lead.


The quiescent currents $I_{1}$,  $I_{2}$, $I_{D}$, and $I_{O}$, have an impact upon circuit performance and stability. Increasing $I_{1}$ increases the open-loop gain across the entire frequency span and increases the slew rate since the transconductance of the 1st stage is proportional to $I_{1}$. The slew rate is ultimately limited by the amount of current that the 1st differential stage can provide to offset the current through the compensation capacitor,

<figcaption id="slew">Slew equation</figcaption>
\begin{equation}\label{slew_Eq}
\frac{dv}{dt} =\frac{I_{1}}{47\,pF}
\end{equation}

Since the 2nd differential stage requires little input current (in comparison with $I_{1}$), even when the output is driving a large voltage across an 8 $\Omega$ load, the actual slew rate is expected to be close to the value given by the <a href="#slew">slew equation</a>. For $I_{1}=10$ mA this equates to a slew rate of 213 V$/\mu s$.
Essentially, $I_{1}$ should be set to larger values provided stability is not compromised. The phase delay of the driver stage is dependent on $I_{D}$ since the gain-bandwidth product of Q19 and Q21 peak at a particular value of $I_{C}$
and the collector-base capacitance has a dependence on collector current. It is found that $I_{D}$ = 80 mA is a good compromise between minimising phase delay without setting an excessive quiescent current. Through SPICE simulations it is found that reducing $I_{D}$ increases the phase delay around the cross-over frequency while having less impact upon the open-loop gain, therefore the phase margin decreases.
There appears to be no point to setting $I_{O}$ beyond a small value. When both output transistors are conducting the transconductance of the output stage is double that of when only a single transistor is conducting. Transitions between these two transconductance modes is a source of distortion.

The open-loop gain and phase, hence, stability depend upon both the magnitude and phase of the load impedance. Speakers and cabling represent quite an unpredictable load. A common practice is to isolate the speaker load at high frequencies and add an additional load with properties that ensure stability. R37 and C30 result in a resistance of 10 $\Omega$ across the amplifier output at high-frequencies, aiding in stability.

The amplifier incorporates basic speaker short-circuit protection to prevent damage to the output transistors and power supply that may otherwise occur if the output is short-circuited. Q22 and Q23 detect excessive driver transistor current and turn on. These signals are detected by the amplifier control board in the form of a MCU interrupt, resulting in rapid shutdown of the amplifier power supply.

More analytic detail on the amplifier gain is found in the Appendix.
