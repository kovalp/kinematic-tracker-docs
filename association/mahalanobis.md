# Mahalanobis association metric

The metric is defined via gaussian function of the Mahalanobis distance
$D$

$$
A_{rt} = \exp(-f D_{rt}^2 / (2 N_z)),
$$

where the pre-factor $f$ is merely a tuning parameter with default value $1$,
$N_z$ is the number of variables in the detection vectors.
The (square) of Mahalanobis distance makes use a covariance matrix $\Sigma$

$$
D^2_{rt} = (\mathbf{z}_r - H\tilde{\mathbf{x}}_t) \Sigma^{-1} (\mathbf{z}_r - H\tilde{\mathbf{x}}_t),
$$
where $H$ is so-called measurement matrix used in Kalman filters.
The choice of covariance matrix $\Sigma$ makes possible to fine-tune the 
association eventually capturing the correlations between variables 
in the measurement space. A popular choice of this covariance is so-called 
*innovation covariance* defined as sum of the measurement covariance $R$ and
process covariance $P$ in measurement space $HPH^T$

$$
\Sigma_rt = R + H P_t H^T.
$$

The matrices $H$, $P_t$ and $R$ are defined ($H$ and $R$) and
calculated ($P_t$) in Kalman filters. $^T$ denotes the matrix transpose.
Because the measurement covariance is set the same for all measurement vectors,
we drop the report index from the measurement covariance $R$.  

The Mahalanobis metric has a number of advantages:

  - Simple matrix algebra operations involved.
  - Definition generalizes straightforwardly to any composition of the state vectors.
  - Allows a dynamical adjustment of the values relaxing the association condition at the start of tracking.

A number of disadvantages to mention here:

  - The result association quality depends on the choice of the Kalman-filter 
  covariances which affect both the predictions and association likelihoods simultaneously.
  - Involves solution of a batch of linear equations which affects the performance.
