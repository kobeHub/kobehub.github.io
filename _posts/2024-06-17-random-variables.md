---
layout: post
title: Common used random variables 
date: 2024-06-17 11:00:00
description: probability, statistics, random variables, discrete and continuous.
tags: Math
categories: math, statistics
giscus_comments: true
---

In this post, I am going to summarize some common used random variables for future references.

## Bernoulli Distribution

A Bernoulli distribution is the simplest discrete probability distribution, representing a single trial that has only two possible outcomes: success (usually denoted by 1) or failure (usually denoted by 0). $ T_t $ has one parameter $ p $, which represents the probability of success.

$$
\begin{aligned}
    P(X=1)=p \\ 
    P(X=0)=1−p
\end{aligned}
$$

**Mean and Variance**

$$
\begin{aligned}
    E(X) = p \\
    Var(X) = p (1-p)
\end{aligned}
$$

## Binomial Distribution

A Binomial distribution represents the number of successes in a fixed number of independent Bernoulli trials, each with the same probability of success. The binomial distribution has parameters $(n, p)$ with $n$ as the number of trails and $p$ is the probability of success of one Bernoulli trail.

The PMF is:

$$
\begin{aligned}
X \sim \rm{Binomial}(n, p) \\
p(X = k) = \binom{n}{k}p^k(1-p)^{n-k}
\end{aligned}
$$

**Mean and Variance**

$$
    \begin{aligned}
        E(X) = np \\
        Var(X) = np (1-p)
    \end{aligned}
$$


## Poisson Distribution

Poisson is a discrete probability distribution to express the probability of a given number of events occurring in a fixed interval of time (i.e. the number of tasks arriving in queuing systems). These events occur with a known mean rate $(\lambda)$ and independently of the time since the last event. The Poisson distribution deals with the number of occurrences in a fixed period of time, and the exponential distribution deals with the time between occurrences of successive events as time flows by continuously. 

The probability mass function is:

$$
P(X = k) = \frac{\lambda^k e^{-\lambda}}{k!}
$$

The positive real number $\lambda$ is equal to the expected value, and the variance of $X$:


$$
\lambda = \rm{E}(X) = Var(X)
$$

The Poisson distribution can be seen as a limit of the binomial distribution under certain conditions. This is known as the **Poisson approximation to the binomial distribution**. 

The Poisson distribution is a good approximation to the binomial distribution when:

1. **$n$ is large** (the number of trials is large).
2. **$p$ is small** (the probability of success in each trial is small).
3. The product  $np=\lambda$ is moderate (the expected number of successes remains constant).

**Additivity**: if $X_i \sim \rm{Poisson(\lambda_i)}$, for $i = 1, 2, \dots, n$, and the $X_i$ is independent, then

$$
X_1 + X_2 + \dots + X_n \sim \rm{Poisson(\lambda_1 + \lambda_2 + \dots + \lambda_n)}
$$

### Poisson distribution as an approximation of Binomial distribution

As a rule of thumb, if $n \ge 100 $ and $np < 10$, the Poisson distribution (taking $\lambda = np$​​) can provide a very good approximation to the binomial distribution.

$$
    \lim_{n\rightarrow \infty} \rm{Binomial}(x) = \frac{e^{-\lambda} \lambda^x}{x!}
$$

One important limit is used to prove the approximation:

$$
    \lim_{n\rightarrow \infty} (1 + \frac{r}{n})^n = e^r \text{ or, } \\
    \lim_{h\rightarrow 0} (1 + rh)^{1/h} = e^r
$$

To better see the connection between these two distributions, consider the binomial probability of seeing $x$ successes in $n$ trials, with the aforementioned probability of success, $p$, as shown below.

$$
    P(x) = \binom{n}{x} p^x (1-p)^{n - x}
$$

Let denote the expected value of the binomial distribution as $\lambda = np$, which means $p = \frac{\lambda}{n}$. Here $\lambda$ is also the arriving rate of success event of the approximated Poisson distribution.

The probability mass function can be written as follows:

$$
    P(x) = \binom{n}{x} (\frac{\lambda}{n})^x (1- \frac{\lambda}{n})^{n - x}
$$

Using the standard formula for the combinations of $n$ things taken $x$ at a time and some simple properties of exponents, we can further expand things to

$$
    P(x) = \frac{n(n-1)\cdots (n-x+1)}{x!} \cdot \frac{\lambda^x}{n^x} (1- \frac{\lambda}{n})^{n - x}
$$

Notice that there are exactly x𝑥 factors in the numerator of the first fraction. Let us swap denominators between the first and second fractions, splitting the$ $n^x$ all of the factors of the first fraction's numerator.

$$
    P(x) = \frac{n}{n} \cdot \frac{n-1}{n} \cdots \frac{n-x+1}{n} \cdot \frac{\lambda^x}{x!} (1- \frac{\lambda}{n})^{n} \cdot (1- \frac{\lambda}{n})^{-x}
$$

Finally, applying the limit to $P(x)$ and keeping $x, \lambda$ Fiexed:

$$
    \begin{align}
    \lim_{n\rightarrow\infty} P(x) = \frac{\lambda^x}{x!} \lim_{n\rightarrow\infty} (\frac{n}{n} \cdot \frac{n-1}{n} \cdots \frac{n-x+1}{n}) \cdot (1- \frac{\lambda}{n})^{-x} \cdot \lim_{n\rightarrow\infty} (1- \frac{\lambda}{n})^{n} \\
    = \frac{e^{-\lambda} \lambda^x}{x!}
    \end{align}
$$


## Exponential Distribution

For the continuous version of exponential distribution, the probability density function (PDF):

$$
f(x; \lambda) =
\begin{cases}
    \lambda e^{-\lambda x} & \text{if } x \ge  0 \\
    0 & \text{if } x < 0 
  \end{cases}
$$

Here $\lambda$ > 0 is the parameter of the distribution, often called the *rate parameter*. The distribution is supported on the interval [0, $\infty$). If a [random variable](https://en.wikipedia.org/wiki/Random_variable) *X* has this distribution, we write $X \sim \rm{Exp}(\lambda)$. 

The cumulative distribution function:

$$
F(x; \lambda) =
\begin{cases}
    1 -  e^{-\lambda x} & \text{if } x \ge  0 \\
    0 & \text{if } x < 0 
  \end{cases}
$$

The CDF and PDF are not the actual probability. For continuous probability distribution, we only compute the probability of a given interval instead of one single point. Because the probability $P(a \le x \le b)$ is the integral of PDF values in the range of $[a, b]$ and the area under a single point is infinitesimally small. The CDF is the integral of PDF, we can simple compute the probability as:

$$
P(a \le x \le b) = F(b) - F(a)
$$

where $F(x)$ is the CDF of $x$. 

Property:

+ **Mean**: $\frac{1}{\lambda}$
+ **Variance**: $\frac{1}{\lambda^2}$​
+ **Memoryless**: The exponential distribution and the [geometric distribution](https://en.wikipedia.org/wiki/Geometric_distribution) are [the only memoryless probability distributions](https://en.wikipedia.org/wiki/Memorylessness).

----

## Poisson Process

Let $\lambda > 0$ be fixed. The counting process $\{N(t), t \in [0, \infty)\}$ is called a Poisson Process with **rate $\lambda$** if all the following conditions hold:

+ $N(0) = 0$;
+ $N(t)$​ has **independent increments**;
+ The number of arrivals in any interval of length $\tau > 0$ has $\rm{Poisson}(\lambda \tau)$ distribution.

Note that from the above definition, we conclude that in a Poisson process, the distribution of the number of arrivals in any interval **depends only on** the length of the interval, **and not on** the exact location of the interval on the real line. Therefore the *Poisson process has stationary increments*.

We denote $X_1$ as the interval of the first arrival, the $T_1$ is the time instant of first arrival. 


<img src="{{site.baseurl}}/assets/img/posts/2024.06.17/Poisson-interarrival.png" alt="Arrivals" style="zoom:50%;" />


If $N(t)$ is a Poisson process with rate $\lambda$, then the. interarrival times $X_1, X_2, \dots$ are independent and

$$
X_i \sim \rm{Exponential(\lambda)}
$$

$X$ is a memoryless random variable, that is

$$
P(X > x + a | X > a) = P(X > x) 
$$

Thinking of the Poisson process, the memoryless property of the interarrival times **is consistent with the independent increment property of the Poisson distribution**. In some sense, both are implying that the number of arrivals in non-overlapping intervals are independent. 

Since the arrival time can be derived by:

$$
\begin{align}
T_1 =X_1 \\
T_2 =X_1 + X_2 \\
T_3 =X_1 + X_2 + X_3
\end{align}
$$
More specifically, $T_n$ is the sum of $n$ independent $\rm{Exponential}(\lambda)$ random variables. Then $T_n \sim \rm{Gamma(n, \lambda)}$. Note that here $n \in \mathbb{N}$. The $\rm{Gamma(n, \lambda)}$​ is also called **Erlang** distribution, i.e, we can write

$$
T_n \sim \rm{Erlang}(n, \lambda) =  \rm{Gamma(n, \lambda)} 
$$

The PDF of $T_n$, for $n = 1, 2, 3, \dots$, is given by

$$
f_{T_n}(t) = \frac{\lambda^nt^{n-1} e^{-\lambda t}}{(n-1)!}
$$

and

$$
E(T_n) = \frac{n}{\lambda} \\
Var(T_n) = \frac{n}{\lambda^2}
$$


----



## Markov Chain

Given the past states $X_0, X_1, \dots, X_n-1$ and the current state $X_n$, a Markov chain is a stochastic process, where the conditional distribution of any future state $X_{n+1}$ is independent of the **past states** and depends solely on **current state**. The $P_{ij}$ represents the probability of the process transiting from state $i$ to $j$, and the one-step transition matrix is a 2D matrix containing all possible probability of states transition. The multiple step transition can be obtained by the **Chapman-Kolmogorov Equations**:

$$
P^{n+m}_{ij} = \sum_{k=0}^{\infty} P_{ik}^n P_{kj}^m
$$

Two states $i$ and $j$ that are accessible to each other are called to **communicate**. For any state, the process can never change since once entered is an **absorbing state**. Two states that communicate with each other are said to be in the same **class**, each state class is separate with another one. The Markov chain is **irreducible** if there is only one class (i.e., all states communicate with each other). 

### Limiting Probabilities

For an irreducible ergodic Markov (one class) chain $\lim_{n \rightarrow \infty} P^n_{ij}$ exists and is independent of $i$. It shows the limiting probability that the process will be in state $j$ at time $n$, which **equals to the long-run proportion of time that the process will be in state $j$**. 

### Mean Time Spent in Transient States

Considering a finite state Markov chain with states in range $T=\{1, 2, \dots, t\}$, the transition probabilities matrix is a $t \times t$ matrix $P_T$. We denote the identity matrix as $I$. For transient state $i, j$, let $s_{ij}$ denotes the expected **number of time periods** that the Markov chain is in state $j$, given it starts from state $i$. The matrix format for transient state times is $S$, we have:

$$
S = (I - P_T)^{-1}
$$

For each element, we can also compute the transition probability $f_{ij}$ from state $i$ to state $j$:

$$
f_{ij} = \frac{s_{ij} - \delta_{ij}}{s_{jj}}
$$

Where $\delta_{ij}$ is the corresponding element in identity matrix.
