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
To do this, we modeled the probability that a hospitalization would have a note with a term using a logistic regression model:

For each term, let $Y_{it}$ be 1 if a hospitalization _i_ occurring at time _t_ contains that term. Then we can model
the probability of a hospitalization containing that term as:
$$logit(P[Y_{it} = 1]) = \beta_0 + \beta_1 t_i + $$