# Constrained K-Means Clustering with Background Knowledge

## Background

## Methodology

When gathering results we use both the linear (1) and geometric (2) means of our accuracy measure.

![mean_eqs](http://www.sciweavers.org/tex2img.php?eq=%281%29%5Cquad%5Cmu%20%26%3D%20%5Cfrac%7B1%7D%7Bn%7D%5Csum_%7Bi%3D1%7D%5E%7Bn%7Df%28x_i%2Cy_i%29%5C%5C%0A%282%29%5Cquad%5Cmu%20%26%3D%20%5Csqrt%5B%5Cleftroot%7B-1%7D%5Cuproot%7B1%7Dn%5D%7B%5Cprod_%7Bi%3D1%7D%5E%7Bn%7Df%28x_i%2Cy_i%29%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0)

The original paper uses the linear mean, so we have included it for direct comparison; however, a geometric mean will havily penalize the final score when there is even one particularly poor result. This means that to achieve a high final rating, every test must return a good result.

Similar to the original work, we implement a 10-fold crossvalidation scheme for training and validation purposes. The clusters are trained on 9 of the folds and the 10th is held out as a validation set. The overall score measures the performance of all 10 folds together. 

In the original work, the authors also perform several "trials" for each set of constraints. We do the same here, so the data in our results represents not only the mean scores for a crossvalidation set, but also the mean of a set of "trials".

## Results
### Soybean:
![Soybean Small 100 Trials](figures/soybean_small_100trials.png)
> Our results

![Soybean Small 100 Trials Original](figures/soybean_original.png)
> Original Soybean results

### Mushroom:
![Mushroom 10 Trials](figures/mushroom_10_trials.png)
> Our results

![Mushroom 100 Trials Original](figures/mushroom_original.png)
> Original Mushroom results

### Tic-Tac-Toe
![Tic-Tac-Toe 100 Trials](figures/tictactoe_100_trials.png)
> Our results

![Tic-Tac-Toe 100 Trials Original](figures/tictactoe_original.png)
> Original Tic-Tac-Toe results


The soybean dataset is small with only 47 samples and 35 attributes with 4 class values. The trendline in our results agree with the trend in the original paper. However, through our testing we did not see the same amount of improvment from basic K-Means (0-constraints).

## Discussion