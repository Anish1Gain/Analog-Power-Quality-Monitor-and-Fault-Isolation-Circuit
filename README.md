# Analog-Power-Quality-Monitor-and-Fault-Isolation-Circuit
# Solid-State Over/Under Voltage and Frequency Protection System

## Overview
This project is an analog, solid-state protection relay designed to monitor an AC power supply. It ensures that sensitive downstream equipment is only powered when the supply voltage and frequency fall within a safe, pre-defined "window." If the power supply experiences a swell (overvoltage), sag (undervoltage), or a shift in frequency, the circuit immediately detects the anomaly and triggers a logic-high "kill switch" signal to isolate the load. 

The system is divided into two independent monitoring modules: **Voltage Monitoring** and **Frequency Monitoring**.

## 1. Voltage Monitoring Module (Over/Under Voltage)
This section constantly measures the amplitude of the incoming AC wave and compares it against safe thresholds.

* **Input & Signal Conditioning:** The raw AC input is first stepped down to a manageable low-voltage level.
* **Full-Wave Precision Rectifier:** Standard diode rectifiers suffer from a forward voltage drop (usually 0.7V), which introduces significant measurement error at low voltages. To solve this, the circuit uses OP413 operational amplifiers and 1N4148 diodes configured as a precision rectifier. This perfectly converts the AC sine wave into a pulsating DC signal without the standard diode voltage loss.
* **Envelope Detection/Filtering:** An RC low-pass filter smooths the pulsating DC into a stable, continuous DC voltage level that is directly proportional to the RMS/Peak voltage of the AC input.
* **Window Comparator:** This stable DC voltage is fed into two op-amps acting as comparators. 
    * One comparator checks if the voltage exceeds the upper threshold (Overvoltage limit).
    * The second checks if it drops below the lower threshold (Undervoltage limit).
* **Diode OR-Logic:** The outputs of these comparators are tied together using diodes. If *either* an overvoltage OR an undervoltage condition occurs, the combined output goes high, triggering the voltage protection kill switch.

## 2. Frequency Monitoring Module (Over/Under Frequency)
Monitoring frequency in the analog domain is trickier than monitoring voltage. This circuit handles it beautifully using an analog frequency-to-voltage conversion technique.

* **Analog Differentiator (Frequency-to-Voltage):** The AC signal passes through a buffer and into an RC network configured as a differentiator or high-pass filter. In an AC circuit, the rate of change (derivative) of a sine wave is directly proportional to its frequency. Mathematically, for an input $V_{in} = A \sin(2\pi ft)$, the derivative incorporates the frequency $f$ into the amplitude. Therefore, if the frequency goes up, the amplitude of the signal exiting this RC network also goes up.
* **Precision Rectification & Filtering:** Just like the voltage module, this newly amplitude-modulated signal (which now represents frequency) is run through its own full-wave precision rectifier and smoothed into a steady DC voltage.
* **Window Comparator:** This DC voltage—now representing the AC line frequency—is fed into a second set of comparators with very tight tolerances.
    * One comparator detects if the DC level (frequency) is too high.
    * The other detects if the DC level (frequency) is too low.
* **Diode OR-Logic:** Similar to the voltage side, if the frequency leaves the safe operating window, the logic ties high, triggering the frequency protection kill switch.

## Applications & Use Cases
This type of rapid analog protection is highly relevant for:
* **Grid-Tied Inverters:** Ensuring a solar inverter only connects to the grid when the utility power is stable.
* **Industrial Motor Protection:** AC induction motors can overheat or sustain damage if driven by incorrect frequencies or brownout (undervoltage) conditions.
* **Generator Monitoring:** Portable or backup generators are prone to RPM fluctuations, which cause frequency shifts that can destroy sensitive electronics.
