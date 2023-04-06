# D.E. Shaw Case Study
---

## 1.	Join candidate_profile and recruiting_activity datasets, keeping each row in the recruiting_activity data.

The following table below shows a sample of rows from the concatenated table.

|      |   cand_id | position                       | dept        | furthest_stage   | app_date            | app_source       |   comp_exp |   yrs_exp | industry   | degree_1   | school_region_1   | degree_2   | school_region_2   | degree_3   | school_region_3   | Highest Degree   |
|-----:|----------:|:-------------------------------|:------------|:-----------------|:--------------------|:-----------------|-----------:|----------:|:-----------|:-----------|:------------------|:-----------|:------------------|:-----------|:------------------|:-----------------|
|  820 |      2468 | Associate Software Developer   | Engineering | Offer Sent       | 2018-11-01 00:00:00 | Campus Job Board |      85100 |  36.2623  | Government | PhD        | region_5          | Masters    | region_4          | Bachelors  | region_2          | PhD              |
| 4002 |      2471 | Associate Relationship Manager | Sales       | Offer Sent       | 2018-12-13 00:00:00 | Campus Event     |      74700 |   8.37313 | Other      | Masters    | region_2          | Bachelors  | region_5          | nan        | nan               | Masters          |
| 1181 |      2475 | Associate Software Developer   | Engineering | Offer Sent       | 2018-12-21 00:00:00 | Advertisement    |      70900 |  30.4426  | Government | Masters    | region_5          | Bachelors  | region_3          | nan        | nan               | Masters          |
| 2917 |      2480 | Associate Relationship Manager | Sales       | Offer Sent       | 2018-12-25 00:00:00 | Campus Event     |      93000 |  16.5672  | Government | Masters    | region_2          | Bachelors  | region_3          | nan        | nan               | Masters          |
| 3290 |      2486 | Associate Relationship Manager | Sales       | Offer Sent       | 2018-12-06 00:00:00 | Campus Event     |      82500 |  11.8657  | Other      | Masters    | region_1          | Bachelors  | region_5          | nan        | nan               | Masters          |
---
## 2.	Create a "Recruiting Funnel" view (as shown in the Legend tab) for Highest Degree. 

| furthest_stage     |   Bachelors Applicants |   Bachelors Conversion Rate |   Masters Applicants |   Masters Conversion Rate |   PhD Applicants |   PhD Conversion Rate |
|:-------------------|-----------------------:|----------------------------:|---------------------:|--------------------------:|-----------------:|----------------------:|
| New Application    |                   2838 |                    --        |                 1218 |                  --        |              910 |              --       |
| Phone Screen       |                    656 |                    23% |                  376 |                  31% |              556 |              61% |
| In-House Interview |                    314 |                    48% |                  202 |                  54% |              292 |              53%  |
| Offer Sent         |                     64 |                    20% |                   30 |                  15% |               30 |              10%  |
---
## 3.	Our Head of Recruiting, Cameron, has asked some questions about compensation expectations, as outlined below. When answering these questions, please focus only on the data available in our simulated dataset and write the responses as if you were writing to Cameron, who runs a large department and does not have a quantitative background. 

1.	**Do applicants with a PhD expect to make more than candidates who don’t have a PhD? Do the compensation expectations for PhD/non-PhD candidates differ across departments?**

    At a high level, it appears that PhD candidates expect to make more than those who don't have a PhD. However, that is largely a result of PhD candidates applying to higher level roles in the first place, skewing their averages. As we do more of an apples to apples comparison (comparing across department and position), we observe no evidence that PhD candidates expect to make more than non-PhD candidates.

2.	**In addition to the PhD and department, please check to see if there is a relationship between compensation expectations and years of experience and/or current industry.**

    We observe that on average, candidates with more years of experience expect a higher compensation. More specifically, for every year of experience, we estimate the expected compensation to be higher by ~3%. This means that an employee with 10 years of experience will have an expected salary 30% higher than someone with no experience. Note that the exact rate varies by department - Engineering candidates value their experience at the highest rate (4%), and IT at the lowest (2%.)

    Another factor that impacts the expected salary is the role the candidate is applying for. Some of the roles appear to be much higher level, and hence command a premium in salary expectations. Note that there is no evidence of industry having any impact on expected salary.

3.	**A colleague on the analytics team asked about your deduplication and analysis methods for a, b, and c above and why you chose that approach. Please draft a few bullets to explain**.

* **Deduplication**: There are two types of duplicates present in the data. The first type is a candidate who applied for more than one role within the same department, and the second one is a candidate who applied for more than 1 roles across distinct departments
* **Deduplication**: The second type of duplication is not a problem, as one candidate can be equally qualified for two different roles across different departments as they can be quite different from one another
* **Deplucation**: However, the first type could be problematic. Roles within the same department are quite different - the seniority appears to differ a lot. It would not make sense for one employee to qualify for two different levels of seniority
* **Deplucation**: In that case, their salary expectations might not be the most accurate and may be muddied by the perception of the other role. So for that reason, we have decided to only retain the role for which the candidate progressed the farthest into, as the best indication of qualification for that role, and hence proper ability to estimate compensation

* **Analysis**: The analysis that I used throughout was a multiple linear regression model, ran across departments
* **Analysis**: Most models had very good performance suggested by their R^2, especially after we applied a log transformation
* **Analysis**: We then infered the impact of various factors by looking at their coefficients and the significance associated with them
* **Analysis**: We chose this model due to its simplicity, high interpretability, and ability to identify the isolated impact of various factors
---
## 4.	The COO suspects that engineering and sales roles may not be priced properly and has asked for thoughts from the analytics team. Please draft an email to your analytics team manager that outlines the analysis you propose to answer this question. The analysis plan should include: 
1.	How you would determine whether the compensation we’d offer a candidate is appropriate for these roles.
2.	What internal/external data you might collect or analyze to answer this question.
3.	The most important points for the COO to know. 
4.	Any other information you think could be relevant to the COO’s question. 

---
Hi COO-Name,

I am writing regarding the idea that you expressed about engineering and sales roles being inappropriately priced. My team and I have been analyzing this issue and have two possible solutions we would like to discuss.

The first idea revolves around using a 3rd party benchmark to properly identify how we are pricing these roles, relative to market expectations. In my experience, these benchmarks can be informative and bring to the table otherwise inaccessible data. This would require the least amount of work from our end and would be quicker but would be more costly.

The second idea revolves around building a predictive model to help us predict pay for a given candidate and role. This would require a dataset that comprises of fair compensation expectations, as this is the data our model would learn from. As such, building this model would require more time as we need to carefully examine our data. We could also feed in additional data, such as geographical information, internal compensation benchmarks for the role and more descriptive role information such as expected span of control and such.

This model, if built properly, could help us implement a sustainable and practical process for making pay decisions for new hires, while also ensuring fairness and consistency, as it would not be biased against external factors like gender and ethnicity. It would be more cost-effective than the other approach but would not be able to provide market benchmarks.

I will schedule a call for us to go over both options and present a set of possible timelines and associated costs. We can then evaluate them both properly and see which direction you would rather go in.

Thanks,

Steven
