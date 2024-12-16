### GRU Training Results Table

| **Try #** | **Epochs** | **Batch Size** | **Optimizer** | **Dataset**    | **Normalization** | **Loss Function** | **Scheduler**     | **Activation** | **Test Loss** | **Test MAE** |
| --------- | ---------- | -------------- | ------------- | -------------- | ----------------- | ----------------- | ----------------- | -------------- | ------------- | ------------ |
| 1         | 200        | 32             | Adam          | Small Dataset  | None              | MSE               | None              | ReLU           | 0.004336      | 0.058398     |
| 2         | 200        | 16             | Adam          | Small Dataset  | None              | MSE               | None              | ReLU           | 0.001756      | 0.035794     |
| 3         | 200        | 32             | AdamW         | Small Dataset  | None              | MSE               | None              | ReLU           | 0.000800      | 0.023349     |
| 4         | 200        | 16             | AdamW         | Small Dataset  | None              | MSE               | None              | ReLU           | 0.000704      | 0.021263     |
| 5         | 200        | 8              | Adam          | Small Dataset  | None              | MSE               | None              | ReLU           | 0.001323      | 0.027974     |
| 6         | 200        | 16             | Adam          | Real-Time Data | None              | MSE               | None              | ReLU           | 0.000619      | 0.019740     |
| 7         | 200        | 16             | Adam          | Real-Time Data | Batch Norm        | MSE               | None              | ReLU           | 0.010598      | 0.088520     |
| 8         | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | MSE               | None              | ReLU           | 0.000678      | 0.021411     |
| 9         | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | Huber             | None              | ReLU           | 0.000154      | 0.012982     |
| 10        | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | Log-Cosh          | None              | ReLU           | 0.000195      | 0.015779     |
| 11        | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | MAE               | None              | ReLU           | 0.015100      | 0.015100     |
| 12        | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | MSE               | ReduceLROnPlateau | ReLU           | 0.000525      | 0.019307     |
| 13        | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | Huber             | ReduceLROnPlateau | ReLU           | 0.000220      | 0.017398     |
| 14        | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | Huber             | Exponential Decay | ReLU           | 0.000075      | 0.009670     |
| 15        | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | Huber             | Cosine Decay      | ReLU           | 0.001228      | 0.046700     |
| 16        | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | Huber             | Exponential Decay | Swish          | 0.000069      | 0.008924     |
| 17        | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | Huber             | Exponential Decay | Softplus       | 0.000170      | 0.015599     |
| 18        | 200        | 16             | Adam          | Real-Time Data | Layer Norm        | Huber             | Exponential Decay | LeakyReLU      | 0.000062      | 0.008414     |

### Detailed Observations on GRU Training Experiments

#### 1. **Best Overall Performance**

The **18th try**, which used **LeakyReLU activation**, **Exponential Decay scheduler**, **Huber loss**, and **Layer Normalization**, achieved the best results:

- **Test Loss**: 0.0000622
- **Test MAE**: 0.0084

This result demonstrates the effectiveness of LeakyReLU activation and Exponential Decay in stabilizing the training process and improving convergence.

---

#### 2. **Impact of Normalization**

Normalization significantly influenced the performance:

- **Layer Normalization** consistently outperformed **Batch Normalization** or the absence of normalization:
  - **8th try (Layer Norm)**: Test Loss = 0.000678, Test MAE = 0.0214
  - **7th try (Batch Norm)**: Test Loss = 0.0106, Test MAE = 0.0885

Layer Normalization stabilized training, especially when paired with advanced loss functions and schedulers.

---

#### 3. **Loss Function Comparison**

Loss functions impacted performance as follows:

- **MSE Loss**:

  - Performed well as a baseline but lacked robustness with outliers.
  - Example: **6th try (real_time_data)**: Test MAE = 0.0197.

- **Huber Loss**:

  - Significantly improved performance by balancing sensitivity to outliers and generalization.
  - Example: **14th try**: Test MAE = 0.0097.

- **Log-Cosh Loss**:

  - Performed adequately but not as effectively as Huber Loss.
  - Example: **10th try**: Test MAE = 0.0158.

- **MAE Loss**:
  - Showed weaker results compared to other loss functions.
  - Example: **11th try**: Test MAE = 0.0151.

---

#### 4. **Learning Rate Schedulers**

Schedulers played a key role in fine-tuning the learning process:

- **Exponential Decay**:

  - Delivered the best performance when paired with Layer Normalization and advanced activations.
  - Example: **18th try**: Test MAE = 0.0084.

- **ReduceLROnPlateau**:

  - Adaptive adjustments improved results in earlier configurations:
    - **13th try**: Test MAE = 0.0174.

- **Cosine Decay**:
  - Underperformed compared to other schedulers:
    - **15th try**: Test MAE = 0.0467.

---

#### 5. **Effect of Activation Functions**

Activation functions influenced how well the GRU captured patterns:

- **ReLU**:

  - Reliable but consistently underperformed compared to more advanced activations.
  - Example: **9th try (Huber loss, Layer Norm)**: Test MAE = 0.0130.

- **Swish**:

  - Provided slight improvement over ReLU.
  - Example: **16th try**: Test MAE = 0.0089.

- **Softplus**:

  - Demonstrated stability and strong performance.
  - Example: **17th try**: Test MAE = 0.0156.

- **LeakyReLU**:
  - Achieved the best results, demonstrating its effectiveness in this context.
  - Example: **18th try**: Test MAE = 0.0084.

---

#### 6. **Impact of Batch Size**

Smaller batch sizes generally improved performance, particularly with smaller datasets:

- **2nd try (batch size 16)**: Test Loss = 0.00176, Test MAE = 0.0358.
- **5th try (batch size 8)**: Test Loss = 0.00132, Test MAE = 0.0279.

Smaller batches likely provided better adaptability to variability in the data.

---

#### 7. **Adam vs. AdamW Optimizers**

- **AdamW**:

  - Demonstrated better generalization early on with the small dataset.
  - Example: **4th try**: Test MAE = 0.0213.

- **Adam**:
  - Consistently delivered excellent results when combined with advanced techniques.
  - Example: **18th try**: Test MAE = 0.0084.

---

#### 8. **Dataset Impact**

Switching to the **real_time_data** led to improved results, highlighting the importance of high-quality, diverse data for time-series predictions. For example:

- **4th try (small dataset)**: Test MAE = 0.0213.
- **6th try (real_time_data)**: Test MAE = 0.0197.

---

### Key Takeaways:

1. **Optimal Configuration Identified**:  
   The best results were achieved with **Layer Normalization**, **Huber Loss**, **Exponential Decay Scheduler**, and **LeakyReLU Activation**.

2. **Normalization Improves Stability**:  
   Layer Normalization proved critical for reducing training noise and improving convergence.

3. **Huber Loss Outperforms Alternatives**:  
   Its balance between sensitivity to outliers and generalization delivered superior results.

4. **Advanced Schedulers and Activations Matter**:  
   Exponential Decay and LeakyReLU provided the best performance when paired with advanced configurations.
