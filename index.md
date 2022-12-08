---
layout: tutorial
title: Principle Component Analysis (PCA) and text labelling
subtitle: Interpret PCA and advanced PCA plot with text labelling
date: 2022-12-8
author: Cong zhaowei
output: html_document
---

## Tutorial Aims

#### 1.Get familiar with adding the text labels using ggplot2 and ggrepal

#### 2.Understand ordination and the theory behind the Principle Component Analysis (PCA)

#### 3.Understand when you can and should apply PCA to your results

#### 4.Learn how to interpret PCA and apply it in R. What do the plot and summary show you?

#### 5.Learn advanced skills for PCA visualisation using ggbiplot and ggplot; combine part 1 and part 3 to provide points with names in PCA.

## Steps:

#### <a href="#a"> Introduction </a>

-   

    #### <a href="#b"> Pre-requirement </a>

-   

    #### <a href="#c"> Data description </a>

#### <a href="#d"> Part 1 Text labelling </a>

-   

    #### <a href="#e"> geom_text </a>

-   

    #### <a href="#f"> geom_label </a>

-   

    #### <a href="#g"> geom_text_repel </a>

-   

    #### <a href="#h"> geom_label_repel </a>

#### <a href="#i"> Part 2 Principle Component Analysis(PCA) </a>

-   

    #### <a href="#j"> Introduce the ordination  </a>

-   

    #### <a href="#k"> What is PCA </a>

-   

    #### <a href="#l"> when to use PCA </a>

#### <a href="#m"> Part 3 Interpret PCA results and plot </a>

-   

    #### <a href="#n"> Apply PCA and Interpret PCA results  </a>

-   

    #### <a href="#o"> Plot the PCA </a>

-   

    #### <a href="#p"> Advanced PCA visualization</a>

#### <a href="#q"> Summary </a>

#### <a href="#r"> Challenge </a>

<a name="a"></a>

# Introduction

As an ecology student, do you often find yourself dealing with "wide" datasets which you are unable to visualize easily? What options do you have when the data cannot be plotted neatly in its raw format?

First, compare the two variables to determine whether or not they are positively correlated or inversely correlated with each other. After plotting the data, you can then investigate the association between them. You would be able to have a better understanding of the data if you were able to display the name of each point as well as its significance. The discussion thus far has focused on two variables; however, what if you have three variables? It is possible to examine the relationship between variables 1-2, 1-3, and 2-3; however, doing so would require a great deal of work. You may find that three-dimensional graphs suit your needs better. Now, let's say you have to deal with four, five, or even ten different variables. Here you would need an enormous number of 2D plots, and trying to make sense of them is a hassle. You may drive yourself crazy by attempting a bizarre plot that has an axis for each variable in the data set. It is plainly evident that these approaches are too rudimentary. After completing the tutorial for today, you will understand that principal component analysis (PCA) is an approach that is more effective in terms of clarity, speed, and suitability when it comes to displaying 'wide' datasets.

First, we will go over the basics for warmup and assist you in labelling the points when contrasting two different variables. Following that, we will concentrate on the visualization of data. In the first part of the tutorial, you will learn about geom text(), geom label()in ggplot2, and ggrepel. In the second part of the tutorial, you will learn about PCA, a part of ordination. PCA converts the correlation of all the variables and turns it into a two-dimensional format. It is a dimensionality-reduction method that helps you out with enormous data frames containing hundreds of variables. We will explore this further after part one. Throughout the tutorial, we will work with the mtcars data set, which is a built-in R Data set. It contains 11 variables and illustrates the principal component analysis (PCA) nicely, proving it to be a highly effective and powerful tool.

<a name="b"></a>

## Pre-requirement

Since we will be utilizing the programming language R throughout this tutorial, the target audience for this tutorial is learners who already have some familiarity with the R language and utilize R-studio. The choice of content should be based on your personal situation. You may choose the first section to learn if you don't require the ability to analyze large datasets or do multivariate analysis. The second half of the tutorial will be relevant if you want to expand on the fundamental understanding of R that you already possess or use it in your experiments for multivariate analysis.

If you intend to understand today's tutorial in an effective and timely way, you need at least have some experience with data visualization and be comfortable working with it. You may get assistance <a href="https://ourcodingclub.github.io/tutorials/datavis/">here</a>.

<a name="c"></a>

## Data description

In this part of the lesson, you will practice doing a Part 1 and Part 3 using the mtcars dataset that is pre-loaded in R. This dataset contains information on 32 different makes and models of automobiles, which was gathered from an automobile magazine known as the Motor Trend Magazine in 1974. It consists of fuel consumption in addition to 11 characteristics of automotive design and performance for a total of 32 vehicles (1973---74 models). In other words, the data frame contains 32 observations on 11 numeric variables.

| Variable description                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------|
| **mpg**: fuel consumption, which is measured in miles per (US) gallon; vehicles that are powerful and weights more often consume more fuel; it is a measure of fuel efficiency                                                                                                                                                                                                             |
| **cyl**: number of cylinders; it's common for vehicles with higher horsepower to possess more cylinders. Vehicles with 4, 6, and 8 cylinder engines are contained in the data                                                                                                                                                                                                              |
| **disp**: displacement (cu.in.); the sum of the volume of the engine's cylinders; it is a reasonable approximation of overall output power.                                                                                                                                                                                                                                                |
| **hp**: gross horsepower; gross horsepower is a measurement of an engine's potential power; this rating is calculated with the engine operating in a vacuum and not attached to any particular vehicle                                                                                                                                                                                     |
| **drat**: Rear axle ratio; it specifies the ratio between the number of revolutions of the drive shaft and the number of rotations of the wheels. If you increase it, fuel efficiency will go down. Greater towing capacity may be attained with a vehicle with a higher ratio                                                                                                             |
| **wt**: Weight (lb/1000) The vehicle's total weight in relation to one thousand pounds (half US ton)                                                                                                                                                                                                                                                                                       |
| **qsec**: 1/4 mile time; measures the acceleration and speed of an automobile, an indicator of performance                                                                                                                                                                                                                                                                                 |
| **vs**: V/S or engine block: this shows if the engine is in the conventional straight configuration or a "V" configuration                                                                                                                                                                                                                                                                 |
| **am**: transmission; whether the automobile has an automated transmission (0) or a manual transmission (1)                                                                                                                                                                                                                                                                                |
| **gear**:amount of forward gears; race cars frequently come with a larger number of gears than regular vehicles. Four or five forward gears are available in manual transmissions and three or four in automatics                                                                                                                                                                          |
| **carb**: number of carburetors; higher carburetor count is often correlated with powerful engines. It is to accommodate the greater airflow rate of a powerful engine; engines with a greater displacement often have a higher barrel layout. This allows for more capacity for an engine when it might require it, rather than limiting power output with a restricted number of barrels |

<a name="d"></a>

# Part 1 text labelling

Labeling various sorts of shapes is facilitated by text geometries. You may use them to create scatter plots by themselves or in conjunction with other geometries, such as to indicate point labels or the height of a bar. The geom text() function adds label points to the plot, while geom label() inputs a rectangular shape after the text to make it more readable. Note that the text labels will try to push one another away from the data points and the boundaries of the drawing area as they compete with one another. The geom_text_repel command inserts text into the drawing. In order to make the text more legible, the geom_label_repel function creates a rectangle underneath it. Both text labels will be repelled by one another and will keep their distance from the data points.

![](images/car1.jpeg) Credits: <a href="https://www.pinterest.co.uk/pin/604749056231636526/">Leadfoot TV</a>

First, launch a blank R script in R Studio and jot down the name of this tutorial, your name, along with the date. The mtcars dataset is chosen for this tutorial. It is a Built-in R dataset acquired from an American automobile magazine and contains ten different characteristics of automotive design and performance. You can use this dataset directly without having to load or use the `read.csv` function. We need to load the packages that we will need today if you don't already have them installed. Input `install. packages(‘package_name')` to do that.

``` r
library(ggrepel) #provides overlapping text labels
library(ggplot2) #plots and creates beautiful and informative graphs
#load the mtcars
data(mtcars)
data <- mtcars
```

When we have a large number of variables in our dataset, we may wish to compare two of them by first plotting them in a general sense. During the course of the ggplot visualization tutorial, you have already gained some basic understanding. Now that we have determined that the ratio of the rear axle and its weight are of importance to us let's plot them out to find out what's happening.

``` r
ggplot(data, aes(wt, drat))+
  geom_point(color = “black") # plotting drat vs wt and making the points black
```

![](images/plot_point1.png)

It looks like a chessboard, and it's not easy to identify anything. We are simply aware of the fact that the variables drat and wt are inversely correlated; however, we cannot identify which automobile is which.

<a name="e"></a>

## geom_text

Let's provide a label for every individual point. `rownames()` is responsible for generating a column that contains the names of each row, while `geom_text` is the function responsible for adding text to the plot.

``` r
data$car <- rownames(data)# creates a new column labelled vehicle and fills it with the name of each row.
ggplot(data, aes(wt, drat, label = car)) +
  geom_point(color = "black")+
  geom_text()# adds text to each point
```

![](images/plot_point2.png)

Does it look better now? Not at all! It is not satisfactory since there are still too many overlapping. Let's investigate the possible ways in which we might make it more appealing.

If `TRUE` is chosen, any text that is plotted in a layer that overlaps with the preceding text will be ignored. `check_overlap` is performed while the drawing is being done and in the order of the data. Consequently, the data need to be sorted according to the label column before using either `geom label()` or `geom text()`

``` r
ggplot(data, aes(wt, drat, label = car)) +
  geom_point(color = "black")+
  geom_text(check_overlap = TRUE) #prevents overlapping
```

![](images/plot_point3.png)

Everything looks better now. Let's say that we wish to move the text to a different location.

``` r
ggplot(data, aes(wt, drat, label = car)) +
  geom_point(color = "black")+
  geom_text(check_overlap = TRUE,
            size=5,#change the size of the text
            hjust = 0, nudge_x = 0.1)
            #horizontal justification 0 and adjust the starting 0.1 position of the text label

#give the text angle and coloured by drat
ggplot(data, aes(wt, drat, label = car)) +
  geom_point(color = "black")+
  geom_text(check_overlap = TRUE,
                angle = 20,
                vjust = -0.5, nudge_y = 0.05,
                aes(colour = factor(drat),size = wt),show.legend = FALSE)
```

![](images/plot_point_4.png)

![](images/plot_point5.png)

Now that you are familiar with `geom_text()`, Let's try to make the label more understandable. In order to make the text more legible, we are going to put a rectangle behind it using the `geom_label()` function.At the moment, the `check_overlap` argument and the angle aesthetic are not supported by the `geom _label()` function. In addition to this, it takes substantially more time than geom_text(). The label's background colour may be altered by changing the fill aesthetic's settings.

<a name="f"></a>

## geom_label

``` r
ggplot(data, aes(wt, drat, label = car)) +
  geom_point(color = "black")+
  geom_label(aes(fill = factor(drat)),#fill based on the drat
             colour = "white", #the color of text
             show.legend = FALSE,
             fontface = "italic")#font of text
```

![](images/plot_point6.png)

<a name="g"></a>

## geom_text_repel

It has improved a little further now. As you can probably see that some labels are obscured by the overlapping controls. Don't worry about it. We can make it simpler and neater by using the `geom_text_repel()` function. There are occasions when we do not wish to prevent labels from repelling labels from plot (panel) edges.

``` r
ggplot(data, aes(wt, drat, label = car)) +
  geom_point(color = "black")+
  geom_text_repel(
    color = "black",
    bg.color = "grey70", # shadow color
    bg.r = 0.1,       # shadow radius
    xlim = c(NA, Inf), #Repel from the left edge rather than the right. NA indicates the edge of the panel.
                  ylim = c(-Inf, Inf))+ # Repels neither from the top nor the bottom edges.
  theme_classic()
```

![](images/plot_point7.png)

<a name="h"></a>

## geom_label_repel

same way for geom_label_repel

``` r
ggplot(data, aes(wt, drat, label = car)) +
  geom_point(color = "black")+
  geom_label_repel(fill = "white", xlim = c(NA, Inf), ylim = c(-Inf, Inf))+
  theme_classic()
```

![](images/plot_point8.png)

Now that you have a working knowledge of text labels, it is time to choose some points on the plot for you to label.

``` r
cars <- c("Volvo 142E", "Merc 230","Merc 450SE","Honda Civic", "Porsche 914-2")# selects the points you want
ggplot(data) +
  aes(wt, drat, label = ifelse(car %in% cars, car, "")) +
  coord_polar(theta = "x")+#plot the polar the coordinates
  geom_point(color = "red")+
  geom_label_repel(
    aes(fill = factor(drat)),colour = "white",show.legend = FALSE,fontface = "italic",
    point.padding = 0.3,  # this is additional padding around each point
    nudge_x = .15,
    nudge_y = .5,
    segment.linetype = 6,# defines arrow’s line type
    segment.curvature = -1e-20, # defines curvature of the line
    arrow = arrow(length = unit(0.02, "npc"), type = "closed", ends = "first"))#defines arrow's type, size and position
```

![](images/plot_point9.png)

<a name="i"></a>

# Part 2 Principle Component Analysis(PCA)

<a name="j"></a>

## Introduce the ordination

Ordination is a multivariate method used to summarize and visualize ecological data into fewer dimensions, normally organized based on eigenanalysis or the level of resemblance (measures of similarity or dissimilarity) among the samples. Ordinations may be classified as either constrained or unconstrained. Closely located samples imply similarity among species composition and vice versa. The technique of ordination helps to produce hypotheses regarding the relationship between the species makeup of the site and the possible explanatory variables. There have been different methods of ordination analysis since it was first introduced into the field of ecology in the 1950s. There is much debate on the best technique to utilize.

<a name="k"></a>

## What is PCA?

There are a variety of approaches for ordination, and principal component analysis (PCA) is one type of canonical analysis. Other ordination tools can be found <a href="https://ourcodingclub.github.io/tutorials/ordination/">here</a>.

Principle component analysis (PCA) is a method that condenses potentially linked variables into fewer dimensions in order to reduce the total number of dimensions in complicated data sets. These variables are referred to as principal components. PCA indicates the directions in which data is most dispersed and the directions in which it has the greatest variation. It is a way to visualize the data in its raw form and see the trends of the data by eliminating unimportant variables and also their clustering and pattern. In short, the goal of PCA is to discover the coordinates that maximize the variance of each feature.

I can relate to the frustration you're experiencing if you're starting to feel confused. So, let's simplify things, so it's easier to understand.

![](images/butterfly3.jpeg) Credits: <a href="https://ourcodingclub.github.io/tutorials/ordination/">Debra Golden</a>

To illustrate the process of PCA, note that the first image is the original picture in 144-dimensional vector form. The objective is to undergo a transformation from 144 dimensions to one dimension. The outcome is shown in the second picture. We can make out its general outline but not its finer details. PCA ensured that we could still recognize the butterfly as such.

Now that you know the logic of PCA, I will give you more details; we know that: - 1-D graph is a number line - 2-D graph is an x/y plot - 3-D graph contains depth/height - 4-D graph is hard to describe and can't be drawn - 144-D ??? Like that butterfly ?

If you ever find yourself dealing with 144 variables, principal component analysis (PCA) will be your best option. Keep in mind that it is very likely that some of the variables are more important than others.

![](images/pca1.png) Credits: <a href="https://subscription.packtpub.com/book/data/9781838821739/10/ch10lvl1sec68/understanding-principal-component-analysis">Kai Sasaki</a>

This is our original observation, and it gave us a positive correlation. By shifting the points a minimal distance to the best fit line, we are converting it from two dimensions to one dimension (along the dashed line) to projected data points.

![](images/pca2.jpeg) Credits: <a href="https://ourcodingclub.github.io/tutorials/ordination/">Koenraad</a>

And now, we are going to assign it Principal Component 1 (PC1), which is the direction where the majority of the variance is found. And Principal Component 2 (PC2) is the direction where the second largest amount of variation may be found. An eigenvalue is assigned to each Principal Component in the analysis. The total variance of all the data points may be written as the product of all the eigenvalues. It is possible to interpret the variable loadings of the original variables on the principal component analysis (PCA) as a measure of how much each variable "contributed" to the construction of a Principal Component. We are not going to get into the mathematical theory that underlies it here. You'll be able to get more information <a href="https://liorpachter.wordpress.com/2014/05/26/what-is-principal-component-analysis/">here</a>

<a name="l"></a>

## When to use PCA?

-   To analyze data where multi-colinearity exists between the variables or features.
-   To cut down on the number of variables, but cannot figure out which variables may be taken out of consideration entirely.
-   To visualize data, especially with more than three features, clustering and classification of algorithms.
-   To guarantee that the variables are independent and do not influence one another.
-   To increase the reliability of classification and model accuracy.
-   To eliminate random noise in the dataset and to compress data.

<a name="m"></a>

# Part 3 Interpret PCA results and plot

<a name="n"></a>

## Apply PCA and Interpret PCA results

Now is the time to get familiar with the PCA method in R. The principal component analysis (PCA) may be carried out using any one of a number of different packages; however, for the sake of this tutorial, we will use the package that is shown below. You may obtain it by running `install.packages("name")` if you do not already have it. In this third part of the tutorial, we will attempt to analyze the PCA findings and visualize the PCA plot. In this very last part, we will attempt to combine parts one and three in order to create a PCA plot that is more aesthetically pleasing.

Now is the time to deal with a large number of variables! You have the option of creating a brand-new script in R or carrying on with the one you were working on before. If you would want to create a new script, the process that you used before should be followed. We will continue to make use of the mtcars dataset in the following sections.

``` r
# Installs and loads the following packages
install.packages("vegan")

install.packages("dplyr")

library(vegan)

library(dplyr)
```

``` r
#This loads the community dataset, which we'll use for the examples (relevant if you have skipped the first part of the tutorial)

data(mtcars)

#Launch the dataset and look for any obvious patterns if you can.

View(mtcars)

data <- mtcars
```

It is probably very difficult to spot patterns by just looking at the data frame!The data set contains 11 continuous variables, and we will use the first seven today. `prcomp()` function is applied to our mtcars dataset set.`center` and `scale`.equal to TRUE prompts prcomp to standardize the variables prior to the application of PCA:

A helpful hint is that to utilize centering and scaling when the data is discrete or when the variables are independent of one another.Also, we simply take centering into consideration for the continuous and dependent variables.

``` r
pca2<-prcomp(data[, 1:7],scale = TRUE,center = TRUE)
#1:7 is variables you want to investigate

summary(pca2) #shows the variables and individual PCs
```

![](images/table.jpeg)

In a nutshell, what we want to achieve here is the capability of determining the degree of variability shown by each PC. It is clear that PC1 and PC2 have the largest proportions of variation among these seven PCs. The cumulative percentage is likewise shown in PC2. It tells us that PC1 and PC2 together may account for 89% of the total variation. We employ principal component analysis because it allows us to focus on the aspects of the dataset that are most significant, it's acceptable to look at additional principal components if you like. That is something to discuss at a later time.

``` r
#You should also extract your eigenvalue as well. To do this, use get_eigenvalue() to generate it from your PCA.

eig<- get_eigenvalue(pca2)
#this step is not only for visualization, the results may be useful for analysis as well.
```

<a name="o"></a>

## Plot the PCA

``` r
#screeplot helps you to visualize your PC
screeplot(pca2,main = NULL)

screeplot(pca2,type = "l",main = NULL)#provides different types of visualization for the variations of each PC
```

![](images/screeplot1.png)

![](images/screeplot2.png)

What we wish to see, and what we often do observe, is that the first two principal components account for the majority of the variance in the data.

``` r
#Suppose we want the y-axis to represent the proportion of variance in the data.

pca_var<-pca2$sdev^2#accounts for the variation in each PC account

pca_var_perc<- round(pca_var/sum(pca_var)*100,1)#transform to the percent

barplot(pca_var_perc, xlab = "PC",ylab = "percent variation") #use the barplot to plot
```

![](images/screeplot3.png)

Do you agree that this is a colossal waste of time? Using the factoextra package will make your life much easier!

``` r
library(ggplot2)
library(factoextra)
fviz_eig(pca2)#generates the scree plot
```

![](images/screeplot4.png)

Let's plot the principal component analysis (PCA), the `bioplot()` function may assist you in plotting the PCA,

``` r
biplot(pca2)#plot the PCA
```

![](images/pcaplot1.jpeg)

you can observe various species arrows.In the PCA plot, the arrows indicate the variables, and in brief, species arrows with small angles to an axis are highly correlated with that axis, and the points that are closer to the axis are more affected by that variable.

<a name="p"></a>

## Advanced PCA visualisation

Now that you are familiar with the procedure for producing PCA plots, I'm going to go a step further using the `ggbiplot` package. `ggbiplot` is a ggplot2-based package, which means you can use many of the same methods to make your graph look even better. you can install the following packages by running

``` r
install.packages("devtools")
install_github("vqv/ggbiplot")
library(devtools)
library(ggbiplot)
```

Now, let's plot the PCA by using ggbiplot

``` r
ggbiplot(pca2) #plots the PCA
```

![](images/pcaplot2.png)

We can also add the circle argument to our plot. What does this circle describe here?

``` r
pca_plot2<-ggbiplot(pca2,varname.size = 5,#sets the size of the variable name
         circle = T)+#add the circle to your plot
  geom_point(color = "red")
```

![](images/pcaplot3.png)

It provides an indication of the degree to which this PCA accurately describes each individual variable. When we refer to the description, what we mean is that the principal component analysis (PCA) explains 89% of the variation, and the longer the arrow, the more variance there is inside that arrow.That indicates that it is most of the variation is adequately explained. This indicates all of the other vectors are accounted for by 89% of the variance, with the exception of drat, in our plot.

Okay, at this point, we are going to count the number of cylinders and visualise the patterns that they form. You will also need to change the value of the `ellipse` argument so that it reads TRUE. This will cause an ellipse to be drawn around each group.

``` r
            ggbiplot(pcobj = pca2,
                     choices = c(1,2),#sets the principle components you need
                     obs.scale = 1, var.scale = 1,#scales the factor to apply to variables and observations
                     labels = row.names(data),#label the points with row name
                     varname.size = 6,
                     varname.abbrev = F,#scales the factor to apply to variables and observations
                     var.axes = F,#draws arrows for the variables
                     circle = F,# draws a correlation circle
                     ellipse = T,groups = data$cyl)#draws a normal data ellipse for each group
```

![](images/pcaplot4.png)

Now, we want to know the origin of automobiles as well as the degree to which they share characteristics. `ggplot2`'s basic functions allow us to add a legend and title to our plot, and the `rep()` function is what facilitates the replication of the automobiles.

``` r
groups <- c(rep("Asian", 3), rep("American",4), rep("EU", 7),rep("American",3), "EU", rep("Asian", 3), rep("American",4), rep("EU", 3), "American", rep("EU", 3))
#groups the cars according to their regions
         ggbiplot(pcobj = pca2,
                     choices = c(1,2),
                     obs.scale = 1, var.scale = 1,
                     labels = row.names(data),
                     varname.size = 6,
                     varname.abbrev = F,
                     var.axes = F,
                     circle = TRUE,
                     ellipse = T,groups = groups)+
  ggtitle("PCA of mtcars")+
  theme(legend.position = "bottom",
        panel.background = element_blank(),#removes the background
        axis.line = element_line(colour = "black"))+
  scale_colour_manual(name="country", values= c("#FFF68F", "#0000FF", "#FF3030"))#changes the color of the circles
```

![](images/pcaplot5.png)

so far so good, you have learned all of the essentials of PCA and have significantly improved your ability to visualize PCA plots. Are you perhaps still not satisfied with your achievement? Let's combine the initial portion of our handwork and complete the rest of the fantastic tutorial using ggplot! First, we will need to construct a data frame that will hold our PC1 through PC7.

``` r
pca_ggplot_data<-data.frame(pca2$x)
pca_ggplot_data$plotx<-pca_ggplot_data[,1]#assigns the first column as plotx
pca_ggplot_data$ploty<-pca_ggplot_data[,2]#assigns the first column as ploty
pca_ggplot_data$car <- rownames(pca_ggplot_data)#creates a column called car, which indicates row names
```

`coord_fixed()`ratio = 1, ensures that one unit on the x-axis is the same length as one unit on the y-axis. `stat_ellipse()` plots data ellipses, and geom draws a polygon. Similarly, you should use `after_scale()` to flag the evaluation of mapping after data has been scaled

``` r
ggplot(pca_ggplot_data, aes(x=plotx, y=ploty,color=groups,label = car))+
  geom_point()+
  coord_fixed(ratio = 1)+#controls the x and y ratio
  stat_ellipse(geom="polygon", aes(fill = after_scale(alpha(color,0.4))))+#plots data ellipses
  geom_label_repel(fill = "white")+
  theme(legend.position = "bottom",
        plot.title = element_text(hjust = 1, face = "bold", size = (15)))+
  xlab("PC1(72.7% explained var.)")+ylab("PC2(16.5% explained var.)")+
  ggtitle("PCA of mtcars dataset in 3 regions")
```

![](images/pcaplot6.jpeg)

This is a bit messy, is it? Let's attempt to identify some of the important automobiles that we wish to learn more about.

``` r
cars2<- c("Lotus Europa", "Toyota Corona","Merc 280","Hornet Sportabout", "Merc 450SL","Merc 230","Chrysler Imperial")
ggplot(pca_ggplot_data, aes(x=plotx, y=ploty,color=groups,label = ifelse(car %in% cars2, car, "")))+
  geom_point()+
  coord_fixed(ratio = 1)+
  stat_ellipse(geom="polygon", aes(fill = after_scale(alpha(color,0.4))))+
  geom_label_repel(fill = "white",fontface ="bold",show.legend = F,
                   point.padding = 0.2,  #provides additional padding around each point
                   nudge_x = .16,
                   nudge_y = .4,
                   segment.linetype = 1,
                   segment.curvature = -1e-20,
                   arrow = arrow(length = unit(0.02, "npc"), type = "closed", ends = "first"))+
  theme(legend.position = "bottom",
        panel.background = element_blank(),
        axis.line = element_line(colour = "black"),
        plot.title = element_text(hjust = 1, face = "bold", size = (15)),
        plot.margin = unit(c(1,1,1,1), units = , "cm"))+
  coord_cartesian(xlim =c(-6,6), ylim = c(-3, 3))+ #sets the x and y lim
  xlab("PC1")+ylab("PC2")+
  ggtitle("PCA of MTcars")
```

![](images/pcaplot7.jpeg)

<a name="q"></a>

## Summary

You've accomplished all that needed to be completed for this tutorial! When doing data analysis, we often wish to enhance the appearance of our graphs by making them look more sophisticated; yet, sometimes, less is more. In the first section, we began by labelling points on a graph to make the comparison between the two variables more evident. Then, we discovered that when we were looking at multiple variables simultaneously, it was difficult to analyze them in a conventional manner. This is the point when we apply principal component analysis (PCA), a technique of ordination that decreases dimensionality while keeping its essential properties.

<a name="r"></a>

## Challenge

If the exercise you did today wasn't too challenging for you, it's time to step up the intensity! - Try changing the size of the dots and moving the text label to one side. - In part 2, we mentioned eigenvalue and eigenvector, but we did not provide an in-depth explanation of each term. Could you attempt to clarify what it is that they stand for in PCA? - If you want to explore the relationship between the explanatory variable and the response variable, you would need to use another analysis of ordination (Hint: RDA).

<hr>

<hr>

#### Check out our <a href="https://ourcodingclub.github.io/links/" target="_blank">Useful links</a> page where you can find loads of guides and cheatsheets.

#### If you have any questions about completing this tutorial, please contact us on [ourcodingclub\@gmail.com](mailto:ourcodingclub@gmail.com){.email}

#### <a href="INSERT_SURVEY_LINK" target="_blank">We would love to hear your feedback on the tutorial, whether you did it in the classroom or online!</a>

<ul class="social-icons">

<li>

<h3><a href="https://twitter.com/our_codingclub" target="_blank"> Follow our coding adventures on Twitter! <i class="fa fa-twitter"></i></a></h3>

</li>

</ul>

###   Subscribe to our mailing list:

::: container

:::
