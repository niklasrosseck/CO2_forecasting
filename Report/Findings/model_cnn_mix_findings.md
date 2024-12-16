## Tabular Results for CNN-LSTM Hybrid

| **Try** | **Epochs** | **Batch Size** | **Optimizer** | **Dataset** | **Normalization** | **Loss Function** | **Learning Rate Scheduler** | **Activation** | **Test Loss** | **Test MAE** |
| ------- | ---------- | -------------- | ------------- | ----------- | ----------------- | ----------------- | --------------------------- | -------------- | ------------- | ------------ |
| 1       | 200        | 32             | Adam          | Small       | None              | MSE               | None                        | ReLU           | 0.004557      | 0.059732     |
| 2       | 200        | 16             | Adam          | Small       | None              | MSE               | None                        | ReLU           | 0.000643      | 0.019701     |
| 3       | 200        | 32             | AdamW         | Small       | None              | MSE               | None                        | ReLU           | 0.002979      | 0.049934     |
| 4       | 200        | 16             | AdamW         | Small       | None              | MSE               | None                        | ReLU           | 0.011453      | 0.094553     |
| 5       | 200        | 8              | Adam          | Small       | None              | MSE               | None                        | ReLU           | 0.013198      | 0.099721     |
| 6       | 200        | 16             | Adam          | Real-Time   | None              | MSE               | None                        | ReLU           | 0.000836      | 0.022179     |
| 7       | 200        | 16             | Adam          | Real-Time   | Batch Norm        | MSE               | None                        | ReLU           | 0.005776      | 0.067273     |
| 8       | 200        | 16             | Adam          | Real-Time   | Layer Norm        | MSE               | None                        | ReLU           | 0.001667      | 0.033760     |
| 9       | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | None                        | ReLU           | 0.000571      | 0.027284     |
| 10      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Log-Cosh          | None                        | ReLU           | 0.000393      | 0.020814     |
| 11      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | MAE               | None                        | ReLU           | 0.035321      | 0.035321     |
| 12      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | MSE               | ReduceLROnPlateau           | ReLU           | 0.001188      | 0.029010     |
| 13      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | ReduceLROnPlateau           | ReLU           | 0.000833      | 0.033669     |
| 14      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | Exponential Decay           | ReLU           | 0.000360      | 0.021259     |
| 15      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | Cosine Decay                | ReLU           | 0.000698      | 0.028842     |
| 16      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | Exponential Decay           | Swish          | 0.000353      | 0.020598     |
| 17      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | Exponential Decay           | Softplus       | 0.000238      | 0.017963     |
| 18      | 200        | 16             | Adam          | Real-Time   | Layer Norm        | Huber             | Exponential Decay           | LeakyReLU      | 0.000272      | 0.019271     |

---

## Observations for CNN-LSTM Hybrid

1. **Initial Attempts on Small Dataset**:

   - Performance varies with optimizer and batch size.
   - Best performance on the small dataset achieved with **Adam optimizer**, **batch size 16**, and **MSE loss**, yielding **Test Loss: 0.000643** and **MAE: 0.0197**.

2. **AdamW Optimizer**:

   - Results are generally worse compared to Adam, showing higher loss and MAE values.

3. **Batch Size Impact**:

   - Decreasing batch size to 8 degraded performance, with **Test Loss: 0.0132** and **MAE: 0.0997**, possibly due to unstable updates.

4. **Transition to Real-Time Dataset**:

   - Transitioning to the real-time dataset significantly improved performance. For example, using Adam with **batch size 16** resulted in **Test Loss: 0.000836** and **MAE: 0.0222**.

5. **Normalization Techniques**:

   - **Batch Normalization** led to higher loss values, demonstrating inefficacy for this model and dataset.
   - **Layer Normalization** greatly improved results, with **Test Loss: 0.001667** and **MAE: 0.0338** when combined with MSE loss.

6. **Loss Functions**:

   - **Huber Loss** consistently outperformed MSE, MAE, and Log-Cosh in most configurations.
   - **Log-Cosh Loss** also provided competitive results, slightly worse than Huber Loss.

7. **Learning Rate Schedulers**:

   - **Exponential Decay** outperformed other learning rate schedulers, achieving **Test Loss: 0.000360** and **MAE: 0.0213**.
   - **ReduceLROnPlateau** provided moderate improvements.

8. **Activation Functions**:
   - **Softplus activation** delivered the best results, with **Test Loss: 0.000238** and **MAE: 0.0180**.
   - **Swish activation** and **LeakyReLU** also performed well, but slightly behind Softplus.
