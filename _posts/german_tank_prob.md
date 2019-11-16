---
layout: post
title: the German tank problem
tags: [statistics, probability, combinatorics, estimation]
snippet: The simplified German tank problem is as follows. During World War 2, the Germans labeled their tanks with sequential serial numbers. The Allied forces used the serial numbers on a captured subset of the German tanks (data!) to estimate the total number of tanks the Germans had.
author: Cory Simon
---

The German tank problem is intellectually stimulating and makes great conversation at wine bars. The problem has a historical context as well.

During World War 2, the Germans inscribed their tanks with sequential serial numbers $1,2,...,n$ when they were manufactured. The total number of tanks $n$ that the Germans had, however, was unknown to the Allied forces-- and of great interest.

The Allies captured a (assumed uniform random) subset of $k$ tanks from the German forces and observed their serial numbers, $x_1,x_2,...,x_k$. The German tank problem is to estimate the total number of tanks $n$ from the observed serial numbers $x_1,x_2,...,x_k$.


{:.centerr}
<figure>
    <img src="/images/german_tank_problem/sample.png" alt="image" style="width: 75%;">
    <figcaption>An outcome of a random sample (without replacement, of course) of German tanks, serial numbers inscribed. Based on this observation, what is your estimate for the total number of tanks the Germans have?</figcaption>
</figure>

The number of tanks is of course greater than or equal to the maximum serial number observed: $n \geq \max x_i$. Estimating that the Germans have fewer than 156 tanks would be outrageous. However, estimating that the Germans have 156 tanks would be too conservative; capturing the most recently manufactured tank is unlikely. So we should estimate the total number of tanks to be 156 *plus some number* to reflect the small likelihood that we captured the very last tank to come out of the factory. How large should this number be?

Let's delve into the math to answer this question, but feel free to skip to `# the unbiased estimator` if you want to know right away.

## the data generating process (dgp)

We are collecting a random sample from a population (the tanks) and making an observation about the members of the sample (the serial numbers). Think of tank capturing as a stochastic process governed by an underlying data generating process (dgp). The data here are the set of serial numbers $$\{x_1,x_2,...,x_k\}$$.

We assume the dgp is a random selection without replacement from the set of integers $$\{1,2,...,n\}$$. Because all viable outcomes are equally probable, the probability of any particular viable outcome is:

$$P(\{x_1, x_2, ..., x_k\} | n)=\dfrac{1}{\binom{n}{k}}$$

A viable outcome is one where $$x_i \in \{1, 2, ..., n\}$$ for all $$i\in \{1,2,...,k\}$$. So our dgp is governed by the probability distribution above, which is the uniform distribution. The denominator is how many ways we can select $k$ integers from $n$ without replacement, where the order in which they were selected does not matter.

## seeking an unbiased estimator

An *estimator* maps an outcome (the data, the serial numbers on the tank) $$\{x_1, x_2, ..., x_k\}$$ to an estimate of a parameter that characterizes the dgp (the total number of tanks, $n$). One estimator is the maximum observed serial number $m\equiv \max x_i$. It maps the observed set of serial numbers to an estimate for $n$.

We aim to derive an *unbiased* estimator, meaning that, if the dgp were to generate many data sets, and we used the estimator to estimate the total number of tanks $n$ for each of these outcomes, the average estimate of $n$ is equal to the true value in the dgp. The maximum serial number $m$ is a biased estimator because it is either equal to the true value of $n$ or an underestimate.

Let us base our unbiased estimator on the maximum serial number observed, $m$. We will find out what number to add to $m$ to arrive at our unbiased estimate of $n$. 


#### the likelihood $Pr(M=m|n)$

Let $M$ be the random variable that is the maximum serial number. Then, the probability that we see a certain max serial number (conditioned on $n$) is:

$$Pr(M=m|n) = \dfrac{\binom{m-1}{k-1}}{\binom{n}{k}}$$

since, as the schematic below illustrates, there are $m-1$ choose $k-1$ distinct sets of $k$ serial numbers where the max number is $m$. These outcomes are all independent events, so we can add up the probabilities of each event. This distribution petertains only to $m$ such that $n \geq m\geq k$ since the max serial number cannot be more than the total number of tanks or less than the number of captured tanks.

{:.centerr}
<figure>
    <img src="/images/german_tank_problem/show_formula.png" alt="image" style="width: 75%;">
    <figcaption>Let $m\equiv \max x_i$ be the maximum serial number observed, which is less than or equal to $n$. The number of ways that the maximum serial number can be $m$ is equal to the number of ways we can select $k-1$ tanks from the $m-1$ preceding tanks.</figcaption>
</figure>

#### a useful identity from the normalization of the probability mass function

Since $n \geq m \geq k$:

$$1= \sum_{m=k}^n Pr(M=m|n) = \sum_{m=k}^n \dfrac{\binom{m-1}{k-1}}{\binom{n}{k}}$$

i.e. the probability that $m$ is between $k$ and $m$ is unity. This gives us an identity that we will use later.

$$\binom{n}{k} = \sum_{m=k}^n \binom{m-1}{k-1}$$

In words, intuitively, the cardinality of the sample space for the German tank problem is equal to the number of outcomes where the max serial number is $k$ plus where it is $k+1$ and so on, up to $n$.

#### the expected value, $E(M=m\|n)$

Now let us find the expected value of $M$ over the outcomes of our dgp. 

$$E(M | n) = \displaystyle \sum_{m=k}^n m Pr(M=m|n)$$

We're conditioning on knowing $n$, which seems awkward because the whole point of the German tank problem is that we don't know $n$. Our strategy here is to find $$E(M \| n)$$, then find what $n$ gives an expected value of $M$ equal to the $m$ we oberved. This turns out to be an unbiased estimator for $n$. Using our earlier expression for $Pr(M=m\|n)$:

$$E(M | n) = \sum_{m=k}^n m \frac{\binom{m-1}{k-1}}{\binom{n}{k}}$$

The sum is over $m$, so we can pull the denominator outside of the sum. Also, we can write this as a sum over combinations by absorbing the $m$ into the factorial and multiplying by one in a fancy way:

$$E(M | n) = \dfrac{1}{\binom{n}{k}} \sum_{m=k}^n \frac{k}{k} m \frac{(m-1)!}{(k-1)!(m-k)!}$$

$$E(M | n) = \dfrac{k}{\binom{n}{k}} \sum_{m=k}^n \frac{m!}{k!(m-k)!}$$

$$E(M | n) = \dfrac{k}{\binom{n}{k}} \sum_{m=k}^n \binom{m}{k}$$

To apply our identity above, we use a change of variables in our sum. Define $\tilde{m}$ and $\tilde{k}$ such that $\tilde{m}-1=m$ and $\tilde{k}-1=k$. Then the sum over $\tilde{m}$ is from $\tilde{k}$ to $n+1$:

$$E(M | n) = \dfrac{k}{\binom{n}{k}} \sum_{\tilde{m}=\tilde{k}}^{n+1} \binom{\tilde{m}-1}{\tilde{k}-1}$$

We can now directly invoke our identity:

$$\sum_{\tilde{m}=\tilde{k}}^{n+1} \binom{\tilde{m}-1}{\tilde{k}-1} = \binom{n+1}{\tilde{k}}=\binom{n+1}{k+1}$$

and simplify the expression for $E(M \| n)$:

$$E(M | n) = \dfrac{k}{\binom{n}{k}} \binom{n+1}{k+1}$$

which, upon expanding the combinations into factorials and canceling terms:

$$E(M | n) = \dfrac{k}{k+1}(n+1)$$

## an unbiased estimator

We obtain an unbiased estimator for $n$, $\hat{n}$, by solving the expression above for $E(M \| n)$ for $n$ and replacing $E(M \| n)$ with our observation for $m=\max x_i$. That is, our estimate for the total number of German tanks is the $n$ such that the expected value of $M$ under the data generating process is equal to our observed $m$. We arrive at:

$$\hat{n} = m + (\frac{m}{k} - 1)$$

Indeed, the estimate of the total number of tanks is the maximum serial number observed plus some number to reflect how unlikely it is to select the most recently manufactured tank. This estimator for the total number of tanks is unbiased, as we will show with computer simulations below. One can also show that it is efficient, meaning that it exhibits the smallest variance among all other unbiased estimators. 

This estimator $\hat{n}$ has a very intuitive interpretation. Imagine placing all of the tanks in a line, from 1 to $n$. Then imagine if the tanks we captured were evenly spaced, i.e. had equal gaps of  The number that we add to $m$ to arrive at the estimate, $m/k-1$, is the gap between the tanks that would be present if the samples were evenly spaced.

{:.centerr}
<figure>
    <img src="/images/german_tank_problem/seven_tanks.png" alt="image" style="width: 75%;">
    <figcaption>If the $k$ captured tanks are evenly spaced among the $m$ total tanks, then the gap between them is $m/k-1$. Consider $n=7$ and $k=3$. </figcaption>
</figure>

## simulating the German tank problem in Julia
