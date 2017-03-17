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
