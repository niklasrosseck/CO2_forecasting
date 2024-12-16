# CO2 Forecasting

This project aims to predict CO2 levels using machine learning techniques. It leverages time series forecasting models to predict CO2 values provided by the Recoglass API. The tested models in this project are LSTM (Long Short-Term Memory), GRU (Gated Recurrent Unit), Bidirectional LSTM, a LSTM model with a CNN layer and a deep LSTM model, which has 3 LSTM layers. The results of this testing are found in the Report directory and the best performing models are in the models directory.

## Files

- **forecasting.ipynb**: Jupyter notebook containing the main forecasting logic and model training.
- **scheduled_forecasting.ipynb**: Notebook for setting up scheduled forecasting tasks.
- **multivariate_co2_data.csv**: Dataset for training models with multivariate features.
- **real_time_co2_data.csv**: Real-time CO2 data for prediction and testing.
- **models/**: Folder containing saved model weights.
- **Loss/**: Contains loss data in json format for every model.
- **Predictions/**: Contains prediction data in csv format for every model.
- **Report/Findings/**: Contains the findings and oberservations for every model.
- **Report/Images/**: Contains the images for loss and prediction for every model and every try.

## Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/niklasrosseck/CO2_forecasting.git

   ```

2. Install the required dependencies:

   ```bash
   pip install -r requirements.txt

   ```

3. Run the Jupyter notebooks

## Usage

1. Run the scheduled_forecasting.ipynb notebook and fetch the latest data from the API. This also sets up a
   background scheduler to periodically fetch data every 10 minutes.

2. Now you can run forecasting.ipynb notebook and run all 5 models or you can take a pretrained model out
   of the models folder and use that for your predictions.
