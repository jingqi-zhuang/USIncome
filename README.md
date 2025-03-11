# Income Levels Prediction

<img src="images/income.png" alt="income" style="zoom:50%;" />

Using the US Census Bureau dataset, I developed a **Neural Network model** to predict whether an individual's income exceeds $50,000 per year based on demographic attributes. Additionally, a sensitivity analysis was conducted to identify the most influential predictors, highlighting their intuitive correlation with income levels.

## Step 1: Data Preprocessing

The dataset was split into 25,000 training and 7,561 testing records, each representing an individual's demographic data from the US Census Bureau, with 14 attributes and a target variable indicating whether income exceeds $50K.

To enhance model efficiency, redundant attributes were removed. Minority categorical levels (fewer than 100 records) were merged into the mode class. Missing categorical values (over 1,000 records) were imputed using a proportional distribution approach to prevent mode overrepresentation. *Capital-gain* and *capital-loss* were combined into *capital-net* to represent net capital return.

For the ANN model, all inputs and outputs were scaled between 0 and 1. Dummy variables expanded nominal predictors to 41 inputs, while numeric and ordinal variables were normalized using min-max scaling. The binary target variable was encoded as 1 for incomes over $50K and 0 otherwise.

## Step 2: Neural Network Model Fitting

The goal was to predict whether an individual's income exceeds $50K per year using 41 input variables with an Artificial Neural Network (ANN) model. To evaluate performance, the original training dataset (25,000 records) was further split into training and validation sets in an 8:2 ratio.

To enhance interpretability, the ANN model was designed with a simple architecture: an input layer with 41 features, a single hidden layer, and an output layer with a Sigmoid activation function for binary classification. Hyperparameter tuning was performed using grid search with 5-fold cross-validation, yielding an estimated test accuracy of 84.8% with the following optimal parameters:

- Hidden layer neurons: 64
- Activation function: ReLU
- Batch size: 64
- Epochs: 20
- Optimizer: Adam

The topology of the resulting network is as follows:

![](/Users/apple/Desktop/income/images/ANN.png)

## Step 3: Predictive Performance

Using the 20% test data, the overall accuracy is 85%, demonstrating a strong predictive power. The confusion matrix obtained using the 20% test data:

<img src="/Users/apple/Desktop/income/images/confusion matrix.png" style="zoom:70%;" />

The model achieved a sensitivity of 0.54 and a specificity of 0.94. The high False Negative rate suggests that many individuals earning over $50K are misclassified as earning less. While this conservative error helps mitigate financial risk by preventing loans to potentially risky applicants, it may also limit customer acquisition by denying credit to eligible individuals.

## Step 4: Variable Importance

To identify the key variables driving income predictions, I conducted a **sensitivity analysis** to assess each feature's influence on the model. First, a baseline prediction was established by setting all input features to their mean values. Then, each feature was individually varied from its minimum to maximum while keeping all others constant, allowing us to measure its impact on the model’s output.  The top 10 important predictors are as follow.

| **Rank** |       **Attribute**        | **Max vs Min** | **Min vs Mean** | **Max vs Min** |
| :------: | :------------------------: | :------------: | :-------------: | :------------: |
|    1     |        Capital-Net         |     0.825      |      0.125      |      0.95      |
|    2     |       Education-Num        |     0.436      |      0.163      |     0.599      |
|    3     |       Hours-Per-Week       |     0.363      |      0.129      |     0.492      |
|    4     |   Relationship-Own-Child   |     0.154      |      0.072      |     0.226      |
|    5     | Relationship-Not-In-Family |     0.132      |      0.087      |     0.219      |
|    6     |            Age             |     0.082      |      0.124      |     0.206      |
|    7     |         Race-White         |     0.035      |      0.166      |     0.201      |
|    8     |   Relationship-Unmarried   |     0.153      |      0.031      |     0.184      |
|    9     | Occupation-Priv-House-Serv |     0.172      |      0.005      |     0.176      |
|    10    |         Race-Black         |     0.146      |      0.025      |     0.171      |

The sensitivity analysis revealed that **capital-net** is the most influential predictor, representing an individual's net return on capital. Other key variables include **education-num**, **hours-per-week**, and **relationship-own-child**. These findings align with intuition—higher education levels and longer work hours often correlate with increased earning potential. Additionally, individuals with children are more likely to be in later career stages, where higher incomes are typically expected.

### The Most and Least Important Numeric Variables

Among numerical variables, **capital-net** is the most influential, while **demogweight** is the least.

The histogram of **capital-net** reveals a clear separation between income groups, emphasizing its strong predictive power. The distinct distribution gap between higher and lower income categories underscores its value in classifying individuals by income level. In contrast, the histogram of **demogweight** shows significant overlap between income groups, indicating minimal impact on income prediction.

![](/Users/apple/Desktop/income/images/capital net.png)

![](/Users/apple/Desktop/income/images/demogweight.png)

### The Most Important Categorical Variables

The most influential categorical variables in predicting income are **education level, marital status, and occupation level**.

**Education Level:** Higher education levels strongly correlate with higher income. A significant increase in the proportion of individuals earning over \$50K is observed among those with a **Bachelor’s degree or higher**, whereas most individuals with lower education levels fall into the ≤\$50K income category.

<img src="/Users/apple/Desktop/income/images/education.png"  />

**Marital status:** Nearly all individuals who are not married earn ≤\$50K, while those who are **married with a spouse present** show a significantly higher proportion of incomes exceeding \$50K.

![](/Users/apple/Desktop/income/images/marital status.png)

**Occupation:** Individuals working in Other Services and Private House Services predominantly earn ≤\$50K. Similarly, most manual laborers, including Handlers-Cleaners and Machine Operators/Inspectors, fall into the low income category. In contrast, a significantly higher proportion of individuals earning >\$50K are found in **executive, managerial, and professional specialty roles**.

![](/Users/apple/Desktop/income/images/occupation.png)