# CO2 Forecasting Project Report

## Overview

This project involves forecasting CO2 levels using machine learning models, specifically focusing on time-series forecasting. The goal was to forecast the CO2 levels with the information provided from the [Recoglass API](https://recoglass.net/api/get.php?key=SPXEGN1TGSB2IKT9&merge=10min&ls=0&le=5000&ext=json). This API publishes new data in certain time intervals and contains data like CO2 levels, temperature, humidity, pressure etc. For this project the time interval of 10 minutes was chosen and the scheduled_forecasting.ipynb notebook fetches the data every 10 minutes using a background scheduler. By adjusting the "&merge=10min" parameter in the API url, different time intervals could be fetched.

## Dataset

The dataset used includes time-series data of CO2 emissions, with a smaller dataset for testing and real-time data. The data was carefully preprocessed for missing values and proper feature scaling before being fed into the models. The data contains the following values:

1. id
2. subject
3. key
4. datetime
5. ip
6. Temperature
7. Pressure
8. Humidity
9. VoC
10. CO2
11. Altitude

The preprocessing in the forecasting.ipynb eliminates values that are not necessary for prediction, so the values that remain after preprocessing are:

1. Temperature
2. Pressure
3. Humidity
4. VoC
5. CO2 (as Target variable)
6. Altitude

The next preprocessing step is a MinMax Scaler which scales the values between 0 and 1 and the the sequences are created. These sequences are needed for Time Series models. For the sequences a sequence length of 96 was used. The data is then split into train and test data with a split of 80:20.

## Model Selection & Experiments

Several types of models were tested:

1. LSTM (Long Short-Term Memory): A robust architecture for time-series tasks.
2. Bidirectional LSTM: A variation of LSTM that processes data in both forward and backward directions, potentially capturing better temporal dependencies.
3. GRU (Gated Recurrent Units): A simpler alternative to LSTM, effective for time-series.
4. Deep LSTM: A stacked LSTM architecture that increases model depth for more complex patterns.
5. CNN-LSTM Hybrid: A combination of Convolutional Neural Networks (CNN) and LSTM layers to capture spatial and temporal patterns in the data.
   Multiple hyperparameters, including batch size, epochs, optimizer (Adam, AdamW), learning rate schedules, and activation functions, were explored. The full table of tested parameters and results for every model can be found in the Findings folder.

## Key Takeaways:

1. Optimal Configuration Identified:
   The best results were achieved using Layer Normalization, Huber Loss and Exponential Decay Scheduler. The choice of activation function depends on the used model. For GRU, deep LSTM and bidirectional LSTM the LeakyReLU Activation function was most effective and for the LSTM and CNN-LSTM Hybrid the softplus activation function was the most effective.

2. Normalization and Loss Functions Matter
   Layer Normalization: This technique improved the stability of training by normalizing activations within the network, preventing issues such as vanishing/exploding gradients. It enhanced generalization and allowed for faster convergence, particularly for deeper architectures.
   Huber Loss: This loss function was a key factor in balancing the model’s sensitivity to outliers. It combines the advantages of MSE (Mean Squared Error) and MAE (Mean Absolute Error), offering robustness to large deviations while maintaining precision on smaller errors.

3. Scheduler Choice is Crucial
   Exponential Decay Scheduler: Among various learning rate schedulers tested, the Exponential Decay scheduler emerged as the most effective. It helped the model adapt to complex patterns in the data by gradually reducing the learning rate during training, thereby enhancing the model’s ability to fine-tune weights over time. This was particularly effective when paired with advanced activation functions like Softplus and LeakyReLU, which allowed the model to learn more complex patterns.

4. Activation Functions Can Improve Performance
   ReLU vs Advanced Activations: While ReLU is a popular choice, its limitations (such as dead neurons) were addressed by switching to more advanced activations. The use of Softplus and LeakyReLU enhanced the model’s learning capacity by preventing dead neurons and allowing gradients to flow more smoothly. These activations enabled the model to better capture non-linear relationships, leading to improved performance in forecasting.

## Future Advancements

In the future, the model’s performance can be significantly improved by:

1. More Data: Larger, more diverse datasets will enable the model to capture a wider range of patterns, leading to better generalization and accuracy.
2. Attention Mechanisms: Implementing attention mechanisms, such as Self-Attention or Transformers, can allow the model to focus on the most important features in the data, improving both forecasting accuracy and interpretability.
3. Hyperparameter Optimization: Further tuning of model parameters and exploring advanced optimization algorithms will help achieve even finer control over training.
   Ensemble Methods: Combining predictions from multiple models could improve robustness and reduce prediction errors
