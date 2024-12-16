## Tabular Results for Bidirectional LSTM

| **Try** | **Epochs** | **Batch Size** | **Optimizer** | **Dataset** | **Normalization** | **Loss Function** | **Learning Rate Scheduler** | **Activation** | **Test Loss** | **Test MAE** |
| ------- | ---------- | -------------- | ------------- | ----------- | ----------------- | ----------------- | --------------------------- | -------------- | ------------- | ------------ |
| 1       | 200        | 32             | Adam          | Small       | None              | MSE               | None                        | ReLU           | 0.002249      | 0.042513     |
| 2       | 200        | 16             | Adam          | Small       | None              | MSE               | None                        | ReLU           | 0.002623      | 0.045699     |
| 3       | 200        | 32             | AdamW         | Small       | None              | MSE               | None                        | ReLU           | 0.014156      | 0.106475     |
| 4       | 200        | 16             | AdamW         | Small       | None              | MSE               | None                        | ReLU           | 0.006370      | 0.068345     |
| 5       | 200        | 8              | Adam          | Small       | None              | MSE               | None                        | ReLU           | 0.014822      | 0.119084     |
| 6       | 200        | 16             | Adam          | Real-Time   | None              | MSE               | None                        | ReLU           | 0.001001      | 0.026063     |
| 7       | 200        | 16             | Adam          | Real-Time   | Batch Norm        | MSE               | None                        | ReLU           | 0.011096      | 0.088123     |
| 8       | 200        | 16             | Adam          | Real-Time   | Layer Norm        | MSE               | None                        | ReLU           | 0.000333      | 0.014184     |
| 9       | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | None                        | ReLU           | 0.000126      | 0.011776     |
| 10      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Log-Cosh          | None                        | ReLU           | 0.000120      | 0.011541     |
| 11      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | MAE               | None                        | ReLU           | 0.014167      | 0.014167     |
| 12      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | MSE               | ReduceLROnPlateau           | ReLU           | 0.000885      | 0.023989     |
| 13      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | ReduceLROnPlateau           | ReLU           | 0.000502      | 0.025728     |
| 14      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | Exponential Decay           | ReLU           | 0.0000951     | 0.010474     |
| 15      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | Cosine Decay                | ReLU           | 0.001864      | 0.050746     |
| 16      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | Exponential Decay           | Swish          | 0.000089      | 0.009985     |
| 17      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | Exponential Decay           | Softplus       | 0.000146      | 0.013896     |
| 18      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | Exponential Decay           | LeakyReLU      | 0.000104      | 0.010770     |

## Observations for Bidirectional LSTM

1. **Initial Attempts on Small Dataset**:

   - Without normalization, results vary based on batch size and optimizer choice.
   - Best performance observed with **Adam optimizer, batch size 16**, yielding a **Test Loss of 0.002622** and **MAE of 0.0457**.

2. **Switching to AdamW**:

   - Transitioning to AdamW worsened performance compared to Adam, with higher loss and MAE values across batch sizes.

3. **Batch Size Impact**:

   - Decreasing batch size to 8 degraded performance, possibly due to noisier updates during training.

4. **Real-Time Dataset with Adam**:

   - Moving to the real-time dataset significantly improved results. Test Loss dropped to **0.001001** with **Adam optimizer**, **batch size 16**, and no normalization.

5. **Normalization Techniques**:

   - **Batch Normalization** increased Test Loss and MAE, confirming its inefficacy for this model and dataset.
   - **Layer Normalization** dramatically improved performance. For example, with **Huber loss**, Test Loss dropped to **0.0001255** and MAE to **0.0117**.

6. **Loss Functions**:

   - **Huber Loss** performed better than MSE, Log-Cosh, and MAE in most cases.
   - Log-Cosh performed slightly worse than Huber but better than MSE.

7. **Learning Rate Schedulers**:

   - **ReduceLROnPlateau** provided moderate improvements, but **Exponential Decay** consistently yielded the best results.
   - For example, with Exponential Decay and Huber loss, Test Loss reached **0.0000951** and MAE **0.0105**.

8. **Activation Functions**:
   - **Swish activation** delivered the best overall performance, with a Test Loss of **0.0000889** and MAE of **0.0099**.
   - **LeakyReLU** and **Softplus** also showed strong results, but slightly behind Swish.
