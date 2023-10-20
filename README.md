# account-fraud-detection
Machine Learning, MLOPs and Data Strategy case study for a real-world fraud dataset


**Analysis & Results** 

**Key Insights:** 

• High-fraud accounts: Approximately 2.5% of the accounts are solely involved in fraudulent activities. The top account in terms of fraudulent transactions had 26 occurrences within the given time period. Such high-risk and high frequency accounts require close monitoring and investigation to mitigate losses. 

• Accounts with both good and fraudulent transactions: 103 accounts (0.4% of total) have both good and fraudulent transactions. Assuming that there are no false positives in the data, this may imply that the good transactions associated with these accounts are mislabelled, representing a scenario where it might take a couple of transactions to detect fraudulent behavior. 

• Device / Browser associations: 

• Browser16: This browser seems to be associated with the highest number of transactions and fraudulent activities. Analyzing this association can provide valuable insights to improve fraud detection mechanisms. 

• Device_feat_2 = 'device_4564': Around 32% of all fraudulent activities in the dataset are associated with this specific device. Focusing on monitoring and investigating transactions from this device can enhance fraud detection accuracy. 

• Correlation and Feature Selection: Some features exhibit high correlation with values above 0.90. Removing redundant features with a correlation of 1 improves model performance. 


**Model Selection & Evaluation:** 

Given the dataset's characteristics, tree-based models _Random Forest_ and _XGBoost_ were selected due to their ability to handle mixed data and imbalanced classes effectively. To optimize model performance, random search cross validation and hyperparameter tuning (RandomizedSearchCV) is used ensuring robust estimates of the model's capabilities. 

Due to the dataset's imbalance, Precision-Recall (PR) curves and F1 score are used for evaluation. The primary focus was achieving a _recall rate_ within the desired range of 70-90%, while minimizing _false positives_ (frictions to legitimate customers). These metrics are used to assess the model's ability to correctly detect fraud cases and balance precision to prevent inconvenience to genuine customers.

**Results:** 

The Final model selected, XGBoost with a decision threshold of 0.55, can detect 73.1% fraud requests keeping the risk of incorrectly labelling good requests as fraud to as low as 0.8%. 

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|**Table 1: Key parameters and results Model**|**Decision Threshold**|**Model Comparison (F1 score)**|**% of Fraud Detected (Recall)**|**% of Correct Predictions (Precision)**|**% of False Detections (False Positive Rate)**|
|Random Forest|0.35 <br><br>(optimal)|0.80|79.5%|80.7%|2.0%|
|Random Forest|0.50|0.78|70.0%|87.9%|1.0%|
|XGBoost|0.40 <br><br>(optimal)|0.81|77.0%|86.0%|1.3%|
|**XGBoost**|**0.55**|**0.81**|**73.1%**|**90.3%**|**0.8%**|


• Model Comparison: We use F1 score to compare model performance. (Higher the better). It measures the balance between precision and recall metrics. XGBoost has the highest F1 score and better performance over Random Forest. 

• Tradeoffs: We need to consider a trade-off between Recall, Precision and False Positive Rate when choosing a model. For every model, we can adjust these metrics based on a decision threshold: 

• % of Fraud Detected (Recall): For the threshold of 0.1, we classify the vast majority of transactions as fraudulent and hence get really high recall of ~0.9. As the threshold increases the recall falls. Increase recall when you care about catching all fraudulent transactions even at a cost of false alerts. 

• Although Random Forest achieved the highest recall of 85.9%, it has the lowest precision of 54.4% and highest false positive rate of 6.6%. XGBoost performed well with a recall of 79.2% and a precision of 76.5%. The false positive rate was 2.6%, and the F1 score was 0.78. 

• % of Correct Predictions (Precision): The higher the threshold the better the precision. When raising false alerts is costly, and you want all the positive predictions to be worth looking at you should optimize for precision. 

• % of False Detections (False Positive Rate) - Fraction of false alerts given by the model. If the cost of dealing with friction is high, we optimize for this metric. If we increase the threshold observations with very high predictions will be classified as positive. In our example, we can see that to reach perfect FPR of 0 we need to increase the threshold to 0.85. 

• By increasing the decision threshold from model-optimal (0.4) to 0.55, we can reduce the false positive rate from 1.3% to 0.8% by compromising on recall. Increasing the threshold any further, would decrease the friction rate but may not be worth the decrease in recall.