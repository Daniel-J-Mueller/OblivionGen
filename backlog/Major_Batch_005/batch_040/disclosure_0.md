# 11467806

## Adaptive Precision Systolic Array with Data-Dependent Normalization

**Concept:** A systolic array where the normalization process *within* each processing element (PE) is dynamically adjusted based on the statistical properties of the incoming data streams. This goes beyond simply handling denormals; it aims to optimize precision allocation at a granular level to maximize throughput and minimize energy consumption.

**Specifications:**

*   **PE Architecture:** Each PE consists of:
    *   Input Buffers (x2):  One for normalized input data, one for normalized weights.  Each buffer includes a statistical accumulator (running mean/variance) for the incoming data.
    *   Dynamic Normalizer: A configurable normalization module. It can select from a range of normalization strategies (e.g., full 18-bit normalization as in the patent, 16-bit scaling, 12-bit truncation) determined by the statistical data.
    *   Multiplier: A configurable multiplier with selectable precision.  Can operate at full 18x18, 16x16, or 12x12 bit precision, as controlled by the Dynamic Normalizer.
    *   Accumulator: 34-bit floating-point adder with configurable rounding modes.
    *   Control Logic: Manages data flow, precision selection, and statistical accumulation.

*   **Normalization Strategies:**
    *   **Full Normalization (18-bit):** As in the reference patent, used for initial data or when wide dynamic range is critical.
    *   **Scaled Normalization (16-bit):**  If the running variance of the input data falls below a threshold, the normalization process can scale the data to utilize the full 16-bit range, eliminating the need for the extra exponent bits.
    *   **Truncated Normalization (12-bit):** For highly stable data streams with low variance, the least significant bits of the significand can be truncated, significantly reducing computational complexity.

*   **Statistical Accumulation:**
    *   Each PE maintains a running mean and variance for both input data and weights.
    *   These statistics are updated with each data pass.
    *   Thresholds are established to trigger changes in normalization strategy (e.g., if variance < threshold, switch to scaled normalization).
    *   Statistics can be communicated between adjacent PEs in a row or column to smooth out variations and improve overall array stability.

*   **Data Flow & Control:**
    1.  Input data and weights enter the array.
    2.  Each PE's statistical accumulator updates its mean/variance.
    3.  Based on thresholds and statistical data, the PE selects a normalization strategy.
    4.  Data is normalized according to the selected strategy.
    5.  Multiplication and accumulation occur.
    6.  Partial sums are passed to the next PE in the array.

**Pseudocode (PE Control Logic):**

```
// Initialization
mean = 0
variance = 0
normalization_strategy = FULL_NORMALIZATION

// Data Processing Loop
for each input_data, weight:
    // Update Statistics
    mean = (mean * (N-1) + input_data) / N  // N = number of data points seen
    variance = ((variance * (N-1)) + (input_data - mean)^2) / N

    // Determine Normalization Strategy
    if variance < VARIANCE_THRESHOLD_1:
        normalization_strategy = SCALED_NORMALIZATION
    elif variance < VARIANCE_THRESHOLD_2:
        normalization_strategy = TRUNCATED_NORMALIZATION
    else:
        normalization_strategy = FULL_NORMALIZATION

    // Perform Normalization
    normalized_data = normalize(input_data, normalization_strategy)
    normalized_weight = normalize(weight, normalization_strategy)

    // Perform Multiply-Accumulate
    product = multiply(normalized_data, normalized_weight)
    partial_sum = add(product, previous_partial_sum)

    // Output partial_sum
```

**Potential Benefits:**

*   **Reduced Energy Consumption:**  By reducing precision when possible, computational complexity is lowered, leading to significant energy savings.
*   **Increased Throughput:**  Simplified computations allow for faster processing.
*   **Adaptive to Data:**  The array dynamically adapts to the characteristics of the input data, maximizing efficiency.
*   **Fine-Grained Precision Control:** Allows for precise control over the precision of each operation.