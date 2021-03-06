---
title: "Mixed models"
author: Jonathan Dushoff and Ben Bolker
---

## Resources

* [classic ANOVA approach](Mixed_models_Classic_approach.html) to mixed models.

* [Bolker chapter from Fox et al.](https://github.com/mac-theobio/QMEE_2017/blob/master/papers/14-Fox-Chap13_ed.pdf) (on private repo, you need to be signed in)


## Intro

Mixed models are models which combine "random" and "fixed" effects.

-   Fixed effects are effects whose parameters we wish to estimate
    -   The slope of a phosphorous response
    -   The difference in shield thickness between males and females

-   Random effects are effects that we wish to account for without
    estimating individually
    -   Neuron identity
    -   Village of residence

## Modern and classic approaches

"Modern" approach to mixed models involves explicitly estimating "random" parameters and their variances (see below).

Compared to the [classic ANOVA approach](Mixed_models_Classic_approach.html) to mixed models, the modern approach is:

* conceptually clear
* flexible
* powerful
* computationally difficult

## Example

Testing the effects of acid rain on spruce tree growth.

We have a complex manipulation (what kind of air the trees are exposed
to). As a result:

-   We have a small number of trees
-   We take a lot of samples from each tree
-   "Treatment" is a fixed effect
    -   We want to estimate the difference in growth with clean air and
        dirty air
-   "Tree" is a random effect
    -   We want to know what about the *distribution* of tree effects
        (so we can control for it); we are not specifically interested
        in the difference in growth between tree 3 and tree 5

## Random effects

Random effects are typically based on unordered factors

-   the levels of the factor are conceptualized as random samples from a
    larger population
-   the effect of each level is therefore a random variable
-   the essential parameters we estimate are not the effect of each
    level, but the mean and variance of the *distribution*.

## Is this a random effect?

Treating something as a random effect means treating the levels as interchangeable from the point of view of your scientific hypothesis.

There is sometimes controversy about when it is appropriate to model a predictor using random effects. Here are some criteria ...

## philosophical questions

-   are the levels chosen from a larger population?
-   are the levels chosen randomly?
-   are the levels a non-exhaustive sample of possible levels?
-   do you want to be able to make predictions about new (unobserved) levels?
	 - or inferences that include them?
-   are you interested in the distribution of levels/variability among levels?
-   are you _uninterested_ in testing hypotheses about specific levels?

## Types of analysis

There are several terms of art that are often used as synonyms for mixed models

* repeated measures
	* individual identity would typically be associated with a random effect

* multilevel or hierarchical models
	* each identifiable level could have its own random effect: e.g., country, village, household

## inferential questions

The choice to make something a random effect often has
more effect on your inferences about *other* variables than on your
inferences about the focal variable

-   Inferring using a fixed effect means we are inferring across the
    group of levels we have measured (only)
-   Inferring using a random effect means we are inferring across a
    *population represented by* the group of levels we have measured

## Examples

We measure effects of an influenza vaccination experiment for several years

* Year as a fixed effect: did vaccination "help" on average over these years (with a particular set of flu strains, etc.
* Year as random effect: will vaccination "help" on average over a wider set of years (assuming observed years are representative)

Spruce trees

* May not have enough data to do a random effects analysis
* Our _statistical_ inference is then limited to the areas we studied
* Whether we can broaden our _scientific_ inference is a science question (not stats)

## Practical questions

- Modern methods
	* have you measured a sufficient number of levels to estimate a
    variance (>5, preferably >10)?
	* May need more for more complicated models

- Classic methods
	* Do you have a balanced design?

- Otherwise
	* Limit your inference
	* Try an even more advanced approach (usually Bayesian)

## Interactions

* Are only intercepts affected by random factors, or also responses?
	* e.g., spruce trees 

* Interactions are often conceptually appealing, but computationally challenging

## Fitting

Modern mixed-model packages (see below) can fit a wide variety of
models. You just need to specify which effects are random.

-   Works with unbalanced designs
-   Works with crossed random effects
-   May require a lot of data (and sometimes a lot of levels) for a reliable fit 
	* Depends on model complexity

## Too few levels

If you have something that should be a random-effect predictor, but you
don't have enough levels, you can't fit a mixed model

It's OK to treat your random effect as a fixed effect, as long as this
is properly reflected in your scientific conclusions.

-   The scope of your analysis covers only the sampled levels, not the
    population they were sampled from

## R-side effects

Some effects can be modeled effectively with MMMs (modern mixed models)
on the "residual (R) side".

Standard random effects

* Discrete groups: any relation is binary
	* You and I are in the same country, but not the same village

R-side effects

* Arbitrary variance-covariance structure (time, space, etc.)
	* I am very close to Ben, and sort of close to Morgan
* Can also allow for heteroscedasticity

## How it works

It's complicated!

## How it works

Typically based on *marginal likelihood*: probability of observing outcomes.

Balance (dispersion of RE around 0) with (dispersion of data conditional on RE)

**Shrinkage**: estimated values get "shrunk" toward the overall
mean, especially in small-sample/extreme units

## How do we do it?

Different for linear mixed models (LMMs: normally distributed response)
and generalized linear mixed models (GLMMs: binomial, Poisson, etc.)

-   LMMs: REML vs ML
    -   analogy: $n-1$ in estimation of variance
    -   analogy: paired $t$-test
    -   what do I do when I have more than 2 treatment levels per block?
-   GLMMs: PQL, Laplace, Gauss-Hermite
-   Bayesian approach: put a combined prior on the parameters

## Practical details in R

-   `aov + Error()` term: _classical_ designs
-   `lme` (`nlme` package): older, better documented, more stable, does
    R-side models, complex variance structures, gives denominator df/$p$
    values
-   `(g)lmer` (`lme4` package): newer, faster, does crossed random effects,
    GLMMs
-   many other special-purpose packages

[ examples](Mixed_models_examples.html)
