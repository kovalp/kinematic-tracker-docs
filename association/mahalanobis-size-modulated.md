# Mahalanobis size-modulated

This is an attempt to learn from the successful IoU-based metrics.
Namely, we observe that the IoU metric makes good use of geometry 
completely disregarding the accuracy with which the states are 
currently known. Indeed, no covariance matrix enters the definition of 
IoU metrics. 

On other side, the Mahalanobis distance is much more adjustable,
easy to define and based in probability theory governing the whole 
tracking methodology. In order to acknowledge the importance of geometry
within the Mahalanobis-distance approach, we add a size-modulated
part to the association covariance. Instead of using the innovation
covariance, we suggest 

$$
\Sigma_{rt} = R_{r} + HP_{t}H^T + S_{rt},
$$

where the matrices $R$, $H$ and $P$ forming the usual innovation covariance,
while the matrix $S$ is a *diagonal* matrix scaling with square average sizes of 
the probed cuboids. For example, to track 2D boxes, the size-modulating matrix 
reads

$$
S_{rt} = \frac{1}{4}
\begin{pmatrix}
(s_{xr} + s_{xt})^2 & 0 & 0 & 0 \\
0 & (s_{yr} + s_{yt})^2 & 0 & 0 \\
0 & 0 & (s_{xr} + s_{xt})^2 & 0 \\
0 & 0 & 0 & (s_{yr} + s_{yt})^2
\end{pmatrix}.
$$

The size-modulated Mahalanobis metric can be used in 2D or 3D tracking.
It shows promising results, albeit it's difficult to quantify its 
advantages and disadvantages at this point.