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

Debugging process:
1) Run the code
2) Analyze the error message
3) Attempt to selectively modify the code 
4) Repeat

Using my process as above, the error message I obtained after running the code was

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
Hence, the final bug is: Return statement being on same line as for loop.

The final bug-free code is:

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

This assignment taught me debugging procedures used in R and was quite fun!





