## Isolation Forest

The core idea behind the Isolation Forest algorithm is based on the assumption that anomalies are “few and different.” As a result, they are easier to isolate compared to normal data points.

In practice, the algorithm recursively partitions the dataset by randomly selecting a feature and a split value. The number of splits required to isolate a data point becomes its **anomaly score**: fewer splits indicate a higher likelihood of being an anomaly.

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

### Contamination Parameter

One key parameter in **IsolationForest** is `contamination`, which defines the expected proportion of anomalies in the dataset. In other words, it tells the model:

> “I expect about X% of the data to be anomalous.”

This guides the algorithm in establishing a threshold for anomaly detection.

* **Too high** a value may classify normal data as anomalous.
* **Too low** a value may miss real anomalies.
* If the expected anomaly rate is unknown, it's common to experiment with different values or use `contamination="auto"`.

**How Contamination Works in IsolationForest**

The `contamination` parameter is essentially an estimate of the fraction of outliers in your dataset. Mathematically:

$$
\text{contamination} = \frac{\text{expected number of outliers}}{\text{total number of samples}}
$$

Here:

* **Expected number of outliers**: what you believe are anomalous points in the dataset.
* **Total number of samples**: the size of your dataset.

**How the Algorithm Uses It**
During training, IsolationForest builds many “Isolation Trees.” Each data point gets an average **path length**, representing how quickly it can be isolated. Points with shorter paths are more likely to be outliers.

After training, each point receives a continuous anomaly score (`decision_function`):

* Negative scores → more anomalous
* Positive scores → more normal

The model then chooses a threshold based on the `contamination` value. Points below this threshold are labeled as outliers (-1), and others as inliers (1).

**Practical Implications (scikit-learn)**

Suppose you have $n$ samples and set `contamination = c`. The predicted number of outliers is:

$$
\text{predicted outliers} = \max(1, \lfloor c \cdot n \rfloor)
$$

This means:

* At least **one point** is always labeled as an anomaly, even if `c * n < 1`.
* For small datasets, even tiny contamination values can produce one or more anomalies.

**Example:**

* Dataset with 1,000 samples, `contamination = 0.01`:
  → 10 points (1% of 1,000) are labeled as anomalies.

* Same dataset, `contamination = 1e-4`:
  → `0.1` point is theoretically anomalous, but scikit-learn forces at least **1 anomaly**.

This explains why, for some datasets, it’s impossible to get exactly zero anomalies unless the contamination is extremely small—so small that all points fall above the model’s internal threshold.

