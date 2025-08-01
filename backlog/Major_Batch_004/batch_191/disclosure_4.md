# 9645064

## Dynamic Material Property Assessment via Focused Ultrasound & Micro-Impact

**Concept:** Integrate focused ultrasound to pre-stress the material sample *during* the micro-impact test, allowing for dynamic property assessment under controlled, remotely adjustable stress states. This moves beyond simply measuring strength/modulus at a given strain rate, enabling mapping of material behavior across a stress-strain space in real-time.

**Specifications:**

*   **System Components:**
    *   Existing Micro-Impact System (as described in provided patent) – Modified to accommodate ultrasound integration.
    *   Focused Ultrasound Transducer Array – High-frequency (MHz range) phased array for precise spatial and temporal control of ultrasound energy delivery.  Transducer positioned to direct ultrasound through the carrier and onto the sample's top surface.
    *   Acoustic Coupling Layer – Thin layer of acoustic gel or fluid between the transducer and carrier for efficient energy transfer.
    *   Real-Time Ultrasound Monitoring – Separate ultrasound receiver array to monitor wave propagation and material response *during* impact.
    *   High-Speed Data Acquisition System – Synchronized data acquisition from impact sensors (force, displacement), ultrasound receiver array, and potentially optical interferometry for precise strain measurement.
    *   Control Software – Algorithm to coordinate ultrasound emission, impact release, and data acquisition.  Includes feedback loops for real-time stress control.
*   **Operational Procedure:**
    1.  Sample Preparation: Prepare sample (glass, polymer, metal) and mount on anvils (as per original patent).
    2.  Ultrasound Pre-Stress: Apply focused ultrasound to create a controlled stress state within the sample. This can be tensile, compressive, or shear, depending on transducer configuration and beam steering. Amplitude and frequency of ultrasound are precisely controlled.
    3.  Synchronized Impact: Release the mass to initiate impact *while* maintaining the ultrasound-induced stress state. The impact force and resulting deformation are measured.
    4.  Real-Time Monitoring: Simultaneously measure:
        *   Impact force and carrier displacement (original patent sensors).
        *   Ultrasound wave propagation through the sample (receiver array). Changes in wave speed or attenuation indicate material deformation and damage.
        *   Optional: Surface strain using optical interferometry.
    5.  Data Analysis: Process data to determine dynamic strength and modulus *under the applied ultrasound stress*. This yields a dynamic stress-strain curve, representing material behavior under combined static (ultrasound) and dynamic (impact) loading.
*   **Pseudocode (Control Software):**

```pseudocode
// Initialization
Initialize impact sensors, ultrasound transducer, receiver array, data acquisition system.
Set initial ultrasound parameters (frequency, amplitude, focus).

// Main Loop
While (test running)
    Apply ultrasound pre-stress:
        Transmit ultrasound pulse with defined parameters.
        Monitor received ultrasound signal for confirmation of pre-stress.
    Release impact mass:
        Trigger impact mass release.
    Acquire data:
        Simultaneously record:
            Impact force and displacement
            Ultrasound signal from receiver array
            Optional: Surface strain from optical interferometry
    Process data:
        Calculate dynamic strength and modulus under ultrasound stress.
        Store data.
        Update ultrasound parameters (optional, for iterative testing).
End While
```

*   **Innovation:** This system moves beyond characterizing material properties at a *single* strain rate, to mapping a *stress-strain space*. By applying controlled ultrasound stress during impact, we can assess material behavior under a wider range of loading conditions, simulating more realistic service environments. The feedback loop allows for *dynamic* control of the stress state, tailoring the test to specific material properties or applications. This opens opportunities for advanced material characterization and predictive modeling.