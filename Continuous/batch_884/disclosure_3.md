# 10312042

## Adaptive Predictive Relay Control for Harmonic Mitigation

**System Specifications:**

*   **Core Component:** A microcontroller-based system integrated within the Automatic Transfer Switch (ATS) housing, or as a standalone module for retrofit.
*   **Input Sensors:**
    *   Current Transformers (CTs) on both primary and secondary power inputs.  Resolution: 1A increments. Sampling Rate: 10kHz minimum.
    *   Voltage Transformers (VTs) on both primary and secondary power inputs. Resolution: 1V increments. Sampling Rate: 10kHz minimum.
    *   Real-Time Clock (RTC) – synchronized via NTP or similar protocol for timestamping.
*   **Output Control:** Direct control of ATS relays via solid-state relays (SSRs) for precise timing. SSRs must support PWM control.
*   **Memory:** 256MB Flash Memory for storing historical waveform data and predictive model parameters. 64MB RAM for real-time processing.
*   **Communication:** Ethernet port for data logging, remote monitoring, and model updates.  Modbus TCP/IP support.
*   **Power Supply:** 24VDC isolated power supply.

**Functional Description:**

This system aims to proactively shape relay closure timing to *mitigate* harmonic distortion introduced during the transfer process.  Instead of simply closing relays at a zero-crossing, the system *predicts* the optimal closure moment to minimize the transient harmonic injection into the load.

1.  **Waveform Capture & Analysis:** The CTs and VTs continuously capture voltage and current waveforms on both power inputs. A Fast Fourier Transform (FFT) algorithm analyzes these waveforms in real-time to identify dominant harmonic frequencies and their amplitudes.
2.  **Harmonic Prediction Model:** A Recurrent Neural Network (RNN), specifically a Long Short-Term Memory (LSTM) network, is trained offline on historical waveform data representative of expected load profiles. The LSTM predicts future voltage and current waveforms, including harmonic content, based on current measurements and time-series patterns.
3.  **Optimal Relay Timing Calculation:** The system calculates the optimal relay closure time by analyzing the *predicted* waveforms. The goal is to minimize the harmonic distortion at the load terminals immediately following the transfer. This is achieved by:
    *   Identifying the points in the predicted waveform where the rate of change of harmonic distortion is minimal.
    *   Calculating the required delay to align the relay closure with this optimal moment.
4.  **PWM Relay Control:** Instead of a hard on/off switch, the relays are controlled via Pulse-Width Modulation (PWM). This allows for a “soft” transfer, gradually bringing the secondary source online while simultaneously reducing the primary source. PWM duty cycle and frequency are dynamically adjusted based on the predicted harmonic distortion.
5.  **Adaptive Learning:** The system continuously monitors the harmonic distortion *after* each transfer. This data is used to refine the predictive model and improve the accuracy of the optimal relay timing calculations.  A Reinforcement Learning (RL) algorithm could be employed to optimize the PWM control strategy based on real-time feedback.
6.  **Fault Detection & Mitigation:** If the system detects a significant increase in harmonic distortion or instability, it reverts to a traditional zero-crossing transfer strategy to ensure system stability.

**Pseudocode (Simplified):**

```
// Main Loop
while (true) {
    // Capture Voltage and Current Waveforms
    waveformData = captureWaveformData();

    // Analyze Waveforms - FFT
    harmonicData = analyzeHarmonics(waveformData);

    // Predict Future Waveform
    predictedWaveform = predictWaveform(harmonicData);

    // Calculate Optimal Relay Timing
    optimalTiming = calculateOptimalTiming(predictedWaveform);

    // Calculate PWM Duty Cycle
    pwmDutyCycle = calculatePwmDutyCycle(optimalTiming);

    // Control Relays via PWM
    controlRelays(pwmDutyCycle);

    // Monitor Harmonic Distortion After Transfer
    postTransferHarmonics = monitorPostTransferHarmonics();

    // Update Predictive Model (Adaptive Learning)
    updatePredictiveModel(postTransferHarmonics);
}
```

**Potential Extensions:**

*   Integration with a cloud-based platform for remote monitoring, data analysis, and model updates.
*   Development of a machine learning model to predict load profiles and proactively adjust the system’s parameters.
*   Implementation of a self-diagnostics module to detect and report any potential issues with the system.