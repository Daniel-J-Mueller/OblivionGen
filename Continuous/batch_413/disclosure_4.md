# D678048

## Dynamic Carton Morphology - 'Bloom'

**Concept:** A carton capable of controlled, outward expansion *after* being sealed, triggered by a delayed-release mechanism incorporated into the carton’s geometry. This "bloom" effect allows for showcasing contents *after* purchase/delivery, creating a unique unboxing experience, or adapting the carton's volume to accommodate added items.

**Materials:**

*   Carton Material: Recycled corrugated cardboard, treated with a moisture-resistant coating.
*   Expansion Elements: Shape-memory alloy (SMA) struts pre-stressed for inward compression.  These struts will be integrated into pre-defined 'petal' or 'leaf' sections of the carton.
*   Trigger Mechanism: Micro-encapsulated phase-change material (PCM) with a melting point of 30-35°C.  PCM will be coated onto a resistive heating element powered by a small, embedded coin cell battery.
*   Adhesive: Water-activated, biodegradable adhesive for initial carton assembly.

**Construction:**

1.  **Carton Geometry:**  The carton is designed with multiple hinged 'petals' or 'leaves' – sections of the side walls. These sections are pre-scored to facilitate outward movement.  The number and size of petals will vary depending on the desired 'bloom' effect.
2.  **SMA Integration:** Shape-memory alloy struts are embedded *within* the petal structures.  These struts are pre-stressed to maintain the petal in a compressed/closed position during shipping. The SMA material must be selected for rapid response time to temperature change.
3.  **PCM/Heater Assembly:** A small, flat resistive heating element is bonded to the interior surface of one or more petals. This heating element is then coated with the micro-encapsulated PCM. The coin cell battery is integrated into the carton wall, wired to the heating element with a thin, flexible conductor.  A small, recessed 'activate' button on the exterior of the carton completes the circuit.
4.  **Assembly & Sealing:** The carton is assembled using the water-activated adhesive. It is then sealed as usual. The user must deliberately activate the 'bloom' after receiving the package.

**Operation:**

1.  User presses the ‘activate’ button on the carton.
2.  The coin cell battery powers the resistive heating element.
3.  The heating element melts the micro-encapsulated PCM.
4.  As the PCM changes phase, the SMA struts *release* their pre-stress, causing the hinged petals to expand outwards.
5.  The carton ‘blooms’ – expanding its volume and revealing its contents more dramatically.

**Pseudocode for Activation Sequence:**

```
FUNCTION activateBloom()
    IF buttonPressed() THEN
        battery.enablePower()
        heater.turnOn()
        WHILE heater.temperature() < PCM_MeltingPoint DO
            WAIT(1 second)
        ENDWHILE
        heater.turnOff()
        SMA_Struts.releaseStress()
        Petal_Hinges.open()
    ENDIF
ENDFUNCTION
```

**Variations:**

*   **Timed Activation:** Replace the manual activation button with a timer circuit to trigger the bloom after a pre-set delay.
*   **Environmental Trigger:** Utilize a humidity sensor or light sensor to trigger the bloom based on external conditions.
*   **Multi-Stage Bloom:** Incorporate multiple SMA struts and heating elements to create a more complex, multi-stage expansion sequence.
*   **Aromatic Release:** Embed scent capsules within the petals to release a fragrance during the bloom.