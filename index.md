---
title       : FFTrees
subtitle    : An R package to create, visualize, and implement fast and frugal decision trees
author      : Nathaniel Phillips, Economic Psychology, University of Basel
job         : BaselR Meeting, March 2017
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
logo : FFTrees_Logo.jpg
biglogo : FFTrees_Logo.jpg
---





## A growing problem at the Cook County Hospital in 1996

<img src="images/cookmap.png" title="plot of chunk cookmap" alt="plot of chunk cookmap" width="70%" style="display: block; margin: auto;" />


--- .class #id 

## Patient overload

<img src="images/crowdedemergency.jpg" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" width="70%" style="display: block; margin: auto;" />




--- &twocol

*** =left

## Diagnosing heart attacks

<!-- A significant number of those people filing into the ED—on average, about thirty a day—were worried that they were having a heart attack.  Chest-pain patients were resource-intensive. The treatment protocol was long and elaborate and—worst of all—maddeningly inconclusive. -->

- 30 people a day worried about a heart attack. 
- Coronary care bed costs $2,000 a night and requires a 3 day stay.
- Send true heart attacks to the coronary care bed, and true healthy patients to a normal bed.

### Multiple, uncertain measures

- Electrocardiogram (ECG), Blood pressure, Stethescope, How long? How much? During exercise? History? Cholesterol? Drugs? etc.


*** =right

<img src="images/chestpain.jpg" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" width="100%" style="display: block; margin: auto;" />

--- .class #id 

## How good were doctor's intuitive decisions?

- Task: Estimate from 0 to 100 the probability of a heart attack of 20 separate patients.

<img src="images/medicalhistory.jpg" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" width="30%" style="display: block; margin: auto;" />

--- .class #id 

## Answer: Not consistent

### "In each case the answers we got pretty much ranged from 0 to 100. It was extraordinary" (Brenden Reilly, Department of Medicine chairman)

<img src="images/arguingdoctors.jpg" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" width="70%" style="display: block; margin: auto;" />


--- .class #id 

## Solution

- A fast and frugal decision tree (FFT) developed by a cardiologist named Lee Goldman.


<img src="images/cooktree.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" width="20%" style="display: block; margin: auto;" />


### Why use a decision tree?

> - Speed, Consistency, Easy to understand and use 'in the head'


--- .class #id 


<img src="images/cooktree.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" width="40%" style="display: block; margin: auto;" />


---  &twocol

*** =left

## The Cook hospital decision tree

- Over two years, the performance of the tree was compared to the physician's intuitive judgments.

### Results

> - Doctor's accuracy: 75-90%
> - Decision tree accuracy: 95%
> - Tree had far fewer false-positives and huge cost savings
> - To this day, the tree is still used at the hospital.

*** =right

<img src="images/cooktree.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" width="80%" style="display: block; margin: auto;" />




---  &twocol

*** =left
## Standard decision tree

> - Standard decision trees created by unrestricted algorithms can become very complex

> - Complexity -> High costs, Difficult to understand, prone to overfitting. 

<img src="images/complextree.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" width="100%" style="display: block; margin: auto;" />

*** =right

## Fast and Frugal tree

> - A fast and frugal decision tree (FFT) is a very simple, highly restricted decision tree where each node has exactly two branches, where at least one branch is an exit branch (Martignon et al., 2008).

> - FFTs -> Cheap, easy to understand, and rarely overfit.


<img src="images/traintreestats.png" title="plot of chunk unnamed-chunk-10" alt="plot of chunk unnamed-chunk-10" width="60%" style="display: block; margin: auto;" />

<!-- --- .class #id  -->

<!-- ## Depression Tree -->

<!-- - Jenny et al. (2013): Simple rules for detecting depression -->

<!-- ```{r , fig.margin = TRUE, echo = FALSE, out.width = "40%", fig.align='center'} -->
<!-- knitr::include_graphics(c("images/depressiontree.png")) -->
<!-- ``` -->

<!-- --- .class #id  -->

<!-- ## Bank failure -->

<!-- - Neth et al. (2013): Homo heuristics in the financial world: From risk management to managing uncertainty -->

<!-- ```{r , fig.margin = TRUE, echo = FALSE, out.width = "40%", fig.align='center'} -->
<!-- knitr::include_graphics(c("images/nethtree.png")) -->
<!-- ``` -->

<!-- --- &twocol -->

<!-- ## Patient success -->

<!-- *** =left -->



<!-- ### FFT -->
<!-- ```{r echo = FALSE} -->
<!-- library(FFTrees) -->
<!-- plot(tree.63.m, stats = FALSE, tree = 5) -->
<!-- ``` -->



<!-- *** =right -->


<!-- ### rpart tree -->
<!-- ```{r echo = FALSE} -->
<!-- plot(rpart.63) -->
<!-- text(rpart.63) -->
<!-- ``` -->

--- .class #id 
## Problem

- There is no off-the-shelf method to construct FFTs.
  - Previous researchers have individually constructed their FFTs.

### Task
- Create an easy-to-use R package that constructs, visualizes, and implements FFTs.

<img src="images/FFTrees_Logo.jpg" title="plot of chunk unnamed-chunk-11" alt="plot of chunk unnamed-chunk-11" width="30%" style="display: block; margin: auto;" />


--- .class #id 
## FFTrees


```r
# v1.1.8 available on CRAN
install.packages("FFTrees")

# v1.2.0 on github
devtools::github("ndphillips/FFTrees", include_vignette = TRUE)
```


--- .class #id 
## Heart disease datatset


```r
library(FFTrees)

head(heartdisease)
```

```
##   age sex cp trestbps chol fbs     restecg thalach exang oldpeak slope ca
## 1  63   1 ta      145  233   1 hypertrophy     150     0     2.3  down  0
## 2  67   1  a      160  286   0 hypertrophy     108     1     1.5  flat  3
## 3  67   1  a      120  229   0 hypertrophy     129     1     2.6  flat  2
## 4  37   1 np      130  250   0      normal     187     0     3.5  down  0
## 5  41   0 aa      130  204   0 hypertrophy     172     0     1.4    up  0
## 6  56   1 aa      120  236   0      normal     178     0     0.8    up  0
##     thal diagnosis
## 1     fd         0
## 2 normal         1
## 3     rd         1
## 4 normal         0
## 5 normal         0
## 6 normal         0
```

--- .class #id 
## Creating a Heart Disease FFT


```r
# Step 1: Create training and test data
set.seed(100)

heartdisease <- heartdisease[sample(nrow(heartdisease)),]
heart.train <- heartdisease[1:150,]
heart.test <- heartdisease[151:303,]

# Step 2: Create heart.fft
heart.fft <- FFTrees(formula = diagnosis ~.,
                     data = heart.train,
                     data.test = heart.test)
```

<!-- --- .class #id  -->
<!-- ## Evaluating a decision algorithm -->


<!-- ```{r , fig.margin = TRUE, echo = FALSE, out.width = "60%", fig.align='center'} -->
<!-- knitr::include_graphics(c("images/confusiontable.png")) -->
<!-- ``` -->




--- .class #id 
## FFT summary statistics


```r
# Step 3: Summary statistics
heart.fft
```

```
## [1] "7 FFTs using up to 4 of 13 cues"
## [1] "FFT #4 uses 3 cues {thal,cp,ca} with the following performance:"
##       train   test
## n    150.00 153.00
## pci    0.88   0.88
## mcu    1.74   1.73
## acc    0.80   0.82
## bacc   0.80   0.82
## sens   0.82   0.88
## spec   0.79   0.76
```

--- &twocol

*** =left

## Heart Disease FFT
<img src="figure/unnamed-chunk-16-1.png" title="plot of chunk unnamed-chunk-16" alt="plot of chunk unnamed-chunk-16" style="display: block; margin: auto;" />



*** =right

### 3 cues


| cue| description|values |
|:------|:----|:-----|
|     `thal`|    thallium scintigraphy, a nuclear imaging test that shows how well blood flows into the heart.|normal (n), indicate a fixed defect (fd), or a reversible defect (rd)     |
|     `cp`|    Chest pain type| Typical angina (ta), atypical angina (aa), non-anginal pain (np), or asymptomatic (a)     |
|     `ca`| Number of major vessels colored by flourosopy, a continuous x-ray imaging tool|0, 1, 2 or 3 |


--- .class #id 
## Heart Disease FFT | Training
<img src="figure/unnamed-chunk-17-1.png" title="plot of chunk unnamed-chunk-17" alt="plot of chunk unnamed-chunk-17" style="display: block; margin: auto;" />



--- .class #id 
## Heart Disease FFT | Prediction

<img src="figure/unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" style="display: block; margin: auto;" />


--- .class #id 
## Heart Disease FFT | ROC

<img src="images/roc.png" title="plot of chunk unnamed-chunk-19" alt="plot of chunk unnamed-chunk-19" width="80%" style="display: block; margin: auto;" />


--- .class #id 
## Heart Disease FFT | Tree 4

<img src="figure/unnamed-chunk-20-1.png" title="plot of chunk unnamed-chunk-20" alt="plot of chunk unnamed-chunk-20" style="display: block; margin: auto;" />



--- .class #id 
## Heart Disease FFT | Tree 3

<img src="figure/unnamed-chunk-21-1.png" title="plot of chunk unnamed-chunk-21" alt="plot of chunk unnamed-chunk-21" style="display: block; margin: auto;" />

--- .class #id
## Heart Disease FFT | Tree 6

<img src="figure/unnamed-chunk-22-1.png" title="plot of chunk unnamed-chunk-22" alt="plot of chunk unnamed-chunk-22" style="display: block; margin: auto;" />

<!-- --- .class #id  -->
<!-- ## How do FFTs compare to regression and CART? -->

<!-- - Simplicity: *How much information is used and how is it combined?* -->
<!-- - Accuracy: *How well can the algorithm predict new data?* -->

<!-- --- .class #id  -->
<!-- ## Heart disease: regression -->
<!-- - 4 significant cues: (sex, cp, trestbps, ca) -->

<!-- ```{r echo = FALSE, fig.width = 10, fig.height = 6, fig.align = 'center'} -->
<!-- heart.lm <- glm(diagnosis ~., data = heartdisease, family = "binomial") -->
<!-- #heart.lm$coefficients -->
<!-- #summary(heart.lm)$coefficients -->

<!-- bar.cols <- rep("white", nrow(summary(heart.lm)$coefficients)) -->
<!-- bar.cols[summary(heart.lm)$coefficients[,4] < .05] <- yarrr::piratepal("basel", trans = .6, length.out = sum(summary(heart.lm)$coefficients[,4] < .05)) -->

<!-- barplot(height = abs(summary(heart.lm)$coefficients[,3]),  -->
<!--         names.arg = rownames(summary(heart.lm)$coefficients), -->
<!--         col = bar.cols, ylab = "Coefficient z-scores", main = "Heart Disease logistic regression") -->

<!-- abline(h = 1.96) -->

<!-- ``` -->



--- .class #id 
## Heart disease cue accuracies


```r
plot(heart.fft, what = "cues", main = "Heart Disease")
```

<img src="figure/unnamed-chunk-23-1.png" title="plot of chunk unnamed-chunk-23" alt="plot of chunk unnamed-chunk-23" style="display: block; margin: auto;" />



--- .class #id 
## FFForest()

### Visualise cue importance and co-occurence

<img src="figure/unnamed-chunk-24-1.png" title="plot of chunk unnamed-chunk-24" alt="plot of chunk unnamed-chunk-24" style="display: block; margin: auto;" />


--- .class #id 
## Comparing FFTs to standard trees


### How does the FFT created by FFTrees compare to a 'standard' decision tree created by rpart?


--- &twocol

*** =left
## Heart disease: rpart

- 8 cues (thal, cp, oldpeak, ca, age, exang, thalach, chol)

<img src="figure/unnamed-chunk-25-1.png" title="plot of chunk unnamed-chunk-25" alt="plot of chunk unnamed-chunk-25" width="80%" style="display: block; margin: auto;" />


**** =right
## Heart disease: FFT

> - 3 cues (thal, cp, ca)


<!-- |     |  train|   test| -->
<!-- |:----|------:|------:| -->
<!-- |n    | 150.00| 153.00| -->
<!-- |pci  |   0.87|   0.88| -->
<!-- |mcu  |   1.87|   1.62| -->
<!-- |acc  |   0.81|   0.80| -->
<!-- |bacc |   0.79|   0.79| -->
<!-- |sens |   0.63|   0.63| -->
<!-- |spec |   0.95|   0.94| -->

> - The FFT is very cheap to implement
>    - Heart disease FFT: $75.91
>    - Regression: $300 


<img src="images/traintreestats.png" title="plot of chunk unnamed-chunk-26" alt="plot of chunk unnamed-chunk-26" width="70%" style="display: block; margin: auto;" />

<!-- ```{r, eval = TRUE, fig.align = 'center', echo = FALSE} -->
<!-- plot(heart.fft,  -->
<!--      main = "Heart Disease",  -->
<!--      decision.names = c("healthy", "sick"), -->
<!--      stats = FALSE) -->
<!-- ``` -->


--- .class #id
## Heart disease classification accuracy




<img src="figure/unnamed-chunk-28-1.png" title="plot of chunk unnamed-chunk-28" alt="plot of chunk unnamed-chunk-28" style="display: block; margin: auto;" />



--- .class #id
## Heart disease classification accuracy

<img src="figure/unnamed-chunk-29-1.png" title="plot of chunk unnamed-chunk-29" alt="plot of chunk unnamed-chunk-29" style="display: block; margin: auto;" />

--- .class #id
## Heart disease classification accuracy


<img src="figure/unnamed-chunk-30-1.png" title="plot of chunk unnamed-chunk-30" alt="plot of chunk unnamed-chunk-30" style="display: block; margin: auto;" />


--- .class #id 
## How accurate are FFTs built by FFTrees?

- Prediction competition
    - 10 datasets taken from the UCI machine learning database
    - 50% Fitting / 50% Prediction subsample splitting, DV: balanced accuracy = (sensitivity + specificity) / 2

|dataset     | cases| cues| base.rate|
|:-----------|-----:|----:|---------:|
|arrhythmia  |    68|  280|      0.29|
|audiology   |   226|   70|      0.10|
|breast      |   683|   10|      0.35|
|bridges     |    92|   10|      0.39|
|cmc         |  1473|   10|      0.35|

Table: 5 of the 10 prediction datasets

--- .class #id 
## Aggregate simulation prediction results

<img src="images/simulationagg_a.png" title="plot of chunk unnamed-chunk-31" alt="plot of chunk unnamed-chunk-31" width="90%" style="display: block; margin: auto;" />


--- .class #id 
## Aggregate simulation prediction results

<img src="images/simulationagg_b.png" title="plot of chunk unnamed-chunk-32" alt="plot of chunk unnamed-chunk-32" width="90%" style="display: block; margin: auto;" />

--- .class #id 
## Aggregate simulation prediction results

<img src="images/simulationagg_c.png" title="plot of chunk unnamed-chunk-33" alt="plot of chunk unnamed-chunk-33" width="90%" style="display: block; margin: auto;" />


--- .class #id 
## Simulation prediction results by dataset

<img src="images/simulation.png" title="plot of chunk unnamed-chunk-34" alt="plot of chunk unnamed-chunk-34" width="90%" style="display: block; margin: auto;" />

--- &twocol

*** =left

## Conclusions

> - FFTrees makes it easy to develop simple, effective, transparent decision trees.
> - FFTrees can compete with complex decision algorithms, even Random Forests and Support Vector machines, in pure prediction.


## Next steps


> - Speed up code with c++ or Julia.
> - Include *cue costs* into algorithm.
> - Quantify when and how a tree **fails** when it is applied to data over time.

*** =right

<img src="images/traintreestats.png" title="plot of chunk unnamed-chunk-35" alt="plot of chunk unnamed-chunk-35" width="100%" style="display: block; margin: auto;" />


--- &twocol

*** =left

## Please help and contribute!

- I want more real-world tests of `FFTrees`! If you have data you want to try `FFTrees` on, or can think of new features, please let me know and I would be happy to collaborate.

- I am very happy for contributions and bug reports at `github.com/ndphillips/FFTrees`, 

*** =right

[](https://www.revive-adserver.com/media/GitHub.jpg)

<img src="https://www.revive-adserver.com/media/GitHub.jpg" title="plot of chunk unnamed-chunk-36" alt="plot of chunk unnamed-chunk-36" width="100%" style="display: block; margin: auto;" />


--- .class #id 
## Questions?

<img src="figure/unnamed-chunk-37-1.png" title="plot of chunk unnamed-chunk-37" alt="plot of chunk unnamed-chunk-37" style="display: block; margin: auto;" />

--- &twocol

*** =left
## FFTrees algorithm

1. Calculate a decision threshold `t` for each cue that maximizes the cue’s balanced accuracy `bacc` in training.

2. Rank cues in order of their maximum balanced accuracy -- select the top N cues. 

3. Creates all possible `2^{N−1}` trees with these cues, using all exit structures.

*** =right

<img src="figure/unnamed-chunk-38-1.png" title="plot of chunk unnamed-chunk-38" alt="plot of chunk unnamed-chunk-38" style="display: block; margin: auto;" />



