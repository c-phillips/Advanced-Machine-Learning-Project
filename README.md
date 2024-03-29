# Constrained K-Means Clustering with Background Knowledge

## Background

Wagstaff, Cardie, Rogers, and Schroedl published a paper in the Eighteenth International Conference on Machine Learning, 2001, in which they proposed an adaptation to the standard k-means clustering algorithm.  This was written after a previous paper by Wagstaff and Cardie where they take a version of the COBWEB clustering algorithm and apply instance-level constraints in a similar manner. This was used for most of their baselines in their Constrained K-means alrogithm. The k-means algorithm was first named by MacQueen in 1967 where he introduced several proofs and lemmas relating to the algorithm. The algorithm has become one of the more common and simple clustering algorithms.

![algorithm](figures/copkmeans_algo.png)

This algorithm uses background information about pairs of data points that must be in the same cluster, or in different clusters. The addition of this information allows a greater clustering accuracy. Instead of just putting a point in the nearest cluster, it puts it in the nearest valid cluster. If there is a must-link pair that a point is a member of, it will be placed in the same cluster as the other member of the must-link pair. If a point is a member of a can't-link pair, it will not be placed in the same cluster as the other member of the can't-link pair, even if it is the closest cluster. 

![link_example](figures/link_violation_example.png)

In the paper, constrained k-means is applied to five different UCI datasets: soybean, mushroom, tic-tac-toe, iris, and wine. The graphs generated show the overall accuracy as well as the accuracy on a hold-out portion of the dataset as a function of the number of constraints used in the clustering. The constraints are generated by randomly picking data points from the dataset and checking their labels to determine if they should be in the same cluster or not. They ran 100 trials where each trial was a 10-fold cross validation test and averaged to generate the test. Ultimately, the authors found that adding the constraints not only significantly improved clustering accuracy on the overall set, but also increased the model accuracy on the hold-out set above the performance of k-means.

## Methodology

### Hardware and software environment
These tests were all run with Python 3.7 on a 4-core desktop machine.

### Measuring and training
Similar to the original work, we implement a 10-fold crossvalidation scheme for training and validation purposes. The clusters are trained on 9 of the folds and the 10th is held out as a validation set. The overall score measures the performance of all 10 folds together. 

In the original work, the authors also perform several "trials" for each set of constraints. We do the same here, so the data in our results represents not only the mean scores for a crossvalidation set, but also the mean of a set of "trials". These trials are meant to smooth out the variance caused by randomly generated constraints.

### Quantifying performance
The result of the clustering algorithm is returned when a test is called post-training. The result is found using the Rand Index:

![rand_index](figures/rand_inx.png)

Which compares each point in the dataset to each other point, only adding 1 if their assignments agree with the labels. This comparison score is divided by the best possible score to scale the measure between 0 and 1.

When gathering results we use both the linear (1) and geometric (2) means of our accuracy measure.

![mean_eqs](http://www.sciweavers.org/tex2img.php?eq=%281%29%5Cquad%5Cmu%20%26%3D%20%5Cfrac%7B1%7D%7Bn%7D%5Csum_%7Bi%3D1%7D%5E%7Bn%7Df%28x_i%2Cy_i%29%5C%5C%0A%282%29%5Cquad%5Cmu%20%26%3D%20%5Csqrt%5B%5Cleftroot%7B-1%7D%5Cuproot%7B1%7Dn%5D%7B%5Cprod_%7Bi%3D1%7D%5E%7Bn%7Df%28x_i%2Cy_i%29%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0)

The original paper uses the linear mean, so we have included it for direct comparison; however, a geometric mean will heavily penalize the final score when there is even one particularly poor result. This means that to achieve a high final rating, every test must return a good result.

## Results
### Soybean:
#### Our results
![Soybean Small 100 Trials](figures/soybean_small_100trials.png)

#### Original Soybean results
![Soybean Small 100 Trials Original](figures/soybean_original.png)

The soybean dataset is small with only 47 samples and 35 attributes with 4 class values. The trendline in our results agree with the trend in the original paper. However, through our testing we did not see the same amount of improvment from basic K-Means (0-constraints).

### Mushroom:
#### Our results
![Mushroom 10 Trials](figures/mushroom_10_trials.png)

#### Original Mushroom results
![Mushroom Original](figures/mushroom_original.PNG)

The mushroom dataset is considerably larger than the soybean set with 8k+ samples and 22 features. One of the features is ill conditioned (due to several datapoints missing information) so the original authors removed that feature from their analysis and we followed suit. Additionally, to decrease the computation time, the authors use only 50 samples/trial, and the same was done here. The results do not correlate nearly as well as the soybean dataset. However, the held-out is still in a similar range and exhibits a similar behavior as the original.

### Tic Tac Toe:
#### Our results
![Tic Tac Toe 100 Trials](figures/tictactoe_100_trials.png)

#### Original Tic Tac Toe results
![Tic Tac Toe Original](figures/tictactoe_original.PNG)

The Tic Tac Toe dataset is also larger than soybean with nearly 1k samples. The dataset measures the possible set of board configurations for a game where X makes the first move. To simplify analysis we again follow the lead of the original authors and use only 100 samples/trial. Similar to the mushroom dataset, the held-out behavior is similar to that of the original, but the overall accuracy does not increase like it does in the original.

## Discussion

Our results do not align well in general with those from the original paper. Several differences between our approach and theirs could explain some of the discrepancies:
1. Sample selection:   
The original paper does not specify exactly how their limited dataset was sampled. We saw clearer trend agreement in the soybean dataset, which does not downsample from the data. 

2. Cross validation procedure:   
There could be some minor difference in implementation; however, we believe our cross validation procedure is correct and most likely not the cause of these discrepancies.

3. Overall data selection:   
Which data was used for the overall test score could change the result. We believe we have this procedure correct in that we used all folds from a 10-fold cross validation as the "overall" dataset.

4. Definition of distance metric:   
The distance metric used to identify the nearest cluster for a given point could have been very different in implementation. In particular: since all of these datasets are symbolically categorized in each feature the definition of distance can become ambiguous.

5. Dataset processing:   
On a similar note, how the data is processed could affect the way distance and clusters are calculated. Whether or not their data was processed symbolically or numerically could affect how the rest of the program proceeds. 

6. Constraint Generation:   
The generation of constraints could have a major effect on the algorithm's performance; however, we believe our random generation is inline with the original author's methods. 

One point of interest is that our performance seems to align approximately on level with the hold-out sets from the original paper. This could be due to a different definition for the "held-out" set, or some other unknown cause. 

## References

[Constrained K-Means Clustering with Background Information (Wagstaff, Cardie, Rogers and Schroedl)](https://pdfs.semanticscholar.org/0bac/ca0993a3f51649a6bb8dbb093fc8d8481ad4.pdf)
 
[Some Methods for Classification and Analysis of Multivariate Observations (MacQueen)](https://projecteuclid.org/download/pdf_1/euclid.bsmsp/1200512992)
 
[Clustering with Instance-Level Constraints (Wagstaff, Cardie)](https://pdfs.semanticscholar.org/17ad/99229140cf72e4495412c44c73d52cc9d913.pdf)
 