# Anomaly Detection Methods on Time Series

This project explores and compares various **Anomaly Detection models** applied to simulated temperature and humidity time series data. The aim is to identify different types of anomalies using both senmi-supervised and unsupervised techniques.


## Simulated Data

The dataset consists of environmental measurements recorded every 15 minutes over 7 days:

- **Temperature**: follows a daily sinusoidal pattern  
- **Humidity**: inversely correlated with temperature  
- **Simulated anomalies include**:  
  - Sudden spikes  
  - Level shifts  
  - Anomalous noise  
  - Altered patterns (e.g., doubled sinusoidal cycles)  
  - Multivariate anomalies (disrupted temperature-humidity correlations)  


## Models Used

- **Isolation Forest**  
  - Random isolation-based anomaly detection  
  - Effective for global outliers (e.g., spikes)  
  - Key parameter: `contamination`

- **PeriodicAverageAnomalyDetector**  
  - Detects deviations from average periodic behavior  
  - Suitable for structural or cyclic anomalies  
  - Produces anomaly scores between 0 and 1  


## Experimental Setup

1. **Train and test on the same dataset**  
   - To evaluate if models detect known anomalies.

2. **Train on normal data, test on altered data**  
   - More realistic scenario for baseline and anomaly detection.


## Features Engineered

- Temperature, Humidity  
- First-order derivatives (step-to-step changes)  
- Time of day, day of week  
- Moving average with window size 3  

## Results Summary

- **Isolation Forest**: excels at detecting spikes and strong outliers; less effective for gradual pattern changes.  
- **PeriodicAverageAnomalyDetector**: effective for periodic and structural anomalies.  


## 📁 Project Structure

```sh
anomaly_detection/
├── README.md                   # Project description and instructions
├── simulated_data/            # Simulated time series data
│   ├── file_csv/              # Generated CSV datasets
│   └── generate_data.ipynb       # Script to generate the simulated data
├── isolationforest_model/     # Isolation Forest implementation
│   ├── isolationforest.md     # Documentation for the model
│   ├── multivariata/          # Multivariate anomaly detection code
│   │   └── isolation_multivariata_semisuper.ipynb
│   │   └── isolation_multivariata_unsuper.ipynb
│   └── univariata/            # Univariate anomaly detection code
│   │   └── isolation_forest_semisuper.ipynb
│   │   └── isolation_forest_unsuper.ipynb
├── periodicaverage_model/     # Periodic Average Anomaly Detector
│   ├── Periodic_Average.md         
│   ├── periodic_average_semisuper.ipynb          
│   └── periodic_average_unsuper.ipynb       

```


## 🧠 Conclusions

Choosing the best anomaly detection model depends on:  
- Data type (univariate vs. multivariate)  
- Anomaly type (spikes, level shifts, multivariate anomalies, pattern changes)  
- Availability of labeled data  

Different models suit different scenarios. Empirical comparison is key to selecting the right approach.




