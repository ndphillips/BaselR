---
title       : FFTrees
subtitle    : An R package to create, visualise, and impliment fast and frugal decision trees
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

<!-- As the city’s principal public hospital, Cook County was the place of last resort for the hundreds of thousands of Chicagoans without health insurance. Resources were stretched to the limit. The hospital’s cavernous wards were built for another century. There were no private rooms, and patients were separated by flimsy plywood dividers. There was no cafeteria or private telephone—just a payphone for everyone at the end of the hall. In one possibly apocryphal story, doctors once trained a homeless man to do routine lab tests because there was no one else available. -->

<!-- > But the Emergency Department (the ED) seemed to cry out for special attention. The rooms were jammed. A staggering 250,000 patients came through the ED every year. How do you figure out who needs what? How do you figure out how to direct resources to those who need them the most?” -->



--- 


## Diagnosing ER patients with chest pain.

<!-- A significant number of those people filing into the ED—on average, about thirty a day—were worried that they were having a heart attack.  Chest-pain patients were resource-intensive. The treatment protocol was long and elaborate and—worst of all—maddeningly inconclusive. -->


### Situation

> - 30 people a day worried about a heart attack. 
> - Coronary care bed costs $2,000 a night and requires a 3 day stay.

### Decision task

> - Send people with heart attacks to the coronary care bed, and healthy patients to a normal bed.

### Multiple, uncertain measures

> - Blood pressure, Stethescope,
> - Questions: How long? How much? During exercise? History? Cholesterol? Drugs? etc.
> - Electrocardiogram (ECG) reading.


--- .class #id 

## Intuition based on clinical expertise was not working.

### Study: How much do doctors agree on diagnoses?

> - Asked doctors to estimate from 0 to 100 the probability of a heart attack of 20 separate patients.


<img src="images/medicalhistory.jpg" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" width="30%" style="display: block; margin: auto;" />

--- #blockquote

## Conclusion: Terribly low agreement

### "In each case the answers we got pretty much ranged from 0 to 100. It was extraordinary" (Brenden Reilly, Department of Medicine chairman)



--- .class #id 

## Solution

> - A decision tree developed by a cardiologist named Lee Goldman.

<img src="images/cooktree.gif" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" width="30%" style="display: block; margin: auto;" />


### Why use a decision tree?

> - Speed, Easy of understanding and implementation





--- &twocol

*** =left

## The Cook hospital decision tree

> - Over two years, the performance of the tree was compared to the physician's intuitive judgments.

### Results

> - Tree dramatically outperformed the doctor's clinical judgments and resulted in far fewer false-positives and huge cost savings

> - To this day, the tree is still used at the hospital.

*** =right

<img src="images/goldman.jpg" title="Dr. Lee Goldman" alt="Dr. Lee Goldman" width="75%" style="display: block; margin: auto;" />


---  &twocol

*** =left
## Fast and frugal decision trees (FFT)

> - A fast and frugal decision tree (FFT) is a very simple, highly restricted decision tree.

> - In an FFT, each node has exactly two branches, where at least one branch is an exit branch (Martignon et al., 2008).

> - FFTs are even faster and require less information than non-FFT trees.

*** =right

<img src="images/traintreestats.png" title="plot of chunk test" alt="plot of chunk test" width="100%" style="display: block; margin: auto;" />

--- .class #id 

## Depression Tree

- Jenny et al. (2013): Simple rules for detecting depression

<img src="images/depressiontree.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" width="40%" style="display: block; margin: auto;" />

--- .class #id 

## Bank failure

- Neth et al. (2013): Homo heuristics in the financial world: From risk management to managing uncertainty

<img src="images/nethtree.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" width="40%" style="display: block; margin: auto;" />

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

There is no off-the-shelf method to construct FFTs

### Task
- Create an easy-to-use R package that constructs, visualizes, and implements FFTs called `FFTrees`.

<img src="images/FFTrees_Logo.jpg" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" width="40%" style="display: block; margin: auto;" />

--- .class #id 
## FFTrees


```r
# Available on CRAN
install.packages("FFTrees")
devtools::github("ndphillips/FFTrees", include_vignette = TRUE)
```


--- .class #id 
## Heart disease datatset


```r
library(FFTrees)

head(heartdisease)
```

```
##     age sex cp trestbps chol fbs     restecg thalach exang oldpeak slope
## 138  62   1 aa      120  281   0 hypertrophy     103     0     1.4  flat
## 250  62   1 aa      128  208   1 hypertrophy     140     0     0.0    up
## 260  57   1 aa      124  261   0      normal     141     0     0.3    up
## 173  59   0  a      174  249   0      normal     143     1     0.0  flat
## 105  49   1 np      120  188   0      normal     139     0     2.0  flat
## 19   48   0 np      130  275   0      normal     139     0     0.2    up
##     ca   thal diagnosis
## 138  1     rd         1
## 250  0 normal         0
## 260  0     rd         1
## 173  0 normal         1
## 105  3     rd         1
## 19   0 normal         0
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

--- .class #id 
## Evaluating a decision algorithm


<img src="images/confusiontable.png" title="plot of chunk unnamed-chunk-12" alt="plot of chunk unnamed-chunk-12" width="60%" style="display: block; margin: auto;" />




--- .class #id 
## FFT summary statistics


```r
# Step 3: Summary statistics
heart.fft
```

```
## [1] "7 FFTs using up to 4 of 13 cues"
## [1] "FFT #3 uses 4 cues {thal,ca,cp,oldpeak} with the following performance:"
##       train   test
## n    150.00 153.00
## pci    0.87   0.88
## mcu    1.87   1.62
## acc    0.81   0.80
## bacc   0.79   0.79
## sens   0.63   0.63
## spec   0.95   0.94
```

--- .class #id 
## Heart disease cue accuracies



```r
plot(heart.fft, what = "cues", main = "Heart Disease")
```

![plot of chunk unnamed-chunk-14](assets/fig/unnamed-chunk-14-1.png)

--- .class #id 
## Heart Disease FFT

```r
plot(heart.fft, 
     main = "Heart Disease", 
     decision.names = c("healthy", "sick"),
     stats = FALSE)
```

<img src="assets/fig/unnamed-chunk-15-1.png" title="plot of chunk unnamed-chunk-15" alt="plot of chunk unnamed-chunk-15" style="display: block; margin: auto;" />


--- .class #id 
## Heart Disease FFT

```r
plot(heart.fft, 
     main = "Heart Disease", 
     decision.names = c("healthy", "sick"))
```

### Wait for it...

--- .class #id 
## Heart Disease FFT - Training
<img src="assets/fig/unnamed-chunk-17-1.png" title="plot of chunk unnamed-chunk-17" alt="plot of chunk unnamed-chunk-17" style="display: block; margin: auto;" />



--- .class #id 
## Heart Disease FFT - Prediction

<img src="assets/fig/unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" style="display: block; margin: auto;" />

--- .class #id 
## Heart Disease FFT - Tree 3

<img src="assets/fig/unnamed-chunk-19-1.png" title="plot of chunk unnamed-chunk-19" alt="plot of chunk unnamed-chunk-19" style="display: block; margin: auto;" />

--- .class #id
## Heart Disease FFT - Tree 6

<img src="assets/fig/unnamed-chunk-20-1.png" title="plot of chunk unnamed-chunk-20" alt="plot of chunk unnamed-chunk-20" style="display: block; margin: auto;" />

--- .class #id 
## How do FFTs compare to regression and CART?

- Simplicity: *How much information is used and how is it combined?*
- Accuracy: *How well can the algorithm predict new data?*

--- .class #id 
## Heart disease: regression
- 4 significant cues: (sex, cp, trestbps, ca)

<img src="assets/fig/unnamed-chunk-21-1.png" title="plot of chunk unnamed-chunk-21" alt="plot of chunk unnamed-chunk-21" style="display: block; margin: auto;" />

--- .class #id 
## Heart disease: rpart

- 8 cues (thal, cp, oldpeak, ca, age, exang, thalach, chol)

<img src="assets/fig/unnamed-chunk-22-1.png" title="plot of chunk unnamed-chunk-22" alt="plot of chunk unnamed-chunk-22" width="50%" style="display: block; margin: auto;" />


--- .class #id 
## Heart disease: FFT

- 3 cues (thal, cp, ca)
- However, when applied to the data, only about 1.75 cues are even used on average. As a result, > 85% of the cue information is completely ignored.

<img src="assets/fig/unnamed-chunk-23-1.png" title="plot of chunk unnamed-chunk-23" alt="plot of chunk unnamed-chunk-23" style="display: block; margin: auto;" />


--- .class #id 
## Heart disease classification accuracy




<img src="assets/fig/unnamed-chunk-25-1.png" title="plot of chunk unnamed-chunk-25" alt="plot of chunk unnamed-chunk-25" style="display: block; margin: auto;" />



--- .class #id 
## Heart disease classification accuracy

<img src="assets/fig/unnamed-chunk-26-1.png" title="plot of chunk unnamed-chunk-26" alt="plot of chunk unnamed-chunk-26" style="display: block; margin: auto;" />

--- .class #id 
## Heart disease classification accuracy


<img src="assets/fig/unnamed-chunk-27-1.png" title="plot of chunk unnamed-chunk-27" alt="plot of chunk unnamed-chunk-27" style="display: block; margin: auto;" />


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

<img src="images/simulationagg_a.png" title="plot of chunk unnamed-chunk-28" alt="plot of chunk unnamed-chunk-28" width="90%" style="display: block; margin: auto;" />


--- .class #id 
## Aggregate simulation prediction results

<img src="images/simulationagg_b.png" title="plot of chunk unnamed-chunk-29" alt="plot of chunk unnamed-chunk-29" width="90%" style="display: block; margin: auto;" />

--- .class #id 
## Aggregate simulation prediction results

<img src="images/simulationagg_c.png" title="plot of chunk unnamed-chunk-30" alt="plot of chunk unnamed-chunk-30" width="90%" style="display: block; margin: auto;" />


--- .class #id 
## Simulation prediction results by dataset

<img src="images/simulation.png" title="plot of chunk unnamed-chunk-31" alt="plot of chunk unnamed-chunk-31" width="90%" style="display: block; margin: auto;" />

--- &twocol

*** =left

## Conclusions

> - FFTrees makes it easy to develop simple, effective, transparent decision trees.
> - Decision aids built with FFTrees can compete with complex, compensatory algorithms in prediction.


## Next steps


> - Speed up code with c++ or Julia.
> - Include *cue costs* into algorithm.
>    - Heart disease FFT: $75.91
>    - Regression: $300 
> - Quantify when and how a tree **fails** when it is applied to data over time.

*** =right

<img src="images/traintreestats.png" title="plot of chunk unnamed-chunk-32" alt="plot of chunk unnamed-chunk-32" width="100%" style="display: block; margin: auto;" />

--- .class #id 
## Questions?

<img src="assets/fig/unnamed-chunk-33-1.png" title="plot of chunk unnamed-chunk-33" alt="plot of chunk unnamed-chunk-33" style="display: block; margin: auto;" />
