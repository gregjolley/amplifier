---
layout: page
title: Project
permalink: /project/
---


\documentclass[a4paper,10pt, oneside]{article}
\usepackage[utf8]{inputenc}
\usepackage{graphicx}
\usepackage{wrapfig}
\usepackage{lscape}
\usepackage{rotating}
\usepackage{epstopdf}
\usepackage{amsmath,amssymb,mathtools}
\usepackage{float}
\usepackage{hyperref}
\setcounter{secnumdepth}{3}

\setlength{\arrayrulewidth}{0.3mm}
\setlength{\tabcolsep}{8pt}
\renewcommand{\arraystretch}{1.2}


\addtolength{\voffset}{-20mm}
\addtolength{\hoffset}{-25mm}
\textwidth = 170mm
\textheight = 250mm

%opening
\title{The design, simulation and characterisation of a class-B audio power amplifier}
\author{G. Jolley}

\begin{document}

\maketitle

\begin{abstract}
In this report I describe the design, simulation and characterisation of a novel, high performance,
medium power ($\approx$ 50 W) class-B audio amplifier. Following a general discussion of amplifier characteristics
and design goals, the amplifier schematic is presented and described in detail. SPICE simulation results verify the design and provide further understanding of circuit behaviour. A brief discussion on the methodologies of good PCB design is followed by
a presentation of the key experimental characteristics of the amplifier along with a comparison to the characteristics
derived by simulation. An amplifier control board which responds to thermal and short-circuit faults and adds radio and alarm
clock features is briefly discussed. The final chapter `recommendations for future work' is a reflection upon the
strengths and weaknesses of the amplifier design and suggest possible means of improvement.
\end{abstract}
\section{Amplifier characteristics and design goals}
Conceptually, an audio amplifier has a rather simple task to perform; it must faithfully reproduce an input signal  at a larger output power.
Audio amplifier parameters of particular interest from a user perspective are:
\begin{itemize}
 \item Cost
 \item Continuous/momentary output power capability
 \item Noise and hum
 \item Thermal performance and reliability
 \item Speaker short-circuit tolerance/detection
 \item Closed-loop frequency response
 \item Closed-loop gain
 \item Electrical efficiency
 \item Distortion
\end{itemize}
From a design perspective the following six parameters are also of particular interest:
\begin{itemize}
	\item Open-loop gain and phase delay as a function of frequency
	\item Margin of electrical stability under various load conditions
	\item Slew rate
	\item Power supply rejection ratio
	\item Thermal stability of quiescent currents
	\item Open-loop linearity
\end{itemize}
Ultimately, the main design goals of the amplifier described here are: low-noise, low distortion, fast slew rate and good margin of electrical stability. It must be noted that the average human ear can not identify the distortion content of sound for distortion values
below about 1\%. In addition, typical distortion values of a speaker are of the order of 1\%. For these reasons the
pursuit of very low distortion audio amplifiers is usually of academic interest rather than of practical importance. Nothing
more will be mentioned regarding topics of an audiophile nature.
\subsection*{Amplifier design methodology}
Open-loop non-linearity can be minimised to some degree, however, the inherent non-linearity of transistors and other components prevent a high degree of open-loop linearity. Also, load impedances usually have a frequency dependence and a possible degree of non-linearity. Therefore, the design of a low distortion amplifier mandates the implementation of a negative feedback circuit. A negative feedback circuit with a large loop gain minimises the impact of both non-linearity and noise sources by correcting for the effects of these perturbations. Take into consideration Fig. \ref{feedback}, which is a block diagram of a negative feedback system that consist of linear sub-systems H(s) and F(s). $\textrm{V}_{d}$ is a disturbance voltage which represents the effects of non-linearity or electrical noise.
\begin{figure}
\vspace{-2cm}
\begin{centering}
  % Requires \usepackage{graphicx}
  \includegraphics[height=3.5cm]{negative_feedback.jpg}
  \caption{Single-input, single-output negative feedback control loop
  	where $\textrm{V}_{d}$ represents the effects of non-linearity or noise.}\label{feedback}
\end{centering}
\end{figure}
Considering the influence of $\textrm{V}_{d}$, there is a change in the output voltage which is dependent upon the loop-gain,
\begin{equation}
\frac{\Delta V_{\textrm{out}}}{V_{\textrm{d}}}=\frac{1}{1+F(s)H(s)}
\end{equation}
The disturbance voltage, $\textrm{V}_{d}$, can be considered to be the result of noise or
non-linearity. In either case the output voltage will reproduce the input voltage to a high degree of fidelity provided the loop-gain, $\textrm{H(s)F(s)}$, is large. As a consequence of Eq. \ref{feedback}, the noise of an amplifier is usually dominated by the noise of those sources that fall outside of the loop, i.e. the input components. The noise characteristics of the amplifier described in this report are measured and analysed in the characterisation section~\ref{sec:characterisation}. Another consequence of Eq. \ref{feedback} is the weak dependence of the closed-loop gain, A, on frequency,
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
is defined as $180-\phi_{delay}(\omega_{c})$. As an example see Fig. \ref{OPA350} which is a plot of gain and phase of
the OPA350 opamp which is unity gain stable since the phase delay is about 120 degrees (phase margin of 60 degrees) at the cross-over frequency of 38 Mhz.

\begin{figure}[H]
\begin{centering}
	% Requires \usepackage{graphicx}
	\includegraphics[width=10cm]{OPA350.jpg}
	\caption{Open-loop gain and phase delay as a function of frequency of the OPA350 opamp.}\label{OPA350}
\end{centering}
\end{figure}

Stabilisation of an amplifier is usually achieved by including a compensation capacitor which reduces the amplifier gain
with increasing frequency such that at the crossover frequency the phase margin is somewhere about 50 degrees or more.
A high-performance amplifier has a large loop-gain for the frequencies of interest, and therefore the crossover frequency
should be relatively high so that the gain of the amplifier is not excessively curtailed by the action of the
compensation capacitance within the frequency band of interest. As a consequence of the necessity of electrical
stability, adding an arbitrarily large number of gain stages to an amplifier is not a strategy that produces good
results since this will result in excessive phase delay. A larger phase delay requires a smaller cross-over frequency to maintain stability. Therefore, the gain within the audio band will be excessively limited by the compensation capacitance.
\subsubsection*{Miller capacitance}
\begin{figure}[H]
\begin{centering}
	% Requires \usepackage{graphicx}
	\includegraphics[width=8cm]{miller_schematic}
	\caption{Circuit schematic that is referred to in the discussion of miller capacitance with SPICE simulations shown in Fig. \ref{miller}.}\label{miller_sch}
\end{centering}
\end{figure}
Before discussing the amplifier schematic the effect of miller capacitance
will be quantified since it has a considerable impact upon amplifier design and performance through the phase delay that it induces.
Referring to figure \ref{miller_sch}, and labelling the AC component of the voltages,
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
by the external capacitor, C as shown in the schematic of Fig. \ref{miller_sch}. For small $\omega$,
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
It is important to consider the phase properties of the voltage amplification stage of an amplifier and eliminate any unnecessary voltage amplification of all other stages. SPICE simulations of the single transistor amplifier shown in Fig. \ref{miller_sch} with and without additional external capacitance are plotted in Fig. \ref{miller}, demonstrating the impact of voltage gain on phase delay and bandwidth.
\begin{figure}[H]
\begin{centering}
	% Requires \usepackage{graphicx}
	\includegraphics[width=11cm]{miller_plot}
	\caption{SPICE simulations that demonstrate the impact of miller capacitance on the bandwidth and phase of the circuit shown in Fig. \ref{miller_sch}. Solid lines plot the current signal through RL and dashed lines plot the phase delay.}\label{miller}
\end{centering}
\end{figure}


\section{Amplifier Schematic description}
The schematic of the amplifier as constructed and characterised is shown in Fig \ref{sch}.
It consists of four stages, two differential, an output driver, and output stage. The differential gain stages operate in a transconductance mode, i.e. neither are required to generate a significant voltage signal, rather, they are viewed as producing a current signal in response to a small magnitude input voltage signal. The output driver stage consisting of transistors KSA1220A (Q19) and KSC2690A (Q21) is the voltage amplification stage (VAS) and the output stage is a unity gain buffer. C16 (47 pF) connected between the output and the 1st differential stage output is the compensation capacitance. The closed-loop gain is 27k/(470$\times$2) = 28.7 = 29 dB, hence, full-power corresponds to input voltages of 700 mV RMS.

The first differential stage incorporates a current mirror which allows straight forward coupling to the 2nd stage input. In addition, the current mirror doubles the transconductance of the 1st stage and hence the low-frequency open-loop gain. Both differential stages are biased by constant current sources and the quiescent current of the 2nd differential stage biases the output driver stage which in-turn biases the output transistors. Therefore, the quiescent current of the 2nd differential
stage determines the quiescent current of the output driver stage. Hereafter, the quiescent currents of the 1st differential, 2nd differential, output driver and output stages are labelled $\textrm{I}_{1}, \textrm{I}_{2}, \textrm{I}_{\textrm{D}}, $
and $\textrm{I}_{\textrm{O}}$ respectively. The two constant current sources of the 2nd stage are assumed to be near equal in magnitude. These constant current sources are adjusted by trimmer potentiometers (R21 and R28) such that $\textrm{I}_{\textrm{D}}$ is set to a desired value.
$\textrm{I}_{\textrm{O}}$ is limited by a BD139 transistor placed across the bases of the output transistors. The BD139 should be in close thermal contact with the output transistors so that there are minimal variations to the relative $V_{\textrm{BE}}$ across temperature,
therefore, the sensitivity of $\textrm{I}_{\textrm{O}}$ to temperature change is minimised. The sensitivity of $\textrm{I}_{\textrm{O}}$ to temperature has been measured, see the characterisation section.
\begin{figure}[H]
\begin{centering}
\vspace{-2cm}
\hspace*{-1.3cm}
	% Requires \usepackage{graphicx}
	\includegraphics[width=26cm, angle=-90]{amp_schematic}
	\caption{Amplifier schematic diagram}\label{sch}
\end{centering}
\end{figure}
The two differential stages operate from supply voltages that are different to that of the VAS and output stage. These supply voltages are generated by zener diode based reference voltages that are buffered by transistors. The positive and negative supplies have
nominal voltages of 15V and -7V respectively. Separate supplies keep the thermal dissipation of the differential stage transistors to within acceptable limits, reduces voltage stress and broadens the set of transistors that can be utilised
by the differential stages.

The input is filtered by a simple RC network (R9, C9) which serves two purposes; firstly
it attenuates frequency components of the input signal that lie beyond the upper end of the audio band, secondly it blocks high frequency signals on the input that originate from radiated emissions. The second point is important since a possible amplifier oscillation mode results from the emission of EM radiation via the output cabling, which is picked up by the input cabling. Therefore, the simple RC network may be required to prevent high-frequency oscillations.

Ground currents have the potential to induce hum on the amplifier output, an issue that may be the result of a poor PCB layout. The amplifier input is driven by a separate control board which contains a volume control I.C. (PGA2311). If ground currents produce
a small potential difference between the ground planes of the amplifier and control boards, this will result in hum since the control board output signal is with respect to a potential that is slightly different to the amplifier ground potential.
The amplifier contains input circuitry which essentially eliminates the possibility of the above mentioned source of hum. The ground potential of the control board is feed into the inverting input of the amplifier, and a portion of it which
comes from the R17, R18 voltage divider is feed into the non-inverting input. As such, any ground potential differences do not result in an amplifier output voltage with respect to its ground potential.

As mentioned previously, maintaining a large gain within the audio band while minimising phase delay is a necessary requirement for the design of a high performance amplifier. The amplifier phase delay is minimised about the unity loop-gain point through 2 main mechanisms; the minimisation of the miller capacitance effect and the phase lead resulting from capacitors placed within the second differential stage and the output driver transistor emitter capacitance. C2 and C3 of the 1st differential stage increase the open-loop gain at about 10 kHz. The impedance ratio of C3 in parallel with R7 (C3$||$R7) and C2$||$R5 becomes significant at about 10 kHz, therefore the current signal
of Q5 is greater than Q3. At higher frequencies the dynamic impedance of the emitter-base junctions of Q3 and Q5 become greater than C3$||$R7 and C2$||$R5, therefore the current signals in the current mirror become more symmetrical. C19 and C22 of the 2nd differential stage increase the phase margin about the cross-over frequency. Likewise, C25 and C33 (output driver stage) improve the phase margin, but to a leaser extent. Miller phase delay is minimised by the direct coupling of the two differential stages and the voltage amplification stage at higher frequencies through C18, C21, C28 and C31.

The 1.2pF capacitor (C10) that is in parallel with the feedback resistor (R8) greatly improves the stability margin of the amplifier. It is expected that the base capacitance of the input transistor, Q6, in addition to any parasitic capacitance induces a phase delay at its base. This contribution to the phase delay can be eliminated by including a small capacitance in parallel with the feedback resistor. The MMBT6429 data sheet indicates that the typical collector-base capacitance is less than 2 pF at V$_{\textrm{CB}}$ = 5 V. Given the small voltage signal on the collector of Q6, the 1.2 pF capacitor must enhance stability primarily because of a generated phase lead.


The quiescent currents $\textrm{I}_{1}$,  $\textrm{I}_{2}$, $\textrm{I}_{\textrm{D}}$, and $\textrm{I}_{\textrm{O}}$, have an impact upon circuit performance and stability. Increasing $\textrm{I}_{1}$ increases the open-loop gain across the entire frequency span and increases the slew rate since the transconductance of the 1st stage is proportional to $\textrm{I}_{1}$. The slew rate is ultimately limited by the amount of current that the 1st differential stage can provide to offset the current through the compensation capacitor,
\begin{equation}\label{slew_Eq}
\frac{dv}{dt} =\frac{\textrm{I}_{1}}{47\,\textrm{pF}}
\end{equation}
Since the 2nd differential stage requires little input current (in comparison with $\textrm{I}_{1}$), even when the output is driving a large voltage across an 8 $\Omega$ load, the actual slew rate is expected to be close to the value given by Eq. \ref{slew_Eq}. For $\textrm{I}_{1}=10$ mA this equates to a slew rate of 213 V$/\mu s$.
Essentially, $\textrm{I}_{1}$ should be set to larger values provided stability is not compromised. The phase delay of the driver stage is dependent on $\textrm{I}_\textrm{{D}}$ since the gain-bandwidth product of Q19 and Q21 peak at a particular value of $\textrm{I}_{\textrm{C}}$
and the collector-base capacitance has a dependence on collector current. It is found that $\textrm{I}_\textrm{{D}}$ = 80 mA is a good compromise between minimising phase delay without setting an excessive quiescent current. Through SPICE simulations it is found that reducing $\textrm{I}_\textrm{{D}}$ increases the phase delay around the cross-over frequency while having less impact upon the open-loop gain, therefore the phase margin decreases.
There appears to be no point to setting $\textrm{I}_{\textrm{O}}$ beyond a small value. When both output transistors are conducting the transconductance of the output stage is double that of when only a single transistor is conducting. Transitions between these two transconductance modes is a source of distortion.

The open-loop gain and phase, hence, stability depend upon both the magnitude and phase of the load impedance. Speakers and cabling represent quite an unpredictable load. A common practice is to isolate the speaker load at high frequencies and add an additional load with properties that ensure stability. R37 and C30 result in a resistance of 10 $\Omega$ across the amplifier output at high-frequencies, aiding in stability.

The amplifier incorporates basic speaker short-circuit protection to prevent damage to the output transistors and power supply that may otherwise occur if the output is short-circuited. Q22 and Q23 detect excessive driver transistor current and turn on. These signals are detected by the amplifier control board in the form of a MCU interrupt, resulting in rapid shutdown of the amplifier power supply.

More analytic detail on the amplifier gain is found in the Appendix.

\section{SPICE simulations}
The circuit for SPICE analysis is shown in Fig. \ref{sim_sch}, all references to components in this section are with respect to this schematic diagram and the component values for all simulations are as they appear unless otherwise stated. LTspice (\url{www.linear.com/designtools/software}) was used for all SPICE simulations. Small signal AC transfer simulations of the closed-loop gain and phase for the optimised component values as shown in the simulation schematic is shown in Fig.\ref{closed}. In-addition, plots are shown for the feedback capacitor, C15=0. The benefits of C15 are clearly evident by the elimination of gain peaking at about 8 MHz and reduction of the phase delay at about the cross-over frequency. Transient simulations indicate that the full-power bandwidth is a little over 1 MHz.

The open-loop gain and phase for the optimised component values is shown in Fig.\ref{optimised}. The two differential gain stages results in a very large voltage gain of 130 dB at frequencies less than 100 Hz. The gain reduces with increasing frequency beyond 100 Hz due to the 47 pF compensation capacitor.
The reduction of open-loop gain with increasing frequency (aside from the influence of other capacitors such as C1 and C2) can considered as the result of the reduction of the load impedance at the output of the 1st differential stage,
\begin{equation}
\textrm{gain} \propto \frac{1}{Z}=\frac{1}{\omega\times 47\,\textrm{pF}}
\end{equation}
which equates to -6 dB per octave. The inclusion of capacitors C1 and C2 pushes the gain at 10 kHz to a little over 100 dB.
The cross-over frequency is modified by C16 which increases the closed-loop gain at higher frequencies to 27k/470 = 57 = 35 dB, giving
a cross-over frequency a little under 10 MHz and a phase margin of 80 degrees, though the phase margin minimum is 69 degrees at 170 kHz.


The frequency at which the open-loop gain begins to decrease is given by the relationship,
\begin{equation}
\omega\times 47\,\textrm{pF}\times\textrm{gm}_{2}\times\textrm{gm}_{\textrm{D}}\times\textrm{rm}_{\textrm{O}}= Z_{out_{1} }
\end{equation}
Where gm$_{2}$ is the transconductance of the 2nd stage, gm$_{\textrm{D}}$ is the transconductance of the driver stage, rm$_{\textrm{O}}$ is the transresistance of the output stage and $Z_{out_{1}}$ is the  impedance as seen at the output of the 1st differential stage. $Z_{out_{1}}$ is the result of the early voltage of Q5 and the load impedance of the 2nd stage input. For more detail see the appendix.

Various transient waveforms for a 1 kHz input signal are shown in Fig \ref{waveforms}. What is interesting is the very non-sinusoidal waveforms of the 1st stage output and the base voltage of Q17. It turns out that the rather small early voltages of the output transistors is the primary influence on the shape of these waveforms. As a function of collector current the current gain of the output transistors are very linear, however, the current gain has quite a strong dependence on V$_{\textrm{CE}}$ leading to a output stage transresistance that is non-linear and dependent on the amplifier output voltage. The voltage on the base of Q4 is close to sinusoidal because the compensation capacitance linearises the load on the 1st stage output. Simulations were performed for a 50 Hz sine wave input of several different magnitudes such that the compensation capacitance has negligible influence on the gain. The low-frequency was chosen to investigate the gain of the various stages of the amplifier as a function of output magnitude. A summary of the peak-peak magnitudes of various signals is shown in table \ref{table:1} and stage gains are shown in table \ref{table:2}. The voltage gain of the 1st differential stage is given by,
\begin{equation}
\frac{\Delta V_{B}Q10}{\Delta V_{B}Q4}
\end{equation}
The transconductance of the 2nd stage is given by,
\begin{equation}
\frac{\Delta I_{C}Q11}{\Delta V_{B}Q10}
\end{equation}
The current gain of the driver stage is given by,
\begin{equation}
\frac{\Delta I_{C}Q17}{\Delta I_{C}Q13}
\end{equation}
The transresistance of the output stage is given by,
\begin{equation}
\frac{\Delta V_{\textrm{out}}}{\Delta I_{C}Q17 + \Delta I_{C}Q22}
\end{equation}



It is evident that the gain of all stages apart from the output are largely independent of output magnitude. The transresistance of the output stage depends on the load impedance and current gain of the output transistors. The early voltages of the driver transistors have little impact upon the combined transresistance of the driver and output stages due to the emitter resistors, R25 and R31.


\vspace{5mm}
\begin{figure}[H]
\begin{centering}
	% Requires \usepackage{graphicx}
	\includegraphics[width=24cm, angle=-90]{sim_sch.jpg}
	\caption{Amplifier schematic diagram}\label{sim_sch}
\end{centering}
\end{figure}
\begin{table}
\begin{centering}
\begin{tabular}{|c|c|c|c|c|c|}
	\hline
	Vout & V$_{\textrm{B}}$ Q4 ($\mu$V) & V$_{\textrm{B}}$ Q10 (mV) & I$_{\textrm{C}}$ Q11+Q13 (mA) & I$_{\textrm{C}}$ Q17 (mA) & I$_{\textrm{C}}$ Q22 (mA) \\ [0.5ex]
	\hline\hline 10 & 3.5  & 27.5 & 0.278 & 3.18 & 3.51 \\
	\hline 20 & 7.68 & 60.9 & 0.616 & 7.18 & 7.84 \\
	\hline 30 & 12.8 & 102  & 1.03 & 12.3 & 13.2 \\
	\hline 40 & 19.2 & 153  & 1.55 & 18.8 & 20.0 \\
	\hline 50 & 27.3 & 219  & 2.21  & 27.2 & 28.6 \\
	\hline 60 & 38.1 & 305  & 3.07  & 38.4 & 40.0 \\
	\hline
\end{tabular}
\caption{Summary of various signals, all magnitudes given as peak-peak values.}
\label{table:1}
\end{centering}
\end{table}
\vspace{5mm}

\begin{table}
\begin{centering}
\begin{tabular}{|c|c|c|c|c|c|}
	\hline
	Vout & Av 1st-stage & $g_{m}$ 2nd-stage & A$_{\textrm{I}}$ driver & $r_{m}$ output stage  & total open-loop gain (dB)\\ [0.3ex]
	\hline\hline 10 & 7860 & 0.0101 &25.3& 1490 & 129.4\\
	\hline 20 & $7930$ & 0.0101 &25.5& 1330 & 128.3\\
	\hline 30 & $7970$ & 0.0101 &25.7& 1180 & 127.4\\
	\hline 40 & $7970$ & 0.0101 &26.0& 1030 & 126.4\\
	\hline 50 & $8020$ & 0.0101 &25.9& 896 & 125.3\\
	\hline 60 & $8010$ & 0.0101 &26.6& 765 & 123.9\\
	\hline
\end{tabular}
\caption{Summary of gain for various output voltage magnitudes. Av 1st-stage, $g_{m}$ 2nd-stage, A$_{\textrm{I}}$ and $r_{m}$ output stage are the voltage gain of the 1st differential stage, transconductance of the 2nd stage, current gain of the driver and transresistance of the output stage respectively. The transconductance of the 2nd stage is calculated by adding the collector signal current of both Q10 and Q12.}
\label{table:2}
\end{centering}
\end{table}
The response to a square wave input with the input filter capacitor (C16) removed is shown in Fig. \ref{slew} indicating a slew rate of 230\,V/$\mu$S which is is close to the upper limit calculated in the amplifier schematic description section.

A long transient simulation with a small time-step was performed for a 10 kHz input and a FFT was taken of the output waveform to obtain an indication of the amount of amplifier distortion, see Fig. \ref{fft}

\begin{figure}[H]
\begin{centering}
  % Requires \usepackage{graphicx}
  \includegraphics[width=13cm]{closed_optimised.pdf}
  \caption{Closed-loop gain and phase for optimised component values and for C18 = 0. The advantage of
  	including a 1.2 pF capacitor within the feed-back loop is clearly seen by the reduction of gain peaking and
  	improved phase margin for frequencies around cross-over.}\label{closed}
\end{centering}
\end{figure}

\begin{figure}[H]
\begin{centering}
	% Requires \usepackage{graphicx}
	\includegraphics[width=11cm]{open_optimised.pdf}
	\caption{Open-loop gain and phase margin for optimised component values.}\label{optimised}
\end{centering}
\end{figure}

\begin{figure}[H]
\begin{centering}
  % Requires \usepackage{graphicx}
  \includegraphics[width=11cm]{openloop_plots.pdf}
  \caption{Open-loop gain and phase plots for optimised, C1 = C2 = 0 and C3 = C4 = 0}\label{C8}
\end{centering}
\end{figure}

\begin{figure}[H]
\begin{centering}
  % Requires \usepackage{graphicx}
  \includegraphics[width=11cm]{waveforms}
  \caption{Various transient voltages; a) Q4 base voltage, b) 1st stage output voltage, c) Q17 base voltage,\newline d) amplifier output voltage.}\label{waveforms}
\end{centering}
\end{figure}

\begin{figure}[H]
  % Requires \usepackage{graphicx}
  \includegraphics[width=15cm]{slew.jpg}
  \caption{Response to a square wave input, the slew rate is 230V/$\mu$S for both the rising and falling edges between 10\% and 90\%
  of the final output value.}
  \label{slew}
\end{figure}

\begin{figure}[H]
  % Requires \usepackage{graphicx}
  \includegraphics[width=16cm]{FFT_b.jpg}
  \caption{Fourier transform of a 50W output signal (8 $\Omega$ load) in response to a 10kHz sinewave input. The 3rd harmonic magnitude is 0.00032\% of the fundamental.}\label{fft}
\end{figure}

\section{Component Selection}
The components that have a significant impact upon the amplifier performance thought choice of their type are;
\begin{itemize}
	\item Output transistors
	\item Output driver transistors
	\item 1st and 2nd differential stage transistors
\end{itemize}
\subsection*{Output transistors}
NJW0281 (npn) and MJW0302 (pnp) are ideal output stage transistors due to their excellent gain linearity (gain matching within 10\% from
50 mA to 3 A), exceptionally safe operating area and high transition frequency. The high transition frequency and a relatively small
output capacitance of 400 pF ($\textrm{V}_{\textrm{CB}}$=10 V) minimises the phase delay at higher frequencies. However, their small early voltage is a significant source of non-linearity.
\subsection*{Output driver transistors}
The KSA1220A (pnp) and KSC2690A (npn) are ideal choice as the driver transistors due to their small output capacitance of 19 pF ($\textrm{V}_{\textrm{CB}}$=10 V),
large gain, and a gain-bandwidth-product of 155 MHz ($V_{\textrm{CE}} = 5 \textrm{V}, \textrm{I}_{\textrm{C}}$ = 0.2 A).
\subsection*{Differential input pair transistors}
MMBT6429 transistors have been chosen for the NPN differential stages due to the following reasons:
\begin{itemize}
 \item Very large gain
 \item Gain-bandwidth product of about 350 MHz at 5 mA and V$_{\textrm{CE}}$ = 5 V.
 \item High Early voltage
 \item Low-noise
\end{itemize}
\subsection*{Feedback resistors}
Common resistor technologies (either thin or thick film) are suitable for the majority of the amplifier resistors. Amplifier noise is
dominated by the noise of the input transistors and associated input resistors, therefore there is some motivation for the use of resistors
with low excesses-noise.
\section{PCB layout}
Consideration to good PCB layout is essential since an inadequate layout can be a major contribution to amplifier hum, electrical instability and distortion. An understanding of the following phenomena is needed in order to design a good layout:
\begin{itemize}
	\item Inductive coupling - time dependent current loops can induce voltages around other trace loops.
	\item Capacitive coupling - traces undergoing voltage transitions can induce voltage fluctuations on other traces.
	\item Ohmic trace resistance - current will induce a voltage drop across a trace.
	\item Parasitic inductance and capacitance - can result in an unintentional impact upon the magnitude and phase of analog signals.
\end{itemize}
\begin{figure}[H]
	% Requires \usepackage{graphicx}
	\includegraphics[width=10cm]{trace_distortion}
	\caption{A diagram demonstrating how trace resistance can lead to distortion.}\label{trace}
\end{figure}
At worst, inductive coupling can result in unintentional feedback paths pushing the amplifier into an unstable electrical state and
otherwise can be a significant source of distortion and noise. Wires and traces carrying the large currents that drive the output load
should be inductively isolated from the small signal differential stages. Isolation can be achieved through distancing the differential
stages from large current loops and by the use of a ground plane to shield the input stages. In addition, minimising trace length and
the area of current loops will minimise inductive coupling. Similar techniques minimise the impact of capacitive coupling between the
input and output stages of the amplifier. Fig. \ref{trace}. is a diagram of an output stage and depicts a potential impact that trace resistance can have on amplifier distortion. The feedback trace is not directly connected to the output and the asymmetry leads to a predominantly 2nd order harmonic content. A little bit of trace resistance may seem trivial, however, a simple calculation suggests that the impact can be greater than expected. Consider the case where the feedback trace is isolated from the output by a trace of length 3 cm with a
width of 5 mm resulting in a trace resistance of about 3 m$\Omega$ (1 oz/ft$^{2}$). For a load impedance of 8 $\Omega$ driven by a peak
voltage of 24 V (load current 3 A) this results in a feedback error voltage of 9 mV between the positive and negative voltage cycles.
9 mV in 24 V is 0.0375\% and, therefore, possibly the greatest source of distortion in an amplifier!
The potential impact that parasitic capacitance can have on the feedback phase can be calculated by considering the case of the feedback
trace (length 6 cm, width 0.3 mm) running over a ground plane. For a PCB thickness of 1.2 mm this results in a capacitance of 2.8 pF.
If the feedback resistor (27 K$\Omega$) is placed directly at the output this results in an additional phase delay of about 4.6 degrees at 10 MHz. The top and bottom copper layouts are shown in figures \ref{top} and \ref{bottom}. The high current output stage is isolated from the
input sections and no large ground current loops influence the input sections.

 \begin{figure}[H]
 	\includegraphics[height=20cm]{PCB1J}
 	\caption{PCB top copper layer with top-layer silk screen}
 	\label{top}
 \end{figure}

 \begin{figure}[H]
 	\includegraphics[height=20cm]{PCB2J}
 	\caption{PCB top copper layer with top-layer silk screen}
 	\label{bottom}
 \end{figure}

\section{Amplifier characterisation} \label{sec:characterisation}
\subsection*{Electrical stability}
The electrical stability of the amplifier as a function of $\textrm{I}_{1}$, $\textrm{I}_{2}$ (hence $\textrm{I}_{\textrm{D}}$) and $\textrm{I}_{\textrm{O}}$ was experimented with and the stability was found to be excellent. As explained in chapter XXX, the open-loop gain across all frequencies, and therefore, stability depend on $\textrm{I}_{1}$. Therefore, stability can be examined by performing measurements as a function of $\textrm{I}_{1}$, in particular the response to a square wave input.

\subsection*{Thermal stability - quiescent current drift}

%\begin{center}
\begin{tabular}{|l|l|}\hline
Temperature & Current (mA)\\\hline
29 & 20\\\hline
46 & 40\\\hline
63 & 60\\\hline
\end{tabular}
%\end{center}
\subsection*{Noise and hum}
The output noise of the amplifier was measured over the 20-1 kHz band using a low-noise amplifier with a gain of 10 000 connected to the
output. Wide-band noise was measured with the power amplifier connected to a regulated power supply.
The low-noise amplifier was connected to a spectrum analyzer (analog discovery 2, Digilent). The noise spectrum obtained with the amplifier
input connector, P2, shorted is shown in Fig \ref{noise}. The wide-band noise of the amplifier due to the thermal noise of the input resistors is calculated to be,
\begin{equation}
v_{\textrm{noise}}(\textrm{RMS}) = \sqrt{4k_{B}T\left[725
\left(\frac{27k}{725}\right)^{2}+\frac{1670}{\sqrt{1+(\omega 1670\times 10^{-6})^{2}}}\left(\frac{27k+725}{725}\right)^{2}+27k\right]}
\end{equation}
where 725 is R9+R10 in parallel with R13, 1670 is the resistance setting of trimmer R16, and $10^{-6}$ is the capacitance of C12.
The frequency dependent term becomes significant for frequencies less than about 100 Hz. For frequencies greater than about 200 Hz the
calculated noise density due to resistor thermal noise is 131\,\, $\textrm{nV}/\sqrt{\textrm{Hz}}$

The flat band noise of the amplifier is measured to be -51 dB/10000 =  282 nV/$\sqrt{\textrm{Hz}}$. Upon multiplication by the low-noise
amplifier gain, the noise spectral density due to resistor thermal noise is calculated to be $-57.7$ dB. The 6.7 dB difference is consistent with the noise figure of the
MMBT6429 transistor. The noise voltage over the 20 Hz - 20 kHz band is about $\sqrt{20000-20}\times 282\,nV$ = 40 $\mu V$ RMS.
Note, the peaks at 50 Hz + harmonics originate from EM interference of the low-noise amplifier input cabling.
\begin{figure}
	% Requires \usepackage{graphicx}
	\includegraphics[width=14cm]{noise_spectrum}
	\caption{Measured noise spectrum of the amplifier output.}\label{noise}
\end{figure}

\subsection*{Open-loop gain}
Open-loop gain measurements were successfully performed by connecting a low-noise amplifier with a gain of 7500 to the U.FL test connector
of the amplifier board. Waveforms captured with an oscilloscope for a 10 kHz input signal are shown in Fig \ref{gain}. The measured gain of
106 dB compares quite well with the calculated value of 102 dB. At 1 kHz the measured gain is 113 dB and the calculated gain is 116 dB.
\begin{figure}[H]
	% Requires \usepackage{graphicx}
	\includegraphics[width=17cm]{gain}
	\caption{Measured waveforms for open-loop gain determination. At 10 kHz the gain is 106 dB.}\label{gain}
\end{figure}
\subsection*{Slew rate}
\begin{figure}[H]
	% Requires \usepackage{graphicx}
	\includegraphics[width=17cm]{slew_meas}
	\caption{Measured slew rate.}\label{slew_meas}
\end{figure}
Slew rate measurements were performed by injecting a square wave into the input with the filter capacitor (C9) removed.
The output waveform is shown in Fig \ref{slew_meas} indicating a slew rate of about 130 V/$\mu S$. This value is significantly less than
the calculated slew rate, the difference is attributed to the rise time of the square wave oscillator.
\subsection*{Distortion}
\section{Control board}
At a minimum an amplifier of more than minimal cost should be immune to catastrophic damage under the event of an output short-circuit
or excessive thermal stress. These requirements can be effectively implemented by a cheap microcontroller circuit.
Considering the overall cost of the amplifier which is dominated by the transformer, heatsinks, output transistors and case etc,
from a design  and component cost perspective there is motivation to add functionality to an amplifier which is of relatively low cost.
A control board which protects the amplifier from short-circuit and thermal faults and includes radio, alarm clock, and volume control
functionality has been designed and constructed and is briefly described below. Incorporating radio and time keeping functionality allows
for alarm clock functionality through software development.
\subsection*{Volume control}
A PGA2311 I.C. is chosen as the volume control device due to its low-noise and distortion, simple SPI digital interface
and ability to drive 600$\Omega$ loads directly. The PGA2311 has a wide gain and attenuation range; +31.5 dB to -95.5 dB with 0.5 dB steps,
however, the range that is useful to the amplifier is likely to be +31.5 dB to about -20 dB. It should be noted that the digital supply
of the PGA2311 is +5V, however, it is driven directly by the 3.3V I/O pins of the MCU since the data sheet indicates that the minimum
high-level input voltage of the PGA2311 is 2V.
\subsection*{FM radio receiver}
A Si4703 I.C. is chosen as the radio receiver due to one feature in particular - it is essentially a complete radio on a chip.
It integrates all tuner functions from antenna input to stereo audio output. Basically, when designing the amplifier and control
board I was not interested in designing something that required RF techniques. However, the Si4703 is a very attractive option due
to its high performance characteristics. Its most significant features are:
\begin{itemize}
	\item Simple 2 or 3-wire digital interface
	\item Worldwide FM band support (76–108 MHz)
	\item Seek tuning
	\item Automatic frequency control (AFC)
	\item Automatic gain control (AGC)
	\item Excellent overload immunity
	\item Signal strength measurement
	\item Programmable de-emphasis (50/75 $\mu$s)
	\item Adaptive noise suppression
	\item Line-level analog output
\end{itemize}
A separate board was constructed for the Si4703 which plugs into the control board.
\subsection*{Real-time clock}
A PCF2127 I.C. is chosen as the time keeping device. Its most notable feature is an integrated temperature compensated crystal
oscillator. It incorporates a host of features that are not utilised by the control board, however, it is included in the control
board since I intend to use it for other projects as such the PCF2127 software I write for the control board will be reused later.
\subsection*{Temperature sensors}
MAX31820 sensors are chosen as the amplifier temperature monitoring devices. An
attractive feature of these devices is their 1-wire interface which reduces MCU
pin count requirements and PCB routing requirements.
\subsection*{Display}
The NHD-C12864LZ-FSW-FBW-3V3 LCD is chosen as the display device since it has sufficient resolution (128 X 64) to display the volume,
temperature, radio and clock parameters, has an LED back-light and has an appropriate active area. Also, this LCD can operate from a
SPI port which reduces the MCU pin count requirements.
\subsection*{MCU}
The PIC18F47J13 is chosen for the microcontroller device for reasons that include:
\begin{itemize}
	\item It is a simple 8-bit MCU which simplifies the writing of code
	\item A MCU with a small computational throughput is ideal for the control device
	\item Peripheral pin select - simplifies PCB routing
	\item SPI port with direct memory access - simplifies writing routines for the LCD
	\item A change in an I/O pin state can trigger an interrupt for interface to the
	amplifier current fault detection circuit
	\item Interrupt sources can be assigned to one of two priorities, therefore, a current fault can interrupt any
	low-priority interrupts that are in service
	\item An internal ADC allows measurement of the amplifier output voltage
\end{itemize}
\subsection*{Audio source selection}
Circuitry is needed to switch between the Si4703 and external audio sources. This is achieved by the use of opamps with a
shutdown feature that puts the output into a hi-impedance state, see figure \ref{SD}.
\subsection*{User commands}
The control board accommodates 3 push button switches for user commands. Two of these switches generate a low-priority interrupt,
while the 3rd switch is polled while the MCU is actively responding to user commands when the low-priority interrupt occurs.
The control board schematic is shown in figures \ref{MCU} to \ref{radio}.


\begin{figure}[ht]
\begin{centering}
	% Requires \usepackage{graphicx}
	\includegraphics[width=24cm, angle=-90]{Amp_cont_V3}
	\caption{Control board schematic - MCU}\label{MCU}
\end{centering}
\end{figure}

\begin{figure}
\vspace*{-2cm}
\hspace*{-1cm}
	% Requires \usepackage{graphicx}
	\includegraphics[width=19cm]{Amp_cont_V3_power}
\vspace{-3cm}
	\caption{Power supply and amplifier switching circuitry of the control board.}\label{power}
\end{figure}
\begin{figure}
	% Requires \usepackage{graphicx}
	\includegraphics[width=15cm]{Amp_cont_V3_RTC}
	\caption{Volume control, realtime clock, LCD connections and Si4703 board connections.}\label{RTC}
\end{figure}
\begin{figure}
\vspace*{-2cm}
	% Requires \usepackage{graphicx}
	\includegraphics[width=17cm]{Amp_cont_V3_SD}
\vspace*{-1.5cm}
	\caption{Audio source selection circuitry.}\label{SD}
\end{figure}
\begin{figure}
	% Requires \usepackage{graphicx}
	\includegraphics[width=16cm]{Si4703}
	\caption{Radio board schematic}\label{radio}
\end{figure}

\begin{figure}
	% Requires \usepackage{graphicx}
	\includegraphics[width=16cm]{ACB_topJ}
	\caption{Control PCB top layer}\label{ACB_top}
\end{figure}

\begin{figure}
	% Requires \usepackage{graphicx}
	\includegraphics[width=16cm]{ACB_bottomJ}
	\caption{Control PCB bottom layer}\label{ACB_bottom}
\end{figure}

\section{Future work}
There are two promising and simple modifications that may improve the amplifier design which are now described.
Experimental characterisation of the amplifier indicated that the output stage quiescent current, $\textrm{I}_{\textrm{O}}$, has an excessive dependence on temperature. It is likely that a small modification to the circuit that limits $\textrm{I}_{\textrm{O}}$ will greatly reduce the sensitivity to temperature, namely, the utilisation of a thermistor or similar. A thermistor in close thermal contact with the output transistors can be used to make a temperature dependent modification to the resistance of potentiometer (R46) such that the sensitivity of $\textrm{I}_{\textrm{O}}$ to temperature is reduced.

Two independent constant current sources bias the 2nd differential stage. These two current sources are set approximately equal in magnitude so that the driver stage current is balanced when minimal current is supplied to the 2nd stage input. A more convenient approach replaces one of the current sources with a current mirror such that only a single constant current source is required to bias the 2nd stage.


\section*{Appendix}
\subsection*{Miller capacitance}
\subsection*{Low-frequency gain}
The low-frequency gain of the various stages is calculated and the results are added to obtain the open-loop gain of the amplifier.
The transconductance of the 1st differential stage is,
\begin{equation}
\frac{I_{1}}{2nV_{T}}
\end{equation}
Therefore, the voltage gain of the 1st stage is,
\begin{equation}
\frac{I_{1}}{2nV_{T}}R_{1}
\end{equation}
where $R_{1}$ is the load resistance present on the output of the 1st stage. There are two contributions to the load,
the input of the 2nd stage and the Early voltage of the 1st stage transistor, Q7. Note, the Early voltage of current mirror transistor, Q5 has an insignificant contribution due to its emitter resistor, R7. The impedance due to the Early voltage is,
\begin{equation}
 \frac{V_{Early}}{0.5\,I_{1}}
\end{equation}
The Early voltage of Q7 (MMBT6429) is very large, its SPICE model file indicates a value of 968 V, giving a load resistance of,
\begin{equation}
\frac{968}{5\times 10^{-3}} =  1.94\times 10^{5}\,\Omega
\end{equation}
The load resistance due to the 2nd stage input is,
\begin{equation}
(R23+R24)\beta_{Q12}||(R25+R26)\beta_{Q14} = 9.83\times 10^{4}||2.36\times 10^{5} = 6.94\times 10^{4}
\end{equation}
Therefore, the total load resistance on the 1st stage output is,
\begin{equation}
6.94\times 10^{4}||1.94\times 10^{5} = 5.11\times 10^{4}\,\Omega
\end{equation}
The voltage gain of the 1st stage becomes,
\begin{equation}
 \frac{I_{1}}{2nV_{T}}\times 5.11\times 10^{4} = \frac{10^{-2}}{0.052}\times 5.11\times 10^{4} = 9.83\times 10^{3} V/V
\end{equation}
The transconductance of the 2nd stage is,
\begin{equation}
\frac{1}{68.1+121} =  5.29\times 10^{-3} A/V
\end{equation}
The transconductance of the driver stage is approximately the ratio of the load resistance on Q13 and Q15 to the resistance on the emitter of Q19 and Q21. A correction to this approximation needs to be made because the dynamic resistance of the LEDs, D6 and D9 are significant.
The dynamic resistance of D6 and D9 are,
\begin{equation}
\frac{nV_{T}}{I} = \frac{13.3\times 0.026}{1.65\times 10^-3} =  200
\end{equation}
The transconductance becomes,

\begin{equation}
\begin{split}
&\left[\frac{1}{1k33} + \left[\left[\frac{1}{1k33} + \frac{1}{1k} + \frac{1}{12\beta_{Q19}}\right]^{-1}+200\right]^{-1}\right]^{-1}\\
&\times  \left[\frac{1}{1k}+\frac{1}{1k33}+\frac{1}{12\beta_{Q19}}  \right]^{-1}/\left[\left[\frac{1}{1k}+\frac{1}{1k33}+\frac{1}{12\beta_{Q19}}  \right]^{-1}+200\right]
\end{split}
\end{equation}
The 1st line is the load resistance and the 2nd line is a voltage divider correction due to the LED dynamics resistance. Assuming Q19 and Q21 $\beta$ are equal,



\subsection*{Thermal dissipation of the output transistors - reactive load}
The real power delivered to a linear load driven by a sinusoidal voltage waveform is given by,
\begin{equation}
\frac{1}{T}\int_{0}^{T}dt\,\textrm{V}_{pk}\sin(2\pi t/T)\textrm{I}_{pk}\left[\sqrt{1-\alpha^{2}}\sin(2\pi t/T)+\alpha\cos(2\pi t/T)\right] = \frac{1}{2}\textrm{V}_{pk}\textrm{I}_{pk}\sqrt{1-\alpha^{2}}
\end{equation}
where $\textrm{V}_{pk}$ and $\textrm{I}_{pk}$ is the peak load voltage and current and $\textrm{T}$ is the period of the sinusoidal waveform.
The parameter $\alpha$ takes on values from -1 to 1, representing the fact that the load current may have an arbitrary phase with respect to the load voltage depending on how inductive or capacitive the load is.
The thermal dissipation of each output transistor (ignoring the small contribution of the quiescent current) is given by,
\begin{eqnarray}
P_{th} = \frac{1}{T}\int_{0}^{T/2} \textrm{V}_{supply}\textrm{I}_{pk}\sin(2\pi t/\textrm{T})\,dt\\
-\frac{1}{4}\textrm{V}_{pk}\textrm{I}_{pk}\sqrt{1-\alpha^{2}}
\end{eqnarray}
where $\textrm{V}_{supply}$ is the output transistor collector voltage, i.e. the supply voltage. The integral is the power delivered
to both the load and output transistor and the 2nd term is the real power delivered to the load. For a load impedance of Z$\Omega$ the
thermal power dissipated by each output transistor becomes,
\begin{equation}
P_{th} = \frac{\textrm{V}_{supply}}{\pi}\frac{\textrm{V}_{pk}}{Z} - \frac{\textrm{V}^{2}_{pk}}{4Z}\sqrt{1-\alpha^{2}}
\end{equation}
\begin{figure}[H]
	% Requires \usepackage{graphicx}
	\includegraphics[width=15cm]{Pth}
	\caption{Power dissipated by each output transistor.}\label{Pth}
\end{figure}
Figure \ref{Pth} is a plot of the thermal power dissipated by each output transistor for $\textrm{V}_{supply} = 40V$ and $Z=8\,\Omega$. The
maximum power dissipated for $\alpha=0$ is about 20 W, which increases to 55 W for $\alpha=1$.
\end{document}


\subsection{Appendix}

Register mapping (offset from FSR2):
[0] - data transfer/temp storage
[1] - data transfer/temp storage
[2] - data transfer/temp storage
[3] - data transfer/temp storage
[4] - data transfer/temp storage
[5] - data transfer/temp storage
[6] - data transfer/temp storage
[7] - data transfer/temp storage
[8] - data transfer/temp storage
[9] - data transfer/temp storage
[10] - data transfer/temp storage
[11] - data transfer/temp storage
[12] T1 processed
[13] T2 processed
[14] T3 processed
[15] T4 processed
[16] volume
[17] amplifier status: [7-I_fault_lockout, 6-temperature_lockout, 5-amp_on, 4-audio_source, 3-alarm_on, 2-amp_pwr_change_rqst, 1-re-enter, 0-alarm active]
[18] T1 raw L
[19] T1 raw H
[20] T2 raw L
[21] T2 raw H
[22] T3 raw L
[23] T3 raw H
[24] T4 raw L
[25] T4 raw H
[26]
[27] hours
[28] minutes
[29] seconds
[30] alarm_hours
[31] alarm_minute
[32]
[33] time_interval for set time [7 - hrs, 6 - minutes, 5 - seconds]
[34] time_interval for set alarm [7 - hrs, 6 - minutes]
[35] Si4073_volume
[36]
[37]
[38] Audio_L - truncated 6 bits
[39] Audio_R - truncated 6 bits
[40]
[41] volume_setting_loop and other loop counter
[42]
[43]
[44]
[45]
[46]
[47]
[48] On and off loop delay reg
[49] On and off loop delay reg
[50] On and off loop delay reg
[51]
[52]
[53]
[54]
[55]
[56]
[57]
[58]
[59]
[60]
[61]
[62]
[63]
[64]
[65]
[66]
[67]
[68]
[69]
[70]
[71]
[72]
[73]
[74]
[75]
[76]
[77]
[78]
[79]




The 1.2pF capacitor (C10) that is in parallel with the feedback resistor significantly improves the stability margin of the amplifier. Capacitance with respect to the base of input transistor (Q9) in addition to stray capacitance associated with PCB tracks can contribute to the loop phase delay. This particular contribution to the phase delay can be eliminated by including a small capacitance in parallel with the feedback resistor (R8). Referring to Fig. \ref{miller_equ}. the feedback phase delay and magnitude are given by,
\begin{equation}
	\phi_{feedback}=\arctan\frac{\omega\left[R_{1}C_{1}-R_{2}C_{2}\right]}{1+R_{2}/R_{1}+\omega^{2}R_{1}R_{2}C_{1}(C_{1}+C_{2})}
\end{equation}
\begin{equation}
	|V_{feedback}|=\frac{\sqrt{R^{2}_{2}+\omega^{2}C^{2}_{2}}}{\sqrt{(R_{1}+R_{2})^{2}+\omega^{2}(C_{1}+C_{2})^{2}}}
\end{equation}
\begin{figure}
	% Requires \usepackage{graphicx}
	\includegraphics[width=7cm]{miller_sch}
	\caption{miller}\label{miller_equ}
\end{figure}
The feedback resistors are $R_{1}=27\textrm{K}\Omega$, and at high frequency $R_{2}=470\Omega$ due to the action of C9. The MMBT6429 data sheet indicates a C$_{CB}$ of about 2 pF at collector base voltage of 5V. To eliminate the phase delay due to input capacitance it is sufficient to set $C_{1}=C_{2}R_{2}/R_{1}$ = 0.035 pF. This is about a factor of 30 from the optimal value of about 1 pF as suggested by simulation. However, the miller effects needs to be taken into consideration.

\begin{itemize}
 \item Talk about the reduction of low frequency gain for other input transistors
\end{itemize}
