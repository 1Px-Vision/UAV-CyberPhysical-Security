# Introduction, Related Work, and Technical Background

AeroResilience is a visual, block-based framework that the authors propose for teaching and evaluating UAV communication resilience. Users configure interference models, timing schedules, RF links, antenna gains, and channel effects through interconnected blocks and reusable JSON scenarios. Power, waveform, spectrum, and metrics scopes support the inspection of simulated communication conditions. A proposed hardware interface would extend this workflow to controlled laboratory evaluation using compatible software-defined radio (SDR) platforms, GNU Radio, and measurement instruments, linking simulation-based exercises with experimental observations.

Figure~\ref{fig:aeroresilience_blocks_ch1} illustrates the connected block architecture and live link-budget display. Figure~\ref{fig:aeroresilience_scopes_ch1} shows the corresponding types of waveform and spectrum views used for qualitative inspection. The following practical exercise connects the software representation to its prospective hardware implementation. It emphasizes block responsibilities, interpreting simulation outputs, and the assumptions required for experimental comparison.

\begin{practicalbox}{Practical Exercise 1.1: Mapping Simulation Blocks to Hardware and Software}

\textbf{Objective.}
Identify the hardware and software functions required to translate a simulated UAV communication-resilience scenario into a prospective laboratory architecture.

\medskip
\textbf{Scenario and supporting material.}
Load \texttt{D2D\_Narrow\_2400MHz.json} in AeroResilience to examine a narrowband-interference scenario in a simulated 2.4\,GHz communication environment. The supporting JSON scenario is provided through the book's repository:

\smallskip
{\small\url{https://github.com/1Px-Vision/UAV-CyberPhysical-Security}}

\medskip
\textbf{Practical task.}
\begin{enumerate}
    \setlength{\itemsep}{2pt}
    \item Trace the connected sequence from the spot-interference model through the timer, RF-link, antenna-gain, and channel-effect blocks to the output power scope.
    \item Identify configured inputs, including frequency, bandwidth, power settings, timing, antenna gains, and channel assumptions, where available.
    \item Inspect the waveform, spectrum, and scenario metrics. Record the available SNR, SINR, interference-to-signal ratio ($J/S$), and expected packet delivery ratio (PDR).
    \item Compare a baseline and an interference case while holding the remaining configuration fixed. Record any countermeasure settings used.
    \item Annotate each block with its prospective hardware or software counterpart.
\end{enumerate}

\par\medskip
\noindent\begin{minipage}{\linewidth}
    \centering
    \includegraphics[width=\linewidth]{chapters/Chapter_1/figures/Practical_p1_ch_1.jpg}
    \captionof{figure}{AeroResilience connected block architecture and live link-budget display for a simulated 2.4\,GHz communication-resilience exercise. The processing chain includes the spot-interference model, timer, RF-link parameters, antenna gains, channel effects, and output power scope. The floating metrics panel and live link-budget display show different frame indices and therefore represent separate simulation states.}
    \label{fig:aeroresilience_blocks_ch1}
\end{minipage}
\par\medskip

\medskip
\textbf{Hint.}
For each block, ask whether its responsibility is configuration, scheduling, signal generation, propagation modeling, reception, or measurement. Identify where a simulated quantity would require calibration before comparison with a physical measurement.

\medskip
\textbf{Worked situation and solution.}
The spot-interference model corresponds to a software-defined signal source, with a compatible SDR or RF signal generator as its prospective hardware counterpart. The timer corresponds to scheduling and synchronization functions. The RF-link block represents communication-link parameters associated with a transmitter and receiver. Antenna gains and channel effects correspond to characterized antennas, propagation conditions, or calibrated attenuation in a conducted setup. Output scopes correspond to analysis software and measurement instruments, while the metrics panel corresponds to performance calculations and logging. GNU Radio and device drivers would support signal processing and SDR control in the proposed laboratory interface.

The resulting annotated architecture therefore connects signal-generation software and RF equipment to the communication receiver and measurement path. A laboratory implementation requires a documented, calibrated, conducted or shielded arrangement. Simulator power settings must be related to defined RF reference points before they can be compared with physical measurements.

\medskip
\textbf{Interpretation of results.}
Expected PDR is a model output. Experimental PDR requires transmitted-packet and valid-received-packet counts over a defined observation interval:
\begin{equation}
    \mathrm{PDR}_{\mathrm{measured}}
    = \frac{N_{\mathrm{valid,received}}}
    {N_{\mathrm{transmitted}}}\times100\%.
\end{equation}
State the packet-validity criteria and counting interval. Treat normalized waveform amplitudes and relative spectral magnitudes as simulation representations rather than calibrated RF power measurements. Compare scope and metrics values from the same frame or observation interval. In Figure~\ref{fig:aeroresilience_blocks_ch1}, the floating metrics panel shows frame~0 while the live link-budget display shows frame~138; these values must not be interpreted as a synchronized record.

\par\medskip
\noindent\begin{minipage}{\linewidth}
    \centering
    \includegraphics[width=\linewidth]{chapters/Chapter_1/figures/Practical_p2_ch_1.jpg}
    \captionof{figure}{AeroResilience simulated waveform and spectrum scopes. The upper view compares the legitimate link signal, simulated interference component, raw received signal, and modeled response after countermeasure processing. The lower view compares their spectral representations. Waveform amplitudes are normalized and spectral magnitudes are relative; these plots do not constitute calibrated RF measurements.}
    \label{fig:aeroresilience_scopes_ch1}
\end{minipage}
\par\medskip

\medskip
\textbf{Implementation checklist.}
\begin{itemize}
    \setlength{\itemsep}{2pt}
    \setlength{\parskip}{0pt}
    \item Identify each block's responsibility.
    \item Separate configured parameters, calculated outputs, and measured quantities.
    \item Map each function to its prospective hardware or software counterpart.
    \item Document units, bandwidths, power reference points, and channel assumptions.
    \item Record baseline and interference-case results using consistent observation intervals.
    \item Document calibration requirements and implementation limitations.
    \item Preserve the JSON configuration, software revision, repository commit identifier, and random seed where applicable.
\end{itemize}

\textbf{Expected deliverable.}
An annotated hardware--software architecture, a table distinguishing configured parameters from calculated outputs, and a brief comparison of baseline and interference-case behavior.

\end{practicalbox}

