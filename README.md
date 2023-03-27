# Assignment11
## Sriya Reddy 

In this assignment, we were given a functon with a deliberate bug inserted and we were asked to find the bug and resolve it. 

The function given is the following:

```
tukey_multiple <- function(x){
    outliers <- array(TRUE, dim = dim(x))
    for (j in 1:ncol(x))
    {
        outliers[,j] <- outliers[,j]&&tukey.outlier(x[,j])
    }
    outlier.vec <- vector(length = nrow(x))
    for (i in 1:nrow(x))
    { outlier.vec[i] <- all(outliers[i,]) } return(outlier.vec)}
```

Reading the function, it looks like a function to read an array and find the outliers in the array. 
For this, it uses the Tukey method, which is: 
Outliers are those datapoints which are at a distance of more than 1.5 times the inter-quartile range from the first quartile and the third quartile. 

The error message I obtained after running the code was

```
Error: unexpected symbol in:
"    for (i in 1:nrow(x))
    {oulier.vec[i] <- all(outliers[i,])}return"
```

I thought that this was due to a misspelled variable, but all the variables are correctly defined and spelt. 
I then tried to see if the whitespaces were causing the issue, but even after inserting white spaces, the code still threw the same error. 
Looking more carefully through the code, I saw that the first for loop was written in multiple lines with indentation, but the second for loop was written in one line, with the return statement in the same line. I separated the lines of the second for loop and provided indentation as in the first for loop. The code now looked like 

```
> tukey_multiple <- function(x){
+     outliers <- array(TRUE, dim = dim(x))
+     for (j in 1:ncol(x))
+     {
+         outliers[,j] <- outliers[,j]&&tukey.outlier(x[,j])
+     }
+     outlier.vec <- vector(length = nrow(x))
+     for (i in 1:nrow(x))
+     { 
+         outlier.vec[i] <- all(outliers[i,]) 
+     } 
+     return(outlier.vec)}
> tukey_multiple <- function(x){
+     outliers <- array(TRUE, dim = dim(x))
+     for (j in 1:ncol(x))
+     {
+         outliers[,j] <- outliers[,j]&&tukey.outlier(x[,j])
+     }
+     outlier.vec <- vector(length = nrow(x))
+     for (i in 1:nrow(x))
+     { outlier.vec[i] <- all(outliers[i,]) } 
+     return(outlier.vec)}
```

Running this code, I found that the error message now disappeared. 
To find which line caused the bug, I reversed the indentation sequentially and found that the return statement being on the same line as the for loop had caused the bug. Probably it was being implemented every iteration of the loop. 

Hence, the bug is: Return statement being on same line as for loop.

The bug-free code is:

```
> tukey_multiple <- function(x){
+     outliers <- array(TRUE, dim = dim(x))
+     for (j in 1:ncol(x))
+     {
+         outliers[,j] <- outliers[,j]&&tukey.outlier(x[,j])
+     }
+     outlier.vec <- vector(length = nrow(x))
+     for (i in 1:nrow(x))
+     { 
+         outlier.vec[i] <- all(outliers[i,]) 
+     } 
+     return(outlier.vec)}
> tukey_multiple <- function(x){
+     outliers <- array(TRUE, dim = dim(x))
+     for (j in 1:ncol(x))
+     {
+         outliers[,j] <- outliers[,j]&&tukey.outlier(x[,j])
+     }
+     outlier.vec <- vector(length = nrow(x))
+     for (i in 1:nrow(x))
+     { outlier.vec[i] <- all(outliers[i,]) } 
+     return(outlier.vec)}
```

According to the lecture, we need to also create a test case and verify that the function is running correctly. 
For this, I created a test case as follows. 

```
> array_test <- matrix(c(2, 3, 100, 5, 2, 4), nrow = 6, ncol = 6)
```

Running the test case with the debugged function, I encountered the following error. 

```
> tukey_multiple(array_test)
Error in tukey.outlier(x[, j]) : could not find function "tukey.outlier"
```

This error tells me that the function `tukey.outlier` is not defined. 
I looked up this function and found the function definition. 

```
> tukey.outlier <- function(x) {
+     quartiles <- quartiles(x)
+     lower.limit <- quartiles[1]-1.5*quartiles[3]
+     upper.limit <- quartiles[2]+1.5*quartiles[3]
+     outliers <- ((x < lower.limit) | (x > upper.limit))
+     return(outliers)
+ }
```

However, this `tukey.outlier` function requires another function to be defined: `quartiles`
I looked up this function as well and found the following. 

```
> quartiles <- function(x) {
+     q1<-quantile(x,0.25,names=FALSE)
+     q3<-quantile(x,0.75,names=FALSE)
+     quartiles <- c(first=q1,third=q3,iqr=q3-q1)
+     return(quartiles)
+ }
```

The function `quantile()` exists in R, hence all the functions are now defined. 
After prviding the function definitions for `tukey.outlier()` and `quartile()`, I ran `tukey_multiple()` and obtained:

```
> quartiles <- function(x) {
+     q1<-quantile(x,0.25,names=FALSE)
+     q3<-quantile(x,0.75,names=FALSE)
+     quartiles <- c(first=q1,third=q3,iqr=q3-q1)
+     return(quartiles)
+ }
> tukey.outlier <- function(x) {
+     quartiles <- quartiles(x)
+     lower.limit <- quartiles[1]-1.5*quartiles[3]
+     upper.limit <- quartiles[2]+1.5*quartiles[3]
+     outliers <- ((x < lower.limit) | (x > upper.limit))
+     return(outliers)
+ }
> tukey_multiple(array_test)
[1] FALSE FALSE FALSE FALSE FALSE FALSE
```

This shows me that the `tukey_multiple()` function is now debugged correctly and is running without any issue on a test case. 

This assignment taught me debugging procedures used in R and was quite fun!





