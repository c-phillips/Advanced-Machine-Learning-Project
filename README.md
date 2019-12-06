# Constrained K-Means Clustering with Background Knowledge

## Background

## Methodology

When gathering results we use both the linear (1) and geometric (2) means of our accuracy measure.
$$
(1) \mu_l &= \frac{1}{n}\sum_{i=1}^n f(x_i,y_i)\\
&\phantom{s}\\
\phantom{s}\\
(2) \mu_g &= \sqrt[\leftroot{2}\uproot{2}n]{\prod_{i=1}^n f(x_i,y_i)}
$$

The original paper uses the linear mean, so we have included it for direct comparison; however, a geometric mean will havily penalize the final score when there is even one particularly poor result. This means that to achieve a high final rating, every test must return a good result.

## Results
### Soybean:
![Soybean Small 100 Trials](figures/soybean_small_100trials.png)
> Our results

![Soybean Small 100 Trials Original](figures/soybean_original.png)
> Original Soybean results

The soybean dataset is small with only 47 samples and 35 attributes with 4 class values. The trendline in our results agree with the trend in the original paper. However, through our testing we did not see the same amount of improvment from basic K-Means (0-constraints).

## Discussion