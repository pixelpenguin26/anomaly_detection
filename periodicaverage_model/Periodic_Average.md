## Periodic Average Anomaly Detector

Model used for semisupervised and unsupervised anomaly detection in time series is the `PeriodicAverageAnomalyDetector` from the `classtimeseria` library.

This model assumes that the time series follows a periodic pattern. It estimates the **expected average value** at each point in the cycle and detects anomalies based on **deviations from these expected values**.

### How it works

1. **Training (`fit`)**:
   The model learns the typical periodic behavior from the input time series.

2. **Anomaly detection (`apply`)**:
   It compares actual values with expected periodic averages and computes a **continuous anomaly score** (ranging from 0 to 1).
   Higher scores indicate a greater likelihood of the point being anomalous.

This approach is particularly effective for time series with strong seasonal or cyclic patterns, as it highlights points that significantly deviate from expected trends.
