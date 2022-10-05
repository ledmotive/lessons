# Lesson 1.8: Encoding the model

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to learn how to process categorical data before fitting the model, fit the model, make predictions on the test data, and check the accuracy of the model. We will also talk about other models used for supervised and unsupervised learning.

---

### Setup

To start this lesson, students should have:

- Completed lesson 1.7
- All previous Setup

### Learning Objectives

After this lesson, students will be able to:

- Encode categorical data
- Fit the model on the training data
- Make predictions on the test data
- Check the accuracy of the model using different statistical measures

---

### Lesson 1 key concepts

> :clock10: 20 min

- Categorical data - nominal, ordinal
- Encoding categorical variables
  - Label encoding
  - One Hot encoding

:exclamation: Note for instructor: Students can continue using the same Jupyter file from the last lesson. If the students do not have that, they can use the following code to quickly set up:

```python
data = pd.read_csv('regression_data.csv')
Y = data['TARGET_D']
data = data.drop(['TARGET_D'], axis=1)
X_num = data.select_dtypes(include = np.number)
from sklearn.preprocessing import Normalizer
transformer = Normalizer().fit(X_num)
x_normalized = transformer.transform(X_num)
print(x_normalized.shape)
# pd.DataFrame(x_normalized)
```

<details>
<summary> Click for Code Sample </summary>

Links to docs:

- [sklearn.preprocessing.OneHotEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html)
- [.fit(x)](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder.fit)
- [.transform(x)](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder.transform)

```python
X_cat = data.select_dtypes(include = np.object)
# pd.get_dummies(X_cat, drop_first=True)
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder(handle_unknown='error', drop='first').fit(X_cat)
# encoder.categories_
encoded = encoder.transform(X_cat).toarray()
encoded
#le = preprocessing.LabelEncoder().fit(X_cat).transform(X_cat) # ordered wrt value counts
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [activity 1](https://github.com/ironhack-edu/data_1.08_activities/blob/master/1.08_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

Keeping One Hot encoding as the reference, talk about how multiple categories in a single column can add a large number of additional columns in the data set. Given that there might be many such categorical columns, this might make the data set sparse, by adding a large number of columns with binary values.

</details>

---

:coffee: **BREAK**

---

### Lesson 2 key concepts

> :clock10: 20 min

- Fitting the model with processed data
- Understanding the documentation
- Making predictions
  - Predictions on the test data
  - Predictions on new data

<details>
<summary> Click for Code Sample </summary>

Links to docs:

- [sklearn.model_selection.train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html#)
- [sklearn.linear_model.LinearRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)

```python
X = np.concatenate((x_normalized, encoded), axis=1)
#Y = data['TARGET_D']. This columns was already droped from 'data'
X_train, X_test, y_train, y_test = train_test_split(X, Y, test_size=0.4, random_state=100)
lm = linear_model.LinearRegression()
model = lm.fit(X_train,y_train)
predictions  = lm.predict(X_test)
r2_score(y_test, predictions)
# to make predictions on the new data, we have to process the data (X features) in the same way.
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

(_Share the following link with your students. Students should clone/download repository to be able to work on the tasks._)

- Link to [activity 2](https://github.com/ironhack-edu/data_1.08_activities/blob/master/1.08_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [activity 2 solution](https://gist.github.com/ironhack-edu/977e3f18c6035f3f2476a4baf3ab6bf0).

</details>

---

:coffee: **BREAK**

---

### Lesson 3 key concepts

> :clock10: 20 min

- Checking the accuracy of the model
  - RMSE
  - MSE
  - R square

<details>
<summary> Click for Code Sample </summary>

```python
mse = mean_squared_error(y_test, predictions)
print(mse)

rmse = math.sqrt(mse)
print(rmse)

r2 = r2_score(y_test, predictions)
print(r2)

n = len(X_test)
p = X_test.shape[1]
adj_r2 = 1-((1-r2)*(n-1)/(n-p-1))
print(adj_r2)
```

</details>

---

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [activity 3](https://github.com/ironhack-edu/data_1.08_activities/blob/master/1.08_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- MSE is bigger than RMSE, but they have the same information about the error. RMSE has the same magnitude order as the data.

</details>

---

:coffee: **BREAK**

---

### Lesson 4 key concepts

> :clock10: 20 min

- Recap the complete process
- Discussion

  - Regression and classification models that will be covered - a quick intro
    - KNN, SVM, decision trees, random forests, neural networks

- Introduce unsupervised machine learning models
  - Clustering algorithms that will be covered - a quick intro
    - K-means clustering, DBSCAN

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 4 </summary>

- Link to [activity 4](https://github.com/ironhack-edu/data_1.08_activities/blob/master/1.08_activity_4.md).

</details>

---

:coffee: **BREAK**

---

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-customer-analysis-round-6](https://github.com/ironhack-labs/lab-customer-analysis-round-6)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/3752be235fc0529beffd785a0e6938cb).

</details>
