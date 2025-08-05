## Isolation Forest

The core idea behind the Isolation Forest algorithm is based on the assumption that anomalies are “few and different.” As a result, they are easier to isolate compared to normal data points.

In practice, the algorithm recursively partitions the dataset by randomly selecting a feature and a split value. The number of splits required to isolate a data point becomes its **anomaly score**: fewer splits indicate a higher likelihood of being an anomaly.

### Contamination Parameter

One key parameter in Isolation Forest is `contamination`, which defines the expected proportion of anomalies in the dataset. Setting this value tells the model, for example:

> “I expect about X% of the data to be anomalous.”

This guides the algorithm in establishing a threshold for anomaly detection.

* **Too high** a value may classify normal data as anomalous.
* **Too low** a value may miss real anomalies.
* If the expected anomaly rate is unknown, it's common to experiment with different values or use `contamination="auto"`.

This parameter directly influences how the model distinguishes between normal and anomalous behavior, and tuning it correctly is crucial for reliable results.

### Features Used

The model was trained on the following feature set:

* Temperature
* Humidity (for multivariate anomalies)
* First derivatives of temperature and humidity (rate of change)
* Time of day
* Day of the week
* Moving average (e.g., `rolling(window=3).mean()` to capture recent trends)

These features help the model detect deviations from normal daily patterns and identify unexpected behavior.

### Evaluation Approach

Two main testing strategies were applied:

1. **Fit and predict on the same dataset**
   The model was trained and evaluated on the same data to assess its ability to detect known anomalies within a familiar context. (unsupervised)

2. **Train on normal data, test on anomalous data**
   The model was trained on a clean dataset (without anomalies) to establish a normal behavior baseline, then tested on a dataset containing potential anomalies. (semisupervised)

This dual approach helped evaluate both **detection accuracy** and the **generalization capability** of the model.
