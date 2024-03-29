# KNN Classification and Regression Library for Java
* An easy-to-use Java API for KNN classification and regression. 
* Indepdent implementation of KNN, does not make use of 3rd-party machine learning libraries. 
* Android-compatible.

----
## Introduction to KNN
**Concepts**

![knn-classifier](https://upload.wikimedia.org/wikipedia/commons/6/6a/Knn-Class.png) 

The core of KNN is a majority voting mechanism. The mechanism involves finding the potential candidate(s) which shares the highest degree of similarity of an given instance. The similarity determined by a loss function, e.g. mean squared error. The 'K' refers to the number of best candidates to be chosen and should be an odd number in the case of classification. The outcome of a prediction is determined by the majority of assigned label from the best candidates.

**Example**

Using MSE(Mean squared error) as a loss function with 1NN:
Given three vectors, label assigned and trained by KNN classifier:

* [1,5,10] = LABEL-1

* [5,10,15] = LABEL-2

* [10,15,20] = LABEL-3

Given a test object(label-less):

* [2,6,11]

Q: Which label has the highest similarity when compared to the test object?

A: LABEL-1, since the test object([2,6,11]) has the smallest difference when compared to training data([1,5,10]). Each attribute has only a difference of 1 when calculated using MSE(loss function).


## Getting Started
This section provides examples on using the KNN classification and regression library.

The procedures are:
1. Create an object of KNN classifier/regressor
2. Create a training instance
     - set training instance attribute
     - set training instance label
3. Create a test instance
     - set test instance attribute
4. Perform prediction on classification/regression


**Example of using a KNN Classifier**

Classifier is used to predict an output that is in discrete value, e.g. gender, color, names etc.

```
public static void main(String[] args){

    /* Example of KNN classifier using nearest neighbor of 1 */
    KNN kNN = new KNN(1, true);

    kNN.createInstance();
    kNN.setInstanceAttr("attr1",26.0);
    kNN.setInstanceAttr("attr2",79.0);
    kNN.setInstanceAttr("attr3",15.0);
    kNN.setInstanceAttr("attr4",78.0);
    kNN.setInstanceLabel("label1");
    //...iteratively create training instances
    
    kNN.createTestInstance();
    kNN.setTestInstanceAttr("attr1",26.0);
    kNN.setTestInstanceAttr("attr2",79.0);
    kNN.setTestInstanceAttr("attr3",15.0);
    kNN.setTestInstanceAttr("attr4",78.0);

    String prediction=kNN.predict();
    System.out.println(prediction);
}
```

**Example of using a KNN Regressor**

Regressor is used to predict an output that is in continous value, e.g. temperature, height, stock price etc.

```
public static void main(String[] args){

    /* Example of KNN regressor using nearest neighbor of 2 */
    KNN kNN = new KNN(2, false);

    kNN.createInstance();
    kNN.setInstanceAttr("attr1",26.0);
    kNN.setInstanceAttr("attr2",79.0);
    kNN.setInstanceAttr("attr3",15.0);
    kNN.setInstanceAttr("attr4",78.0);
    kNN.setInstanceLabel(30);

    //...iteratively create training instances
    
    kNN.createTestInstance();
    kNN.setTestInstanceAttr("attr1",13.0);
    kNN.setTestInstanceAttr("attr2",45.0);
    kNN.setTestInstanceAttr("attr3",55.0);
    kNN.setTestInstanceAttr("attr4",98.0);

    double prediction=kNN.predict();
    System.out.println(prediction);
}
```


## Loss Function: Mean Squared Error

**Mean Squared Error**

Mean squared  error isa  widely used loss-function in statistics. When integrated with KNN, it tests the similarity between training and test data  and produces the average error between the two vectors. MSE is a positive integer. When MSE is 0, the two vectors have the same value(identical). A greater MSE implies greater difference between two vectors(more errors). 

**The MSE equation**:

![mean-squared-error](https://wikimedia.org/api/rest_v1/media/math/render/svg/e258221518869aa1c6561bb75b99476c4734108e)

* MSE is the mean of the squares of the errors
* the 'n' refers to the number of attributes compared
* Y represents the attribute value from test data
* Y-prime represents attribute value from training data


## Normalization on Attributes

**Z-Score Normalization**

Normalization brings all attributes/features to a common scale. The process of normalization is needed when the numeric attributes/features in a dataset doesn't belong to a common scale, e.g. age and height. The accuracy of classification/regression will be severely impact when performs directly on an unnormalized dataset with different kind of attributes.

There are many techniques to normalize a dataset, Z-score is used in this implementation. Z-Score is widely used in many machine learning algorithms, it expresses the normalized value in terms of how far the raw value is deviated from mean. Z-score and has range of [-3,3], 0 is the mean value of the normalized attribute, both 3 and -3 represents the boundary for z-score. 

**Equation for Z-score Normalization**

![z-score-normalization](https://wikimedia.org/api/rest_v1/media/math/render/svg/5ceed701c4042bb34618535c9a902ca1a937a351)

where:

* z : is the normalized value
* x : unnormalized attribute value
* μ : mean value of the attribute
* σ : standard deviation of the attribute

## Classification and Regression

**Classification**

The KNN classification takes the label which has the highest frequency occurrence in the nearest neighbors and return it as a prediction result.

**Regression**

The KNN regression returns the average of nearest neighbors and return it as a prediction result.


## Performance
* Tested the accuracy performance manually with the Iris dataset, the result is equivalent to Weka's KNN classifier.(Weka uses MSE as KNN loss-function)
* As for KNN regressor, Weka does not support KNN regression as of this writing but since the majority voting implementation is the same for both classifier and regressor, it should work fine.

## The Pros and Cons of KNN
**Advantages of KNN**

* **Easy to understand and optimize**

KNN is one of the most popular machine learning algorithm due to its simplicity and classification/regression effectiveness. The KNN model is easy to understand, the core idea is to employ majority voting mechanism to determine the best possible outcome. The base of KNN algorithm comprises of loss function, attribute normalization, sorting algorithms and majority voting mechanism. Given the interpertability of KNN, it is highly optimizable when performance is concerned.

* **No training phase required**

KNN is modelless. While performing classification/regression, KNN does not create any model at the training phase, which makes it highly adaptable to new data.

* **High adapbility for new data**

KNN is highly adaptable to new data. When new data are added to KNN classifier or regressor, no model rebuild or or training overhead will occur. Like most of the machine learning algorithms, the overhead of normalization will still occur in KNN, but the cost of normalization is trivia when compared to model rebuilding in neural network or any other deep learning approach.

**Disadvantages of KNN**

* **Performance scale poorly with data size**

As data size grows, the time-complexity performance of KNN drops. The convenience of modelless machine learning algorithm comes at a cost of run time performance. Since KNN checks every training instance against the given test instance for the best matches, therefore when data size grows, the computational overhead will inevitably increase as well.

* **Dillema of optimal K(the count of nearest neighbors)**

When K is below optimal point, the algorithm is susceptible to the noisy data. When K above optimal point, it raises the issue of overfitting. Both cases will contribute errors in classsification/regression.

## Future work

* **Runtime Performance**

The current implementation is single threaded, the performance can be largely improved using parallel processing. E.g. reading file concurrently(IO-bound), parallel iterative operation(CPU-bound).

* **Multi-label KNN**

Currently the KNN classifier supports only single label classification and does not support multilabel classification. 

* **Inverse Distance Weighted Average**

Inverse distance weighted average is a mean value calculated based on the degree of error. A neighbor with higher error will contribute lesser value to the aggregated mean. Inversely,  a neighbor with lesser error will contribute more value to the aggregated mean. 

The current implementation takes the basic average from the nearest neighbors and is susceptible to outliers effect. IDWA is introduced to resolve the issue.

## Feedback

There are many improvements can be made to this algorithm, please let me know if you find any issues, I'll be happy to improve them.
Thanks in advance.
tee@junhao.my
