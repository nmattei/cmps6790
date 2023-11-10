---
layout: page
title: Project 3
subtitle: Gapminder Data
published: true
---

# Project 3: GapMinder Data

* **Posted:** November 6, 2023
* **Last Revision:** November 6, 2023

For this project you will practice and experiment linear regression using data from [gapminder.org]("http://gapminder.org"). We recommend spending a little time looking at material there, it is quite an informative site.

We will use a subset of data provided by gapminder provided by [Jennifer Bryan](https://jennybryan.org/) described in its [github page](https://github.com/jennybc/gapminder)

Get the data from [Jennifer's Github here.](https://github.com/jennybc/gapminder/blob/master/data-raw/08_gap-every-five-years.tsv)

(A mirror of that data is available in the project repository, in the Project 3 directory.)

## Regression Analysis of Gapminder Data

If we run the following code you should see the data for this project.

```
import pandas as pd
data = pd.read_csv("gap.tsv", sep='\t')
data.head()
```

```
    country	 continent	year	lifeExp	pop	        gdpPercap
0	Afghanistan	Asia	1952	28.801	8425333	      779.445314
1	Afghanistan	Asia	1957	30.332	9240934	     820.853030
2	Afghanistan	Asia	1962	31.997	10267083	853.100710
3	Afghanistan	Asia	1967	34.020	11537966	836.197138
4	Afghanistan	Asia	1972	36.088	13079460	739.981106
```

For Part 1 you will explore how life expectancy has changed over 50 years across the world, and how economic measures like gross domestic product (GDP) are related to it. Note that for all questions you should have a cell in your notebook that answers that question along with the required plots.

**Exercise 1**: Make a scatter plot of life expectancy across time.

**Question 1**: Is there a general trend (e.g., increasing or decreasing) for life expectancy across time? Is this trend linear? (answering this qualitatively from the plot, you will do a statistical analysis of this question shortly)

A slightly different way of making the same plot is looking at the distribution of life expectancy across countries as it changes over time:

```
    fig, ax = plt.subplots()

    ax.violinplot(life_exp_per_year,years,widths=4,showmeans=True)
    ax.set_xlabel("Year")
    ax.set_ylabel("Life Expectancy")
    ax.set_title("Violin Plot Example")
    fig.savefig("violin.png")
```

<p style="text-align:center;"><img src="{{ 'img/violin.png' | relative_url }}" height="242" /></p>

This type of plot is called a <em>violin plot</em>, and it displays the distribution of the variable in the y-axis for each value of the variable in the x-axis. (It is okay to use other plotting libraries and tools to create this plot, and others throughout the assignment.)

Note that in order for the example code above, you will have to wrangle the data using Pandas. The way I did it was to create an array for each year, storing the Life Expectancy values for that year, then collected those arrays into a list: `life_exp_per_year`. It's not the only way of doing it, but it's how I first thought of it.

**Question 2**: How would you describe the distribution of life expectancy across countries for individual years? Is it skewed, or not? Unimodal or not? Symmetric around it’s center?

Based on the Violin plot you made, consider the following questions.

**Question 3**: Suppose I fit a linear regression model of life expectancy vs. year (treating it as a continuous variable), and test for a relationship between year and life expectancy, will you reject the null hypothesis of no relationship? (do this without fitting the model yet. I am testing your intuition. Just write out the answer.)

The next set of questions ask you to reason about the *residuals* of a model. You can learn more about [residuals](https://www.khanacademy.org/math/statistics-probability/describing-relationships-quantitative-data/regression-library/a/introduction-to-residuals) here from Khan Academy, but in a nutshell residuals are the errors in every prediction that you make. Specifically, NIST says that ``Residuals are estimates of experimental error obtained by subtracting the observed responses from the predicted responses.
The predicted response is calculated from the chosen model, after all the unknown model parameters have been estimated from the experimental data.'' You should have a read of the [NIST Residuals Article Here](https://www.itl.nist.gov/div898/handbook/pri/section2/pri24.htm).

Now we don't want to add these up, because they might cancel each other out, but looking at a plot of the residuals can help us understand *where* our model is making errors. In fact, there are some handy [SciKit Functions for this](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.PredictionErrorDisplay.html)!

**Question 4**: What would a violin plot of residuals from the linear model in Question 3 vs. year look like? (Again, don’t do the analysis yet, answer this intuitively).

**Question 5**: According to the assumptions of the linear regression model, what *should* that violin plot look like?  That is, consider the assumptions the linear regression model you used assumes (e.g., about noise, about input distributions, etc); do you think everything is okay?

**Exercise 2**: Fit a linear regression model using, e.g., the `LinearRegression` function from [Scikit-Learn](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html) or the closed-form solution, for life expectancy vs. year (as a continuous variable).  There is no need to plot anything here, but please print the fitted model out in a readable format.

**Question 6**: On average, by how much does life expectancy increase every year around the world? You should compute and print this value nicely.

**Question 7**: Do you reject the null hypothesis of no relationship between year and life expectancy? Why?

**Exercise 3**: Make a violin plot of residuals vs. year for the linear model from Exercise 2.

**Question 8**: Does the plot of Exercise 3 match your expectations (as you answered Question 4)?

**Exercise 4**: Make a boxplot (or violin plot) of model residuals vs. continent.

**Question 9**: Is there a dependence between model residual and continent? If so, what would that suggest when performing a regression analysis of life expectancy across time?

**Exercise 5**: As in the Moneyball project, make a scatter plot of life expectancy vs. year, grouped by continent, and add a regression line.  The result here can be given as either one scatter plot per continent, each with its own regression line, or a single plot with each continent's points plotted in a different color, and one regression line per continent's points.  The former is probably easier to code up.

### Bonus Questions

The next set of questions have to do with the concept of [interaction terms in linear regression](https://developer.nvidia.com/blog/a-comprehensive-guide-to-interaction-terms-in-linear-regression/). In short, interaction terms enable you to examine whether the relationship between the target and the independent variable changes depending on the value of another independent variable. Before attempting this section you'll want to read the above link very closely. To claim this bonus you need to very clearly label this section of your notebook bonus!

**Bonus Question 10 (2 Points)**: Based on this plot, should your regression model include an interaction term for continent *and* year? Why?

**Bonus Exercise 6 (2 Points)**: <em>Fit a linear regression model for life expectancy including a term for an interaction between continent and year.  Print out the model in a readable format, e.g., print the coefficients of the model (no need to plot).  Hint: adding interaction terms is a form of feature engineering, like we discussed in class (think about, e.g., using (a subset of) polynomial features here).</em>

**Bonus Question 11 (2 Points)**: <em>Are all parameters in the model significantly (in the p-value sense) different from zero? If not, which are not significantly different from zero? Other libraries (`statsmodels` or `patsy` may help you solve this problem)</em>

**Bonus Question 12 (2 Points)**: <em>On average, by how much does life expectancy increase each year for each continent? (Provide code to answer this question by extracting relevant estimates from model fit)</em>

**Bonus Exercise 7 (2 Points)**: <em>Make a residuals vs. year violin plot for the interaction model. Comment on how well it matches assumptions of the linear regression model.</em>

## Part 2: Classification and Model Selection

Find a dataset on which to try out different classification (or regression) algorithms.  If you'd like, you can use a dataset provided by SKLearn; [more info here](https://scikit-learn.org/stable/datasets/toy_dataset.html). Note that this might be a good time to try out some models from your final project, the world is your oyster here!

Choose *two* of the following algorithms, or feel free to use other ``standard'' supervised learning classifiers or regressors from [SKLearn](https://scikit-learn.org/stable/supervised_learning.html). 
* Linear Discriminant Analysis (LDA) (only classification)
* decision trees,
* random forests,
* linear SVM,
* non-linear SVM,
* k-NN classification (or regression).

(It will likely make sense to choose two classifiers or two regressors, not one of each, for most datasets and tasks.)

**Model Comparison / Discussion:** Compare the two chosen algorithms on their prediction performance, using your chosen dataset.  Let us know what your performance metric is (e.g., accuracy, false negative rate (FNR), false positive rate (FPR), precision, recall, etc), and feel free to use more than one.  Use either (i) holdout validation, like we discussed in class , or (ii) 10-fold cross-validation.  SKLearn offers some one-liners to help with cross-validation; [more info can be found](https://scikit-learn.org/stable/modules/cross_validation.html).

Note: for those algorithms that have hyper-parameters, i.e., all of the above except for LDA, you need to specify in your writeup which model selection procedure you used. Typically this should be with a [Grid Search](https://scikit-learn.org/stable/model_selection.html) over a small set of values.

**Bonus (3 Points)** If you ran 10-fold cross-validation, feel free to run a statistical test such as a paired t-test to further support your performance comparison above.

## Submission

Prepare a Jupyter notebook that includes for each Problem: (a) code to carry out the step discussed, (b) output showing the result of your code, and (c) a short prose description in *markdown* of how your code works. Remember, the writeup you are preparing is intended to communicate your data analysis effectively. Thoughtlessly showing large amounts of output in your writeup defeats that purpose and will lose points.

All axes in plots should be labeled in an informative manner. Your answers to any question that refers to a plot should include both (a) a text description of your plot, and (b) a sentence or two of interpretation as it relates to the question asked.

Submit this completed notebook which contains your answers as markdown cells to [Canvas](https://tulane.instructure.com/)

## Grading Rubric

Note that code that does not work will not be graded and you will receive a 0 for that section.  We reserve the right to deduct points for things like general sloppiness of the notebook, poor labels, unlabeled axes, etc.  You should include markdown cells to break up your notebook and **clearly label** the problems and questions below.

* (5 Points) *Professionalism*: You have used both code comments and markdown cells to professionally and clearly document your work including having a clear and clean notebook; linking to resources and documents; and doing so with code that is reasonable and efficient. 
 * (25 Points) Part 1: *Regression Analysis* 
   * (5 Points) Exercise 1 and Question 1: Code and comments are present to generate the violin plots and it is analyzed.
   * (5 Points) Questions 2 - 5: Questions 2 - 5 are discussed and clearly answered.
   * (5 Points) Exercise 2 and Questions 6 - 7: Code is present for the linear regression model and questions are clearly answered.
   * (5 Points) Exercise 3 - 4 and Questions 8 - 9: Code is present to create the two residual plots, explanations of questions are provided.
   * (5 Points) Exercise 5: A scatter plot is present showing the proper groupings.
* (20 Points) Part 2: *Classification Analysis*
  * (5 Points) Data and Model: Data is explained along with the target of prediction and why.
  * (5 Points) Model 1: Code is present for a first model that is validated using an algorithm.
  * (5 Points) Model 2: Code is present for a second model and relevant validation is done.
  * (5 Points) Discussion: A markdown cell is clearly labeled with information as to which model is better, along what metric, and why.

| Full    |    Good |  Okay |   Lacking   |   Poor  | No Marks |
| : ---- :| : ---- :| : ---- :| : ---- :| : ---- :| : ---- : |
| You completely and fully met the criteria described. | You have met most of the criteria but lack sufficient depth and/or missed one or more minor pieces. | You have missed components of the required criteria and/or they are incorrect / inappropriate or lack depth. | You have missed major components of the required criteria and/or may of them are incorrect. | Little to no evidence of work and major missing components. | Missing or completely inadequate. |

### Credits

Thanks to [John P. Dickerson](http://jpdickerson.com/) for the Project writeup.