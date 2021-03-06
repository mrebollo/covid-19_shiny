---
title: "Details"
author: "Michelle Kendall"
date: "04/03/2020"
output: html_document
---

<!-- NB you need to perform
     library(knitr)
     knit(details.Rmd)
     to create the md file
     to be used in the shiny app -->



# Details

For full details of our work please see our <a href="https://045.medsci.ox.ac.uk/" target="_blank">website</a> and our <a href="https://science.sciencemag.org/lookup/doi/10.1126/science.abb6936" target="_blank">paper in *Science*</a>.

This app provides a convenient way to test the sensitivity of our results to our assumptions.
The "reset values" button in the top right allows you to return to the original values presented in our paper.

Here we provide further explanation of the results and a description of each of the input parameters.

### Infectiousness

In the **Infectiousness** tab we plot $\beta(\tau)$ which is the mean rate at which individuals infect others at a time $\tau$ after they themselves were infected. It is defined by:

$$
\beta(\tau) = \underbrace{P_a x_a \beta_{s}(\tau)}_\text{asymptomatic} + \underbrace{(1-P_a) (1 - s(\tau))\beta_s(\tau)}_\text{pre-symptomatic} + \underbrace{(1-P_a)s(\tau)\beta_{s}(\tau)}_\text{symptomatic} + \underbrace{\int_{l=0}^{\tau}\beta_{s}(\tau-l)E(l)dl}_\text{environmental}
$$

where 

* $P_a$ is the proportion of infected individuals who are asymptomatic

* $x_a$ is the relative infectiousness of asymptomatic compared to symptomatic individuals

* $\beta_s(\tau)$ is the infectiousness of an individual conditional on them having started displaying symptoms

* $s(\tau)$ is the fraction of infected individuals that have already shown symptoms before time $\tau$ post infection, defined by:

$$
s(\tau)=\frac{1- P_a}{1-P_a+P_ax_a}\int_0^\tau i(\tau') d\tau'
$$

where $i(\tau)$ is the incubation time distribution for individuals that will eventually become symptomatic

* $E(l)$ is the rate at which a contaminated environment infects new individuals with a time lag $l$ after having been contaminated.


The area under the curve $\beta(\tau)$ is the reproduction number $R_0$ (in the setting where all contacts are susceptible).

We see that $\beta(\tau)$ is composed of four contributions: 
* **symptomatic transmission $R_S$**: direct transmission from a symptomatic individual, through a contact that can be readily recalled by the recipient
* **pre-symptomatic transmission $R_P$**: direct transmission from an individual that occurs before the source individual experiences noticeable symptoms. (Note that this definition may be context specific, for example based on whether it is the source or the recipient who is asked whether the symptoms were noticeable.)
* **asymptomatic transmission $R_A$**: direct transmission from individuals who never experience noticeable symptoms
* **environmental transmission $R_E$**: transmission via contamination, and specifically in a way that would not typically be attributable to contact with the source in a contact survey

The area under the curve of each of these four contributions gives the mean total number of transmissions over one full infection via that route, and these are plotted in the four smaller figures.

We also report $\theta = 1 - \dfrac{R_p}{R_0}$, the proportion of transmissions which are not from direct contact with symptomatic individuals.

### Interventions

In the **Interventions** tab we present a heat map of the growth rate $r$ of the epidemic when the interventions of isolation and quarantining of contacts are applied with different success rates, either instantly or after a given delay.
The calculations for this follow the approach of <a href="https://doi.org/10.1073/pnas.0307506101" target="_blank">Fraser et al. 2004</a> and are detailed in our <a href="https://science.sciencemag.org/lookup/doi/10.1126/science.abb6936" target="_blank">Supplementary Information</a>.

From the time an individual becomes symptomatic, we model them being isolated with efficacy $\epsilon_I$, and contact-tracing and quarantining being performed with efficacy $\epsilon_T$.
These processes of isolation and quarantining of contacts can be modelled as happening instantaneously (delay $=0$) or with a specified delay of up to 72 hours using the slider bar.
For the chosen input parameters and for each combination of $\epsilon_I$ and $\epsilon_T$ we calculate the growth rate $r$ of the epidemic.
If $r$ is positive (red area of the heat map) then the epidemic is growing and if $r$ is negative (green area of the heat map) then the epidemic is declining. 
The black line higlights the values where $r=0$, the threshold for epidemic control.

By adjusting the assumptions of the model in the left sidebar we can see that even with some delays, there are combinations of interventions of isolation and quarantining of contacts with significantly less-than-perfect efficacy for which the epidemic is controllable.

### Input parameters

The user may vary the following input parameters.
Here we define them and briefly provide our justification for their starting values.

The **incubation period** is defined as the time between infection and onset of symptoms. 
It is estimated as the time between exposure and report of noticeable symptoms. 
We used the incubation period distribution calculated by <a href="https://www.medrxiv.org/content/10.1101/2020.02.02.20020016v1" target="_blank">Lauer et al</a>. 
The distribution is lognormal with mean 5.5 days, median 5.2 days and standard deviation 2.1 days.
The parameter "sdlog" is the standard deviation of the normal variable whose exponential is log-normally distributed.

The **generation time** is defined for source-recipient transmission pairs, as the time between the infection of the source and the infection of the recipient.
We directly estimated the generation time distribution from 40 source-recipient pairs, for whom publicly available information points to direct transmission, and time of onset of symptoms is known for both source and recipient. 
The distribution is best described by a Weibull distribution with mean and median equal to 5 days and standard deviation 1.9 days, which has *shape* 2.83 and *scale* 5.67.

The **epidemic doubling time** is the time taken for the epidemic to double in size during the early uncontrolled phase of expansion.
It is given by $\dfrac{\log_e(2)}{r}$, where $r$ is the exponential growth rate, and we estimate it to be 5 days.

We estimate the **relative infectiousness of asymptomatic individuals compared with symptomatic individuals** $x_a$ to be 0.1 based on observing few missing links in the Singapore outbreak to date.

We estimate the **fraction of infected individuals who are asymptomatic** $P_a$ to be 0.4 based on media reports from the Diamond Princess.

We estimate the **fraction of transmissions that are environmentally mediated** to be 0.1 based on the anecdotal observation that many infections can be traced to close contacts once detailed tracing is completed.

For our infectiousness analysis we estimate the **environmental contribution** using the rate $E(l)$ at which a contaminated environment infects new people after a time lag $l$. 
We suggest this to be a constant rate of $l=3$ but we have also included the option of modelling an exponential decay in environmental infectiousness, which may prove to be a more accurate model when more data is available.


