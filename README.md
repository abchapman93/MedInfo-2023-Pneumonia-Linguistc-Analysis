# A linguistic analysis examining the impact of COVID-19 on pneumonia diagnosis and disease models

_Authors_: Alec Chapman, Kelly Peterson, Elizabeth Rutter, McKenna Nevers, Jian Ying,
David Classen, Makoto Jones, Matthew Samore, and Barbara Jones

*Affiliations*: University of Utah School of Medicine and the US Department of Veterans Affairs Salt Lake City Healthcare System

## Background
The way clinicians think about and treat diseases such as pneumonia changes over time. These changes 
can occur due to a number of factors, including:
- The underlying pathology of the disease (e.g., what viruses are prevalent among the population)
- Systematic changes in the treatment of a disease (e.g., changes in clinical guidelines or quality initiatives)
- Improved biologic understanding of how a disease arises

The Covid-19 pandemic was a large systematic shock to the way clinicians treat and think about pneumonia. 
However, prior to the COVID-19 pandemic, the clinical conceptualization of pneumonia was already shifting 
away from concern for healthcare-associated pneumonia (“HCAP”) 
and toward recognition of heterogeneity of pathogens (bacterial vs. viral) and host response. 

Written clinical language embodies and reflects the clinician’s mental models of disease. 
Thus, clinical notes for pneumonia positive patients may provide clues as to the prevailing conceptual model 
of pneumonia. 

The objective of this analysis was to:
1) Examine temporal changes in pneumonia documentation that may reflect changes in pneumonia conceptual models; and 
2) Explore the extent to which these changes were driven by the Covid-19 pandemic or pre-existing trends

## Methods
### Data
We used a previously validated NLP system ([Chapman et al, 2023](https://academic.oup.com/jamiaopen/article/5/4/ooac114/6965695)) 
and ICD-10 codes to identify to identify 314k pneumonia-positive hospitalizations in the U.S. Department of Veterans Affairs 
between January 1st, 2016 and December 31st, 2021. For each hospitalization, we processed emergency department notes, radiology 
reports, and discharge summaries and extracted the following pneumonia-related concepts:
- **"Viral pneumonia"**
- **"Bacterial pneumonia"**
- **"Sepsis"**
- **"Healthcare-associated pneumonia"**

We aggregated to a hospitalization level by creating indicator variables indicating whether a concept was present 
in at least one note. 

### Temporal Model
Our objective was to study trends in documentation over calendar time, pre- and post-pandemic, and by Covid-19 status.
To do this, we modeled the probability that a hospitalization would have a note with a term using a segmented logistic regression model.

#### Model 1

For each term, let $Y_{i}$ be 1 if a hospitalization _i_ occurring at time $t_i$ contains that term. Then we can model
the probability of a hospitalization containing that term as:
$$logit(P[Y_{it} = 1]) = \beta_0 + \beta_1 t_i + \beta_2 \mathbf{F(t_i)} + \beta_3 A_{t_i} + \beta_4 (A_{t_i} \times t_i)  + \beta_5 (A_{t_i} \times F(t_i))$$

where:
- $t_i$ represents the number of days January 1st, 2016 and the hospitalization discharge 
- $\mathbf{F(t_i)}$ is a polynomial spline on the day of the year
- $A_{t_i}$ is an indicator variable for whether the hospitalization occurred before or after March 1st, 2020

Intuitively, $\beta_1$ captures an overall temporal trend in documentation between 2016 and 2021. We would expect this to capture non-pandemic-related changes in documentation patterns. $\beta_2$ should allow us to model possible seasonal trends. $\beta_3$ should capture the immediate shift in documentation caused by the pandemic, while the remaining coefficients should capture the interactions between the pandemic and time.

#### Model 2
We might also be interested in whether changes in term prevalence occur only in Covid-19 positive patients or whether it is a more general change observed in all pneumonia patients. To do this, will add additional terms to our model: $\beta_6 C_i + \beta_7(C_i \times t_i) + \beta_8 (C_i \times F(t_i))$, where $C_i$ is an indicator for whether the patient was Covid (+) during their hospitalization.

### Trend analysis
After fitting our models, we obtain the expected probability by predicting the probability of each term over time. To compare pre- and post-pandemic trends, we obtain a counterfactual prediction by setting $A_i$ and/or $C_i$ to 0 for each post-pandemic hospitalization. To compare general secular trends we can compare this counterfactual probability with the observed probabilities in 2016.
