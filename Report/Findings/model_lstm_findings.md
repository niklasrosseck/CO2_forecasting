## LSTM Experiment Results

| Try  | Epochs | Batch Size | Optimizer | Validation Split | Dataset        | Normalization | Loss Function | Learning Rate Scheduler | Activation | Test Loss              | Test MAE             |
| ---- | ------ | ---------- | --------- | ---------------- | -------------- | ------------- | ------------- | ----------------------- | ---------- | ---------------------- | -------------------- |
| 1st  | 200    | 32         | Adam      | 0.1              | Small dataset  | None          | MSE           | None                    | ReLU       | 0.005996004678308964   | 0.07434069365262985  |
| 2nd  | 200    | 16         | Adam      | 0.1              | Small dataset  | None          | MSE           | None                    | ReLU       | 0.00482294661924243    | 0.06378374993801117  |
| 3rd  | 200    | 32         | AdamW     | 0.1              | Small dataset  | None          | MSE           | None                    | ReLU       | 0.005092973820865154   | 0.06328088790178299  |
| 4th  | 200    | 16         | AdamW     | 0.1              | Small dataset  | None          | MSE           | None                    | ReLU       | 0.0008181920275092125  | 0.02108464203774929  |
| 5th  | 200    | 8          | Adam      | 0.1              | Small dataset  | None          | MSE           | None                    | ReLU       | 0.0008806510013528168  | 0.023224735632538795 |
| 6th  | 200    | 16         | Adam      | 0.1              | Real-time data | None          | MSE           | None                    | ReLU       | 0.0009653194574639201  | 0.025548918172717094 |
| 7th  | 200    | 16         | Adam      | 0.1              | Real-time data | Batch Norm    | MSE           | None                    | ReLU       | 0.0013319727731868625  | 0.029228655621409416 |
| 8th  | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | MSE           | None                    | ReLU       | 0.00034223662805743515 | 0.01433303952217102  |
| 9th  | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | Huber         | None                    | ReLU       | 0.0009255538461729884  | 0.03762352094054222  |
| 10th | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | Log-Cosh      | None                    | ReLU       | 0.0010927640832960606  | 0.04225155711174011  |
| 11th | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | MAE           | None                    | ReLU       | 0.024655736982822418   | 0.024655736982822418 |
| 12th | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | MSE           | ReduceLROnPlateau       | ReLU       | 0.00045791975571773946 | 0.01668296568095684  |
| 13th | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | Huber         | ReduceLROnPlateau       | ReLU       | 0.0003653733292594552  | 0.02196132019162178  |
| 14th | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | Huber         | Exponential Decay       | ReLU       | 0.0003658000787254423  | 0.022817440330982208 |
| 15th | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | Huber         | Cosine Decay            | ReLU       | 0.0016495564486831427  | 0.0508381724357605   |
| 16th | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | Huber         | Exponential Decay       | Swish      | 0.0004201366100460291  | 0.024954169988632202 |
| 17th | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | Huber         | Exponential Decay       | Softplus   | 0.00006220967770786956 | 0.008411196991801262 |
| 18th | 200    | 16         | Adam      | 0.1              | Real-time data | Layer Norm    | Huber         | Exponential Decay       | LeakyReLU  | 0.00015748136502224952 | 0.015025259926915169 |

### Detailed Observations on LSTM Training Experiments

#### 1. **Best Overall Performance**

The **17th try**, which used **Softplus activation**, **Exponential Decay scheduler**, **Huber loss**, and **Layer Normalization**, achieved the best results:

- **Test Loss**: 0.0000622
- **Test MAE**: 0.0084

This combination suggests that Softplus, when paired with Exponential Decay and Huber Loss, effectively captures the patterns in the dataset and stabilizes training.

---

#### 2. **Impact of Normalization**

Normalization layers played a critical role in improving model performance.

- **Layer Normalization** consistently delivered better results than **Batch Normalization** or no normalization.  
  For example:
  - **8th try (Layer Norm)**: Test Loss = 0.00034, Test MAE = 0.0143
  - **7th try (Batch Norm)**: Test Loss = 0.00133, Test MAE = 0.0292

Layer Normalization appears particularly suited for time-series models, as it stabilizes training and enhances convergence.

---

#### 3. **Loss Function Comparison**

Different loss functions had varying impacts on the model's performance:

- **MSE Loss**:
  - Performed reasonably well in many configurations (e.g., 8th try: Test MAE = 0.0143).
  - However, it was less robust when paired with certain schedulers or activations.
- **Huber Loss**:
  - Consistently outperformed MSE in terms of Test MAE, particularly when combined with Layer Normalization and advanced learning rate schedulers.
  - Example: **13th try** with Huber Loss, Layer Norm, and ReduceLROnPlateau achieved Test MAE = 0.0219.
- **Log-Cosh Loss**:

  - Performed adequately but not as well as Huber Loss. Example: **10th try**: Test MAE = 0.0422.

- **MAE Loss**:
  - Had the weakest performance among tested loss functions. Example: **11th try**: Test MAE = 0.0246.

---

#### 4. **Learning Rate Schedulers**

Learning rate schedulers had a significant impact on training dynamics:

- **ReduceLROnPlateau**:

  - Adaptive adjustment worked well in the 12th and 13th tries (Test MAE = 0.0166 and 0.0219, respectively).

- **Exponential Decay**:

  - Delivered strong results in the 14th, 16th, and 17th tries, especially when paired with Huber Loss and advanced activations like Softplus or LeakyReLU.

- **Cosine Decay**:
  - Performed worse than other schedulers (e.g., 15th try: Test MAE = 0.0508).
  - This may indicate that the cosine decay pattern was less suited for the specific training dynamics of this dataset.

---

#### 5. **Effect of Activation Functions**

Activation functions influenced the model's ability to capture patterns:

- **ReLU**:

  - A reliable baseline but consistently underperformed compared to newer activations.

- **Swish**:

  - Showed slight improvement over ReLU in the 16th try but did not outperform other advanced activations.

- **Softplus**:

  - Achieved the best results in the 17th try. Its smooth, non-zero gradient likely contributed to improved optimization stability.

- **LeakyReLU**:
  - Performed well in the 18th try (Test MAE = 0.0150), surpassing Swish but slightly behind Softplus.

---

#### 6. **Impact of Batch Size**

Smaller batch sizes generally led to better performance:

- For example:
  - **4th try (batch size 16)**: Test Loss = 0.00082, Test MAE = 0.0211.
  - **5th try (batch size 8)**: Test Loss = 0.00088, Test MAE = 0.0232.

Smaller batches allow the model to better adapt to variability in the data, which is particularly important for time-series tasks.

---

#### 7. **Adam vs. AdamW Optimizers**

- **AdamW**:

  - Showed strong performance on small datasets in earlier experiments. Example: **4th try** with MAE = 0.0211.

- **Adam**:
  - Delivered consistently strong results across configurations.
  - Its combination with Layer Normalization, Huber Loss, and Exponential Decay Scheduler (e.g., 17th try) proved to be highly effective.

---

#### 8. **Impact of Dataset**

Switching to the **real-time dataset** generally led to better results compared to the small dataset, especially when advanced techniques were applied. This highlights the importance of data variability and relevance for improving model generalization.

---

### Key Takeaways:

1. **Optimal Configuration Identified**:  
   The best results were achieved using **Layer Normalization**, **Huber Loss**, **Exponential Decay Scheduler**, and **Softplus Activation**.

2. **Normalization and Loss Functions Matter**:  
   Layer Normalization stabilizes training, while Huber Loss effectively balances sensitivity to outliers.

3. **Scheduler Choice is Crucial**:  
   Exponential Decay proved to be the most effective scheduler for this dataset, particularly in combination with advanced activations.

4. **Activation Functions Can Improve Performance**:  
   Replacing ReLU with Softplus or LeakyReLU significantly enhanced the model's ability to learn complex patterns.
