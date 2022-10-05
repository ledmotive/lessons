# Lesson 1.7: Data Preprocessing

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to understand the final steps in data preprocessing before fitting the model and testing the accuracy of the model predictions.

---

### Setup

To start this lesson, students should have:

- Completed lesson 1.6
- All previous Setup
- In this lesson we will continue working on the same Jupyter file from the previous lesson

### Learning Objectives

After this lesson, students will be able to:

- Check assumptions in a linear regression model
- Apply data transformations (box-cox and log transformation) on the numerical column
- Process numerical data with scaling

---

### Lesson 1 key concepts

> :clock10: 20 min

- Review multivariate linear regression model and its assumptions on independent variables
- Using EDA to check the assumptions in a linear regression model

  - Checking normality assumption (for the residuals)
  - Check skewness in the data

- Managing skewness in the data

:exclamation: Note for instructor: In this lesson, we will continue working with the same Jupyter file from the previous lesson. If the students do not have the file, use the following code for quick set up.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```

```python
data = pd.read_csv('regression_data1.csv')
```

<details>
<summary> Click for Code Sample </summary>

```python
data['HV1'].hist()
plt.show()
sns.distplot(data['HV1'])
plt.show()
sns.distplot(data['IC1'])
plt.show()
sns.boxplot(x=data['IC1'])
plt.show()


iqr = np.percentile(data['IC1'],75) - np.percentile(data['IC1'],25)
upper_limit = np.percentile(data['IC1'],75) + 1.5*iqr
lower_limit = np.percentile(data['IC1'],25) - 1.5*iqr
data = data[(data['IC1']>lower_limit) & (data['IC1']<upper_limit)]
sns.distplot(data['IC1'])
plt.show()
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

(_Share the following link with your students. Students should clone/download repository to be able to work on the tasks._)

- Link to [activity 1](https://github.com/ironhack-edu/data_1.07_activities/blob/master/1.07_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [activity 1 solution](https://gist.github.com/ironhack-edu/7eb4f6351bb7e6a17b4ab83ffcf282ad).

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- Data transformations:
  - Log transformations: to change the scale of a feature ranging from e.g., 0-1000000 (10**6) to 0-6

<details>
<summary> Click for Code Sample </summary>

> Log transformation

- Since some values might become -inf when we take the logarithm of values in the column that are 0, we will have to filter those values. Remember we might not able to use this method if there are too many such values and if they are important.

```python
# Code to test how the transformation will look like.
# Here we are trying two different codes. Notice that in the first function
# we are replacing the -inf values after we take logarithm by 0 while in
# the second case we will replace them with np.NaN. The idea is that
# we will then replace those NaN values in the column with the mean or
# median of the column

def log_transfom_clean1(x):
    x = np.log(x)
    if np.isfinite(x):
        return x
    else:
        return 0

HV1_log = list(map(log_transfom_clean1, data['HV1']))
sns.distplot(HV1_log)
plt.show()
```

```python
def log_transfom_clean2(x):
    x = np.log(x)
    if np.isfinite(x):
        return x
    else:
        return np.NAN # We are returning NaNs so that we can replace them with means later
data['HV1_log'] = list(map(log_transfom_clean2, data['HV1']))
# Now we replace the NA by the mean
data['HV1_log'] = data['HV1_log'].fillna(np.mean(data['HV1_log']))
sns.distplot(data['HV1_log'])
plt.show()
```

```python
data = data.drop(['HV1'], axis=1)
data.head()
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [activity 2](https://github.com/ironhack-edu/data_1.07_activities/blob/master/1.07_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/d312ba015b43c65d87fc0046ee711901).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Data transformations continued
  - Box-cox/power transformations - make the feature closer to a normal distribution. Note that this is not a strict requirement for Linear Regression. Not the data but the residuals need to be normally distributed. Still there are many methods that work better if the data is normally distributed (e.g., hypothesis testing)

<details>
<summary> Click for Code Sample </summary>

- Box-cox transformation

The data should be strictly positive to be able to use it in SciPy. We will use it on 'IC1'.

```python
# Since this is median household income, we can filter out negative values:
# len(data[data['IC1']<=0])  # number of such observations.

# if you do not want to lose this data, replace it with the mean of the column
data['IC1_'] = np.where(data['IC1']<=0,0,data['IC1'])

# now calculate mean of the new column , excluding zeros in the count
mean = np.sum(data['IC1_'])/len(data[data['IC1_']>0])

data['IC1_'] = data['IC1_'].replace(0,mean)
data = data.drop(['IC1'], axis=1)
from scipy import stats
xt, lmbda = stats.boxcox(data['IC1_'])
sns.distplot(xt)
plt.show()
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_1.07_activities/blob/master/1.07_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [activity 3 solution](https://gist.github.com/ironhack-edu/ee9dcf8a37231078c829936ba9b22e9a).

</details>

---

:coffee: **BREAK**

---

### Lesson 4 key concepts

> :clock10: 20 min

- Why do we need data preprocessing
  - make the scales for features similar because methods depend on distances
  - make the feature less skewed
- Preprocessing data using the `sklearn`: scaling features
  - Standardization using MinMaxScaler
  - Normalizing using StandardScaler
- these are the most common examples of transformations, there are more and we need to choose which ones to apply

<details>
<summary> Click for Code Sample </summary>

```python
from sklearn.preprocessing import MinMaxScaler
from sklearn.preprocessing import StandardScaler


data = pd.read_csv('regression_data.csv') # this file is inside files_for_lesson_and_activities folder

Y = data['TARGET_D']
data = data.drop(['TARGET_D'], axis=1)
X_num = data.select_dtypes(include = np.number)
X_cat = data.select_dtypes(include = np.object)
```

```python
# scaling data
transformer = MinMaxScaler().fit(X_num)
x_normalized = transformer.transform(X_num)
print(x_normalized.shape)
# pd.DataFrame(x_normalized)
```

```python
# normalizing using standard scaler
transformer = StandardScaler().fit(X_num)
x_standardized = transformer.transform(X_num)
print(x_standardized.shape)
# pd.DataFrame(x_standardized)
```

</details>

---

:coffee: **BREAK**

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Link to [activity 4](https://github.com/ironhack-edu/data_1.07_activities/blob/master/1.07_activity_4.md).

</details>

<details>
  <summary>Click for Solution: Activity 4 solutions</summary>

- Link to [activity 4 solution](https://gist.github.com/ironhack-edu/a431d2ee41c09f1cec3323bde89760e2).

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-customer-analysis-round-5](https://github.com/ironhack-labs/lab-customer-analysis-round-5)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/61d4dc9eba7bf396f40d6a5c862711bc).

</details>

---

### Additional Resources

- [Dummy Variable Trap](https://www.jigsawacademy.com/understanding-dummy-variable-traps-regression/)
- [Linear regression accuracies](https://www.dataquest.io/blog/understanding-regression-error-metrics/)
- [Model accuracies](https://scikit-learn.org/stable/modules/model_evaluation.html)
- [Choosing transformations](https://towardsdatascience.com/scale-standardize-or-normalize-with-scikit-learn-6ccc7d176a02)
