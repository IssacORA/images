---
title: "Problem Set 1"
output:
  html_document:
    df_print: paged
---

### CS 152-01 Problem Set 1                           Due Friday, September 23, 2022

#### 1. Working with data in R 

a. (2 points)  Use the `read.table()` command to read the tab-delimited data from the file `testgrades.txt` into a data frame in R.  This file contains the grades from three midterms and a final exam for a (fictional) class of 100 students; the first two columns contain the students' first and last names.  Show your code below:
```
table = read.table("~/CS152/testsgrade.txt",header = TRUE)
```

b. (3 points) Using basic R graphics, create a boxplot (which by default shows the median, inter-quartile range, and outliers) for each of the four tests.  Show the code and plot here:
```
grades =  table[,c(3,4,5,6)]
label = c("Midterm1","Midterm2","Midterm3","Final")
boxplot(grades,names = label)
```

![avatar](https://github.com/IssacORA/images/blob/main/CS152/hw1_q2.png)

c. (4 points) Look at the help page for `boxplot()` to answer the following: how far do the whiskers of each box extend, relative to the size of the box itself (using default parameters)?  How many outliers (i.e., students whose grade is more than that distance from the median) are there for the final exam?  Write your answers below as text in the R Markdown file.

    *
    1.
    2.From the plot, there is one outlier in Final exam*

d.  (4 points) Which two tests have medians that are farthest apart?  We haven't discussed this in class yet, but do you believe those differences show one exam to be truly harder than the other?  How might you tell?  

     *Midterm 3 and final exam
     No I cannot assert the relative difficulty of 2 exams because the grade of student whose grade is the median might be only significant difference between two distributions.  * 
    
e. (4 points) Next, suppose that the final grade for the course counts each midterm as 20\% of the grade and the final as 40\%.   Create a vector called `finalgrades` containing these weighted averages.  Note that the dot-product operator in R is `\%*\%`.  E.g., the R commands
```
x <- c(5,2,3,4)
y <- c(2,8,4,6)
dot <- x %*% y
``` 
end with `dot` containing the value 62 (that is, `5*2 + 2*8 + 3*4 + 4*6`).  

    To write better and more efficient R code, try to avoid using explicit loops.  Instead, think about how you can use the apply() function to solve this problem. 

    Write your code below. 

```
grades = table[,c(3,4,5,6)]
weight = c(.2,.2,.2,.4)
dot_with_w = function(array){
  return (weight %*% array)
}
finalgrades = apply(grades,1,dot_with_w) 
```


f. (4 points) What are the lowest, highest, average, and median final grades?  Are there many outliers or extremely low values?   

![avatar](https://github.com/IssacORA/images/blob/main/CS152/hw1_q_f.png)
```
> summary(finalgrades)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  72.60   87.20   90.90   90.43   93.90   99.20
```

    *From summary(finalgrades):
       lowest final grade = 72.60
       highesst final grade = 99.20
       median final grade = 90.90
       average final grade = 90.43
     From boxplot(finalgrades):
       there are 2 extremely low values*  

g.  (4 points) Using base R graphics, print a histogram of final grades, with bins of size 2.5. (You can use `seq()` to exactly define breakpoints between the bins.)

```
x_axe = seq(from = min(finalgrades),to = max(finalgrades)+1,by = 2.5)
print(x_axe)
hist(finalgrades,breaks = x_axe, xlim = c(70,100))
```
![avatar](https://github.com/IssacORA/images/blob/main/CS152/hw1_g.png)

h. (3 points) Create a new data frame, `allgradesdf`, that contains a copy of `studentsdf` with  `finalgrades` in the final column, sorted by the final course grade.  Print the names and scores of the top two students.

```{r}
```


#### 2. Why vectorize?   

a. (4 points) In the R box below, define a variable, `n`, to have the value 1,000,000.  Now define `a` and `b` to be two vectors of n normally distributed random numbers.  The first should have mean 1 and standard deviation 1; the second, mean 4 and standard deviation 2.  

```{r}
 
```

b. (4 points) Use vectorized addition (a+b) to create a new vector, `mysum`, that contains the element-wise sum of a and b.  Once you're sure this works,
wrap that code using the `system.time()` function to determine how long this takes to run.  Use `replicate` to run 100 iterations of this code timing your summation.  Store the elapsed times from all 100 runs in a vector called `mytimes`.
Look at the distribution and summary() statistics for the elapsed times.   

```{r}
 
```

c. (4 points) Now, using the same function to time your code, re-compute mysum using a `for` loop over the indices from 1 to n.  The values in mysum should be the same.  Use `replicate` to run 100 iterations of this code timing your summation.  Store the elapsed times from all 100 runs in a vector called `myfortimes`.  Look at the distribution and summary() statistics for the elapsed times.  

```{r}
 
```
 
d. (4 points) Plot a histogram of mysum.  Does the sum look like it too follows a normal distribution?

```{r}

```

    *Written answer goes here.* 

e. (5 points) Make a guess at the mean and standard deviation of a normal distribution that might fit the data in mysum.  Create a simulated vector, `simnormal`, of random instances from your hypothesized normal distribution, and plot the two histograms (the real mysum, and that of your simulated normal vector) one above the other.  

    (The command  `par(mfrow=c(2,1)` will create a plot layout with two rows and one column.   [If you get a complaint from RStudio about figure margins being too large, enlarge the plot window until this error goes away.])   

    You may need to try a few values of the parameters to match the distributions reasonably.   

```{r}

```

f.  (4 points) What is the apparent relationship between the means of mysum and the means of a and b?   What is the apparent relationship between the variance of mysum and the variance of a and b?  

    Hint: The variance is the square of the standard deviation, but you can also  compute the variance of a vector with the *var()* function.  

    *Written answer goes here.*   


#### 3. Functions and simulation

a. (3 points) Explain, in a sentence or two, what is "extreme" about extreme value analysis. Give an example of a case where you might want to use an extreme value analysis beyond the examples in the textbook or this problem.

    *Written answer goes here.* 

b.  (5 points) Write an R function named `extremelyNormal()` that uses simulation to estimate the probability that the maximum of n normally distributed random variables with mean 0 and standard deviation 1 is at least m.  The parameters to your function will be n, m, and t.  The function should conduct t trials, where each trial samples n random variables  from  the  normal  distribution, and should then use these trials to estimate the probability that the largest of the n values is at least m.  The function should return this probability.  

    Again, good R programming style avoids writing explicit loops in code, preferring vector operations or the "replicate" or "apply" functions.   Functions that use loops to iterate over the data instead of these alternatives will receive partial credit for this problem. 
    
    Include your code below.

```{r}
 
```


c. (2 points) Note:  Good programming style, regardless of language, dictates that we avoid hard-coding constant values throughout our programs.  One solution to this problem is to define constants, variables whose values do not change during  the  program.   E.g.,  you  could  start  a  function  with  the commands
```
norm_mean = 0.0
norm_stdev = 1.0
```
  However, in this case, you do not even need to do that.  Look at the manual page for the rnorm function and explain why we do not need to define constants to set the mean and standard devation to 0 and 1, respectively.

    *Written answer goes here.*

d. (4 points)  Use "replicate()" to run extremelyNormal() five times, where each run has 10 random variables, a threshold of m=2, and 10 trials. What range of values do you get in your five iterations? Do you think the probability estimates you get using this approach are accurate?

```{r}

```

    *Written answer goes here.*

e. (3 points)     Run it five more times with the same n and m values but with each replicate having 10000 trials. What is the range of values you see now? Why is the range different from the range in part d?



```{r}

```

    *Written answer goes here.*   


#### 4. The hypergeometric distribution 

Imagine that you have an urn containing 100 balls, 
10 of which are brown and 90 of which are blue 
(Tufts colors, yay!).  Now suppose that you pick a set of 8 balls at random from the urn, without putting any back in.   Think about how to compute the probability that you have at least 3 brown balls in your set of 8.

The part about not putting the balls back after you pick each one out is important.  Consider: the probability that the first ball will be brown is 10/100 or 0.1.  If you then put the first ball back before you choose again, the probability that the next ball is brown will also be 0.1.  So you would simply be performing 10 Bernoulli trials with a success probability of 0.1, and the resulting probability distribution could be modeled by the `binom` family of functions.  Been there, done that.

But if you keep the first ball, what is the probability that the second ball is brown?  Well, if the first one was brown, then it would be 9/99, but if the first one was blue, it would be 10/99.  What about the second one? And what if you picked the first two in the other order? Suddenly this doesn't look so Bernoulli-like!  

The answer to this question is addressed by the hypergeometric distribution, which models sampling "without replacement" from the urn.  R has the usual family of functions for this distribution, `dhyper`, `phyper`, `qhyper`, and `rhyper`.  

The distribution is often used to argue that an observed biological event is "surprising" - or that in fact it is not. Suppose that we have, instead of 100 balls, 100 genes that have been implicated in genetic association studies of schizophrenia. 
Of these 100 disease-associated genes, 10 of them relate to 5-HT serotonergic receptor signaling (a pathway that plays a role in many other psychiatric or neurological disorders) and the remaining 90 do not.   

Now, suppose that you sequenced these 100 genes in a patient with schizophrenia and observed mutations in only 8 of them.   Suppose three of those genes were in the 5-HT pathway, and the five other mutated genes were not.  Based on the results of the genetic testing you wonder whether the 5-HT pathway may be dysregulated in this particular patient. What is the probability that you would find mutations in at least three of the ten 5-HT pathway genes by chance? 

a. (3 points) First, read the help page for phyper and use that function to compute this probability explicitly in R.

```{r}
 
```

b. (5 points) 
Now, estimate the probability by simulation, using 10000 trials and the `rhyper()`  function in R. What is the probability of seeing at least 3 of the 5-HT serotonergic receptor signaling pathway genes in your simulation?  The probability of seeing at least 4?  The probability of seeing exactly 2?

    Show your code below:
```{r}
 
```

    *Answer the questions here.* 
 
c. (3 points) Compute the probability, by simulation, of seeing at least 3 of the serotonin receptor pathway genes in a random **binomial** distribution where the probability of success is 10/100 (the number of 5 HT serotonin receptor signaling pathway genes out of the total number of genes in our data set).  What is the probability of seeing at least 3 genes from this pathway in this simulation?

    Show your code below:

```{r}

```

d. (4 points) Do you think the binomial distribution is a good approximation to the hypergeometric distribution in this case?  Show your code for answering this below.   How do the two distributions differ?  Generally speaking, when might the binomial be a good approximation to the hypergeometric distribution?

```{r}

```

    *Answer the questions here*
 
#### 5. Modeling nucleotide frequencies with the multinomial distribution

In class, we discussed modeling nucleotide frequencies using the multinomial distribution. Suppose you're given the following probabilities for A, C, G, and T, respectively:

```
pvec=c(0.375, 0.25, 0.25, 0.125)
```

a. (2 points) Given the probabilities above, calculate the expected nucleotide counts in a sequence of length 10 (e.g. 4 As, 2 Cs, 3 Gs, 1 T) and store the values in a vector called "expected." In the previous example, printing "expected" would look like this:

    [1] 4 2 3 1

    Note that in your case, expected values will not be integers.

```{r}

```

b. (3 points) Use "rmultinom" to simulate nucleotide frequencies five times (n=5) for sequences of length 10 (size=10) and the probabilities in pvec, and store the results in a matrix called "observed".

    Look at the observed counts in your 5 simulations. Do they differ from your expectations? If so, how? If not, why not?

```{r}

```

    *Answer the questions here*

c. (4 points) The observed frequencies in your trials may not always be consistent with your expectations - but what happens as we repeat the simulation many times?

    To test this, we'll examine the *mean squared error* between observed and expected nucleotide frequencies in 100 simulations with varying numbers of trials (from n=1 to n=100), normalized by number of trials.

    First, write a function called "norm_mean_squared_error". It should take your "observed" and "expected" data, and return the mean squared error between them, normalized by the number of trials in your simulation, *n*.   Normalized mean squared error should be calculated as follows: $$\frac{(x_1-\hat{x_1})^2 + ... + (x_n - \hat{x_n})^2}{n^2}$$ where $$x_i$$ are the observed values, $$\hat{x_i}$$ are the expected values, and $$n$$ is the number of trials.

    Next, use this mean_squared_error function to calculate the normalized MSE between "observed" and "expected."

    Hint: 
    - In the next part, we'll be comparing MSE values across simulations with varying values of *n*.   Design accordingly.

```{r}

```


d. (4 points) Now let's generate some more data. Write R code that will generate the amino acid distribution for
n instances of a sequence of length 10 (using the same code from part b),
with the probabilities from part a, for values of n ranging from 1 to 100,
and calculate the normalized mean squared error for the result from each value of *n*. Save the results in a variable.

    Hints:

     - It may be useful to write a helper function to run the simulation and call mean_squared_error

     - Remember to use apply and its variants rather than for loops when possible!

```{r}

```


e. (3 points) Plot your normalized MSE vector as n goes from 1 to 100.

    Do the mean squared error values converge? If so, where? How many trials does it take to reach this point of convergence?

    What conclusions can you draw from this experiment?

```{r}

```

    *Answer the questions here.*




