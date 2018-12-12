# Spline Cheat Sheet 
## Two broad categories of splines 
The fundamental difference between the two approaches is that smoothing splines explicitly penalize roughness
and use the data points themselves as potential knots whereas regression splines place knots at
equidistant/equiquantile points.
* __Smoothing Splines__ 
  - Called 'smoothing' because they 'smooth' rather than interpolate the data. A smoothing spline has a **knot at each data point**, but introduces a penalty for lack of smoothness. If the penalty is zero you get a function that interpolates the data. If the penalty is infinite you get a straight line fitted by ordinary least squares.
  $$\sum _{i=1}^{n}\{Y_{i}-{\hat {f}}(x_{i})\}^{2}+\lambda \int {\hat {f}}''(x)^{2}\,dx$$
* __Regression Splines__ 
  - Data is fitted to a set of spline basis functions with a **reduced set of knots**, typically by least squares. No roughness penalty is used.
  -  This is a broader category of splines that includes *B-splines* and *P-splines*. 

## Smoothing Splines 
* __Thin Plate Splines__ 
  - Example 2-dimensional smooth: $$\displaystyle E_{\mathrm {tps} ,\mathrm {smooth} }(f)=\sum _{i=1}^{K}\|y_{i}-f(x_{i})\|^{2}+ \lambda \iint_{R^2}(f_{x_1^2}^2+2f_{x_1x_2}^2+f_{x_2^2}^2)dxdy,$$ where $K$ are the unique data points and $\lambda$ is a smoothing parameter. 

## Regression Splines 
* __Thin Plate Regression Splines__ 
  - These are low rank isotropic smoothers of any number of covariates. Low rank means that they have far fewer coefficients than there are data to smooth. They are reduced rank versions of the thin plate splines and use the thin plate spline penalty 
  - Thin plate regression splines are constructed by starting with the basis and penalty for a full thin plate spline and then truncating this basis in an optimal manner, to obtain a low rank smoother.
  - In practice, you can sample a number of your unique $X_i$ and use those to create the smooth. 
  - [Link from mgcv package documentation discussing these points](https://stat.ethz.ch/R-manual/R-patched/library/mgcv/html/smooth.terms.html).
* __B-splines__ 
  - Uses bases to estimate the spline. The basis functions are local (each basis function is only non-zero over the intervals between $m + 3$ adjacent knots, where $m + 1$ is the order of the basis).
  - To define a $k$ parameter B-spline basis, first define $k + m + 2$ knots. An $(m + 1)^\text{th}$ order spline can be represented as a linear function of the basis functions: $$f(x) = \sum_{i=1}^{k}B_{i}^{m} (x)\beta_i$$
* __P-splines__  
  - *Penalized B-spline* (**P-spline = B-spline + Penalty**)
  - Based on B-splines, but include a *difference penalty* applied directly to the parameters to control function wiggliness. 
  - Generally apply the $m$ order difference (`mgcv` default is order 2):
  $$\mathcal{P} = \sum_{i=1}^{k-1} (\beta_{i+1} - \beta_i)^m$$
  - Can also apply a derivative based penalty (**P-splines with derivative based penalties**). Let $m_2$ denote the order of differentiation required in the penalty:
  $$J = \int_{a}^{b} f^{[m_2]}(x)^2dx$$
* __Adaptive smoothing splines__ 
  - Based on P-splines 
  - Just use a weighted *difference penalty* from the regular P-splines. The weights vary smoothly with $x$ (use a B-spline basis expansion for the weights). 
  $$ \mathcal{P} = \sum_{i=1}^{k-1} \omega_i(\beta_{i-1} - 2\beta_i + \beta_{i+1})^m $$
  