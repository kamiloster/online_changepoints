# online_changepoints
A Python code to determine online change points based on maximum log-likelihood

Selection of parameters for smoothing can be crucial for automation of the approach. The following methods for smoothing can be distinguished:
* Naive: all forecasts for the future are equal to the last observed value of the series: $\hat{y}_{T+h|T} = y_T$
* Average method: all future forecasts are equal to a simple average of the observed data: $\hat{y}_{T+h|T} = \frac{1}{T} \sum ^T _{t=1} y_t$, for h = 1, 2, ... . THe average method assumes that all observations are of equal importance, and gives them equal weights when generating forecasts.

It may be sensible to attach larger weights to more recent observations than to observations from the distant past. This is the concept of simple exponential smoothing. Forecasts are calculated using weighted averages, where the weights decrease exponentially as observations come from further in the past - the smallest weights are associated with the oldest observations:
$\hat{y}_{T+h|T} = \alpha y_t + \alpha (1 - \alpha) y_{T-1} + \alpha (1 - \alpha)^2 y_{T-2} + ...$, where $0 \leq \alpha \leq 1$, is the smoothing parameter. 

Other methods of smoothing include:
* double exponential
* triple exponential
* moving average
* Holt-Winters

The above methods are really good for general approach to smoothing. However, there are a few facts/assumptions in our system:
* time series do not contain trends or seasonalities - only flat lines are considered stationary conditions
* any change points are an effect of new stationary condition, with flat line, until the next change point
* variance is an effect of noise in the signal, and any deviations are caused by either change point or device malfunction

I will first look at the first 10 000. Ideally, we want to have online changepoints detection. Meaning: as we gather more data points, the algorithm will decide whether the new point is still in the stationary condition or we have a change point and new stationary condition becomes apparent.

Time series: t = 1, 2, ..., N
Probability distribution: $\theta$ ($\mu$, $\sigma ^2$), $\mu$ is mean, $\sigma$ is standard deviation, thus $\sigma ^2$ is variance of the distribution. Assumption: our data are normal distributed.
Time of the change point: t = $\tau$.

Two hypotheses:

Null hypothesis (no change point) $H_0: \theta_1 = \theta_2 = ... = \theta_{N-1} = \theta_N$

Alternative hypothesis (change point) $H_1: \theta_1 = \theta_2 = ... = \theta_{\tau-1} = \theta_\tau \neq \theta_{\tau+1} = \theta_{\tau+2} = ... = \theta_{N-1} = \theta_N$

Likelihood: probability of observing the data that we have for $H_0$. A measure of how good the hypothesis is: higher likelihood - stronger $H_0$ hypothesis.

Likelihood for $H_0$:
$\mathcal{L}(H_0) = p(x|H_0) = \prod_{i=t_0}^{N} p(x_i|\theta_0) $
