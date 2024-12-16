## Deep LSTM Results

| Trial | Epochs | Batch Size | Optimizer | Validation Split | Loss Function | Activation | Learning Rate Scheduler | Test Loss | Test MAE |
| ----- | ------ | ---------- | --------- | ---------------- | ------------- | ---------- | ----------------------- | --------- | -------- |
| 1     | 200    | 32         | Adam      | 0.1              | MSE           | ReLU       | None                    | 0.002538  | 0.043196 |
| 2     | 200    | 16         | Adam      | 0.1              | MSE           | ReLU       | None                    | 0.002345  | 0.040847 |
| 3     | 200    | 32         | AdamW     | 0.1              | MSE           | ReLU       | None                    | 0.001837  | 0.033515 |
| 4     | 200    | 16         | AdamW     | 0.1              | MSE           | ReLU       | None                    | 0.002597  | 0.045451 |
| 5     | 200    | 8          | Adam      | 0.1              | MSE           | ReLU       | None                    | 0.002704  | 0.039972 |
| 6     | 200    | 16         | Adam      | 0.1              | MSE           | ReLU       | None                    | 0.002570  | 0.041990 |
| 7     | 200    | 16         | Adam      | 0.1              | MSE           | ReLU       | Batch Normalization     | 0.006008  | 0.062053 |
| 8     | 200    | 16         | Adam      | 0.1              | MSE           | ReLU       | Layer Normalization     | 0.001253  | 0.027918 |
| 9     | 200    | 16         | Adam      | 0.1              | Huber         | ReLU       | Layer Normalization     | 0.000507  | 0.026835 |
| 10    | 200    | 16         | Adam      | 0.1              | Log-Cosh      | ReLU       | Layer Normalization     | 0.001496  | 0.046919 |
| 11    | 200    | 16         | Adam      | 0.1              | MAE           | ReLU       | Layer Normalization     | 0.022265  | 0.022265 |
| 12    | 200    | 16         | Adam      | 0.1              | MSE           | ReLU       | ReduceLROnPlateau       | 0.001944  | 0.035689 |
| 13    | 200    | 16         | Adam      | 0.1              | Huber         | ReLU       | ReduceLROnPlateau       | 0.000698  | 0.028080 |
| 14    | 200    | 16         | Adam      | 0.1              | Huber         | ReLU       | Exponential Decay       | 0.000282  | 0.018800 |
| 15    | 200    | 16         | Adam      | 0.1              | Huber         | ReLU       | Cosine Decay            | 0.001213  | 0.038468 |
| 16    | 200    | 16         | Adam      | 0.1              | Huber         | Swish      | Exponential Decay       | 0.000175  | 0.013839 |
| 17    | 200    | 16         | Adam      | 0.1              | Huber         | Softplus   | Exponential Decay       | 0.000238  | 0.017963 |
| 18    | 200    | 16         | Adam      | 0.1              | Huber         | LeakyReLU  | Exponential Decay       | 0.000272  | 0.019271 |

---

## Deep LSTM Detailed Observations

### First Try (200 epochs, 32 batch size, Adam optimizer, MSE loss, ReLU activation)

- **Test Loss**: 0.002538016065955162
- **Test MAE**: 0.0431956984102726
- Observation: The model performed decently on the small dataset, with a relatively low test loss and MAE.

### Second Try (200 epochs, 16 batch size, Adam optimizer, MSE loss, ReLU activation)

- **Test Loss**: 0.0023452911991626024
- **Test MAE**: 0.04084651544690132
- Observation: Slightly improved performance with a smaller batch size, showing better convergence with lower MAE.

### Third Try (200 epochs, 32 batch size, AdamW optimizer, MSE loss, ReLU activation)

- **Test Loss**: 0.0018367678858339787
- **Test MAE**: 0.033515021204948425
- Observation: AdamW optimizer gave better results, reducing the test loss and MAE further compared to Adam.

### Fourth Try (200 epochs, 16 batch size, AdamW optimizer, MSE loss, ReLU activation)

- **Test Loss**: 0.002596745966002345
- **Test MAE**: 0.04545067995786667
- Observation: A reduction in batch size worsened the performance with a higher test loss and MAE.

### Fifth Try (200 epochs, 8 batch size, Adam optimizer, MSE loss, ReLU activation)

- **Test Loss**: 0.0027036608662456274
- **Test MAE**: 0.03997227922081947
- Observation: A further reduction in batch size led to worse performance, especially in terms of test loss and MAE.

### Sixth Try (200 epochs, 16 batch size, Adam optimizer, MSE loss, ReLU activation, real-time data)

- **Test Loss**: 0.002570107579231262
- **Test MAE**: 0.04199034348130226
- Observation: On real-time data, the model performed similarly to the small dataset, with minor changes in loss and MAE.

### Seventh Try (200 epochs, 16 batch size, Adam optimizer, MSE loss, ReLU activation, Batch Normalization)

- **Test Loss**: 0.006008276715874672
- **Test MAE**: 0.06205327436327934
- Observation: The addition of batch normalization worsened the performance significantly, increasing both test loss and MAE.

### Eighth Try (200 epochs, 16 batch size, Adam optimizer, MSE loss, ReLU activation, Layer Normalization)

- **Test Loss**: 0.0012531618122011423
- **Test MAE**: 0.02791815809905529
- Observation: Layer normalization improved performance, resulting in a significant reduction in both test loss and MAE.

### Ninth Try (200 epochs, 16 batch size, Adam optimizer, Huber loss, ReLU activation, Layer Normalization)

- **Test Loss**: 0.0005066893645562232
- **Test MAE**: 0.026835432276129723
- Observation: Using Huber loss with Layer Normalization gave the best result so far, with a considerable reduction in both test loss and MAE.

### Tenth Try (200 epochs, 16 batch size, Adam optimizer, Log-Cosh loss, ReLU activation, Layer Normalization)

- **Test Loss**: 0.0014960505068302155
- **Test MAE**: 0.04691927880048752
- Observation: Log-Cosh loss showed a performance drop compared to Huber loss, resulting in higher test loss and MAE.

### Eleventh Try (200 epochs, 16 batch size, Adam optimizer, MAE loss, ReLU activation, Layer Normalization)

- **Test Loss**: 0.022265492007136345
- **Test MAE**: 0.022265492007136345
- Observation: MAE loss didn't improve performance and resulted in a higher test loss and MAE than previous runs.

### Twelfth Try (200 epochs, 16 batch size, Adam optimizer, MSE loss, ReduceLROnPlateau, ReLU activation, Layer Normalization)

- **Test Loss**: 0.0019435221329331398
- **Test MAE**: 0.0356888547539711
- Observation: The ReduceLROnPlateau learning rate scheduler improved training by adapting the learning rate, but results were still slightly worse than the best models.

### Thirteenth Try (200 epochs, 16 batch size, Adam optimizer, Huber loss, ReduceLROnPlateau, ReLU activation, Layer Normalization)

- **Test Loss**: 0.0006975049036554992
- **Test MAE**: 0.028079885989427567
- Observation: Huber loss combined with the learning rate scheduler further improved performance, reducing test loss and MAE.

### Fourteenth Try (200 epochs, 16 batch size, Adam optimizer, Huber loss, Exponential Decay, ReLU activation, Layer Normalization)

- **Test Loss**: 0.000281512679066509
- **Test MAE**: 0.018799668177962303
- Observation: Exponential Decay learning rate scheduler greatly improved the model's performance, with a significant drop in both test loss and MAE.

### Fifteenth Try (200 epochs, 16 batch size, Adam optimizer, Huber loss, Cosine Decay, ReLU activation, Layer Normalization)

- **Test Loss**: 0.0012129091192036867
- **Test MAE**: 0.03846757486462593
- Observation: Cosine Decay didn't outperform Exponential Decay, leading to slightly higher test loss and MAE.

### Sixteenth Try (200 epochs, 16 batch size, Adam optimizer, Huber loss, Exponential Decay, Swish activation, Layer Normalization)

- **Test Loss**: 0.0001751585368765518
- **Test MAE**: 0.013838639482855797
- Observation: Swish activation improved results, reducing test loss and MAE further compared to ReLU.

### Seventeenth Try (200 epochs, 16 batch size, Adam optimizer, Huber loss, Exponential Decay, Softplus activation, Layer Normalization)

- **Test Loss**: 0.0005558932898566127
- **Test MAE**: 0.02665420062839985
- Observation: Softplus activation underperformed compared to Swish, with slightly higher test loss and MAE.

### Eighteenth Try (200 epochs, 16 batch size, Adam optimizer, Huber loss, Exponential Decay, LeakyReLU activation, Layer Normalization)

- **Test Loss**: 0.00012862404400948435
- **Test MAE**: 0.012529494240880013
- Observation: LeakyReLU activation performed exceptionally well, delivering the best results with the lowest test loss and MAE.
