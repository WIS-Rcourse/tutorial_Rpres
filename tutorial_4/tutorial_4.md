Tutorial # 4
========================================================
author: Rstaff_2021
width: 1980
height: 1020
date: 19/Nov/2020
autosize: T

RECAP
=========
## Vectors

- Vectors are 1 dimensional data containers.
- Can only contain elements of the same type.

**Character**

```r
c("This", "is", "a", "character", "vector")
```

```
[1] "This"      "is"        "a"         "character" "vector"   
```

**Numeric**

```r
1:10
```

```
 [1]  1  2  3  4  5  6  7  8  9 10
```

---

**Logical**

```r
c(TRUE, FALSE, F, T, F)
```

```
[1]  TRUE FALSE FALSE  TRUE FALSE
```

**Factor** A categorical vector. Is built on top of a vector of integers, and 
their elements are limited by the levels defined at their creation.

```r
fct <- factor(c("sick", "healthy", "sick"))
fct[2] <- "maybeSick"
fct
```

```
[1] sick <NA> sick
Levels: healthy sick
```

```r
fct[2] <- "healthy"
fct
```

```
[1] sick    healthy sick   
Levels: healthy sick
```

RECAP
======
## Matrices (and arrays)

- Matrices are 2 dimensional data containers.
- They can only contain one type of values as vectors.


```r
mat1 <- matrix(data = 1:9, nrow = 3, ncol = 3, byrow = TRUE)
mat1
```

```
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6
[3,]    7    8    9
```

```r
mat2 <- matrix(data = 1:9, nrow = 3, ncol = 3, byrow = FALSE)
mat2
```

```
     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9
```

---

- Arrays are the multidimensional versions of matrices, but are rarely used.


```r
array(data = 1:3, dim = c(1,2,2))
```

```
, , 1

     [,1] [,2]
[1,]    1    2

, , 2

     [,1] [,2]
[1,]    3    1
```

- As of R 4.0, when we check the class of matrices, they will be considered 
arrays as well as matrices.


```r
class(mat1)
```

```
[1] "matrix" "array" 
```

RECAP
============

## Cohersion

If objects of different types are combined in an object that cannot 
contain different types (vectors or matrices), they will be coersed 
(converted by force) to a single type in the following hierarchy.

Character < Double (More flexible numeric) < Integer < Logical

Logical + Integer

```r
c(c(TRUE, FALSE, TRUE), 5:7)
```

```
[1] 1 0 1 5 6 7
```

Logical + Character

```r
c(c(T, F, T), "Hello")
```

```
[1] "TRUE"  "FALSE" "TRUE"  "Hello"
```

---

Numeric + Character

```r
rbind(1:5, c("a", "b", "c"))
```

```
     [,1] [,2] [,3] [,4] [,5]
[1,] "1"  "2"  "3"  "4"  "5" 
[2,] "a"  "b"  "c"  "a"  "b" 
```

Factors cannot be combined, they will be coerced to their numeric version

```r
c(factor(c("sick", "healthy")), factor(c("sick")))
```

```
[1] 2 1 1
```

- Useful cases of coersion are the averaging or summing of logical vectors.


```r
mean(c(TRUE, TRUE, FALSE, TRUE))
```

```
[1] 0.75
```

RECAP
======

## Dataframes

- Dataframes can contain different classes of vectors, **each per column**.

```r
toyDF <- data.frame(numbers = 1:3, 
                    charact = letters[1:3], 
                    factors = factor(c("sick", "healthy", "sick")),
                    logical = c(T, F, T))
toyDF
```

```
  numbers charact factors logical
1       1       a    sick    TRUE
2       2       b healthy   FALSE
3       3       c    sick    TRUE
```

- BUT the number of elements per column **must be the same.**

```r
data.frame(numbers = 1:3,
           longer = 1:10)
#> Error in data.frame(numbers = 1:3, longer = 1:10) : 
#  arguments imply differing number of rows: 3, 10
```

---
Elements in **dataframes** can be retrieved and assigned using:

- `$` (peso/dolar sign) creating new columns **by name**.


```r
toyDF$newCol <- NA
toyDF
```

```
  numbers charact factors logical newCol
1       1       a    sick    TRUE     NA
2       2       b healthy   FALSE     NA
3       3       c    sick    TRUE     NA
```

- `[ , ]` (square-brackets). 
Separated by commas the first vector indicates the rows indices, while the second
indicates the columns indices we want to retrieve or assign.


```r
toyDF[c(1, 3),"newCol"] <- 5
toyDF
```

```
  numbers charact factors logical newCol
1       1       a    sick    TRUE      5
2       2       b healthy   FALSE     NA
3       3       c    sick    TRUE      5
```

Lists
=================
- Lists are a data structure that can contain elements of different classes
with **any length**.

- Lists can be created using the `list()` function, which takes an arbitrary
number of arguments as its elements.


```r
toyList <- list(numbers = 1:10, 
                smallMat = matrix(1:9, 3, 3))
toyList
```

```
$numbers
 [1]  1  2  3  4  5  6  7  8  9 10

$smallMat
     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9
```

---

- To **store and retrieve** elements from lists we use the `$` (peso/dolar 
sign) and the `[[]]` (double square-brackets). 

- **`[[ ]]`** works to retrieve and store elements by **index** OR **name**.


```r
toyList[["smallMat"]]
```

```
     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9
```


```r
toyList[[2]]
```

```
     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9
```

Lists (cont.)
=================

- **`$`** works to store or retrieve **ONLY named** elements in the list.


```r
toyList$aList <- list(a = 1:5,
                      b = letters[1:5])
toyList
```

```
$numbers
 [1]  1  2  3  4  5  6  7  8  9 10

$smallMat
     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9

$aList
$aList$a
[1] 1 2 3 4 5

$aList$b
[1] "a" "b" "c" "d" "e"
```

---

- If we want to retrieve an element in a list within another list we would use 
several `$` until reaching the element we want.


```r
toyList$aList$b
```

```
[1] "a" "b" "c" "d" "e"
```

- **Importantly** using single brackets can subset lists but elements will be
kept inside a list.


```r
toyList[c(1,3)]
```

```
$numbers
 [1]  1  2  3  4  5  6  7  8  9 10

$aList
$aList$a
[1] 1 2 3 4 5

$aList$b
[1] "a" "b" "c" "d" "e"
```


FOR loops 
=============
In computer science, a for-loop (or simply for loop) is a **control flow** 
statement for specifying **iteration**, which allows code to be executed 
repeatedly [REF](https://en.wikipedia.org/wiki/For_loop).

A FOR loop has two parts: 
* A header specifying the iterate and vectors to be iterated
* A body which is executed once per iteration. 

In R the iterate (commonly referred as 'i') is specified as each
element IN a object, whose length determines the number of iterations to
be performed by the loop, AND the elements of the object to iterate through
is commonly also used as a variable in the code of loop's body.


```r
for(i in 1:5){
  print(2^i)
}
```

```
[1] 2
[1] 4
[1] 8
[1] 16
[1] 32
```

---

## Looping in dataframes and lists

If we use a dataframe or a list as the vector to iterate through, the FOR loop
will use its entire elements as **iterate**, columns in DF.


```r
DF1 <- data.frame(mat1)
for(i in DF1){
  print(sum(i))
}
```

```
[1] 12
[1] 15
[1] 18
```


```r
for(i in toyList){
  print(class(i))
}
```

```
[1] "integer"
[1] "matrix" "array" 
[1] "list"
```

Storing FOR loop results
========================

- A **FOR** loop runs code iteratively but doesn't store values in an object,
we have to execute that explicitly. But to store a value in an object, first
we need to create an empty object. 
- We can create an empty object with the `NULL` reserved word.
- Then at the end of the loop we will save our result to an index in the 
"results" object.

---

**Exercise**: Let's make a FOR loop that prints out the multiplication table
of 5 from 1 to 10 as: "1 times 5 equals 5",  "2 times 5 equals 10", 
"3 times 5 equals 15", ...etc


```r
RES <- NULL
for (i in 1:10){
  r <- i * 5
  RES[i] <- paste(i, "times 5, equals", r)
}
RES
```

```
 [1] "1 times 5, equals 5"   "2 times 5, equals 10"  "3 times 5, equals 15" 
 [4] "4 times 5, equals 20"  "5 times 5, equals 25"  "6 times 5, equals 30" 
 [7] "7 times 5, equals 35"  "8 times 5, equals 40"  "9 times 5, equals 45" 
[10] "10 times 5, equals 50"
```


Lists and FOR loop
=============
- To store values with a FOR loop in a list you have to initialize a list 
object, and use the double square-brackets `[[]]` to store the value in the
list. 


```r
RES <- list()
for(i in vector){
  BODY
  RES[[i]] <- OUTPUT
}
```

---
## Now practice
Transform the previous FOR loop so that the final object is a LIST instead
of a vector.


```r
RES <- NULL
for (i in 1:10){
  r <- i * 5
  RES[i] <- paste(i, "times 5 equals", r)
}
RES
```

```
 [1] "1 times 5 equals 5"   "2 times 5 equals 10"  "3 times 5 equals 15" 
 [4] "4 times 5 equals 20"  "5 times 5 equals 25"  "6 times 5 equals 30" 
 [7] "7 times 5 equals 35"  "8 times 5 equals 40"  "9 times 5 equals 45" 
[10] "10 times 5 equals 50"
```

Answer
=========

```r
RES <- list()
for (i in 1:5){
  r <- i * 5
  RES[[i]] <- paste(i, "times 5 equals", r)
}
RES
```

```
[[1]]
[1] "1 times 5 equals 5"

[[2]]
[1] "2 times 5 equals 10"

[[3]]
[1] "3 times 5 equals 15"

[[4]]
[1] "4 times 5 equals 20"

[[5]]
[1] "5 times 5 equals 25"
```

---

apply functions
=================

The "apply" family functions is a collection of functions that as FOR loops 
will execute code repeatedly.

- Apply functions run a function defined in its `FUN` argument. Commonly 
stated as *function(x){CODE}*, which will in turn indicate that *x* is 
the element that will iteratively **change**, similar to **i** in FOR loops.
- But they differ from FOR loops in that they will output an object, not
requiring initialization, neither explicit assignment to store values calculated
in the iterations.

- **apply()**: Apply function to margins of an array or matrix.
- **lapply()**: Apply over a vector and returns a list.
- **sapply()**: A 'user-friendly' version and wrapper of lapply()
- **tapply()**: Apply a function over groups of elements of vectors.

----


apply()
=========
Is a function that iterates specifically over a matrix along one
of its **margins**, applying a defined function and outputs a **vector**.

Now let's use apply() to get the average of each row of a *testMatrix*

```r
testMatrix <- matrix(1:100, 10, 10, byrow = T)
apply(X = testMatrix,
      MARGIN =  1,
      FUN =  function(x){
        mean(x)
      })
```

```
 [1]  5.5 15.5 25.5 35.5 45.5 55.5 65.5 75.5 85.5 95.5
```

---

Exercise:
Now use the apply() function to get the median over each column of *testMatrix*

Answer
========

```r
apply(X = testMatrix,
      MARGIN =  2,
      FUN =  function(x){
        median(x)
      })
```

```
 [1] 46 47 48 49 50 51 52 53 54 55
```

---

lapply()
=========
Is a function that iterates over the elements of its first input,
applying a defined function over it. The input can be a vector or a list, but the
output will always be a list in which each element is the result of each iteration.

Example:

```r
lapply(1:5, function(x){
  2^x
})
```

```
[[1]]
[1] 2

[[2]]
[1] 4

[[3]]
[1] 8

[[4]]
[1] 16

[[5]]
[1] 32
```

---
## "Simplifying" output

- If we want to convert the ouput elements to a vector we can use the unlist()
function.


```r
RES2 <- lapply(1:5, function(x){
  2^x
})
unlist(RES2)
```

```
[1]  2  4  8 16 32
```

- If we want to create a table out of the resulting elements in a list, we 
can use `do.call()`, along `cbind()` or `rbind`, to bind vectors as columns
or as rows respectively.


```r
do.call(what = cbind, args = RES2)
```

```
     [,1] [,2] [,3] [,4] [,5]
[1,]    2    4    8   16   32
```


Exercise
=====

Now take the last FOR loop that we used to create a list, and convert it
into an lapply()


```r
RES <- list()
for (i in 1:5){
  r <- i * 5
  RES[[i]] <- paste(i, "times 5 equals", r)
}
RES
```

```
[[1]]
[1] "1 times 5 equals 5"

[[2]]
[1] "2 times 5 equals 10"

[[3]]
[1] "3 times 5 equals 15"

[[4]]
[1] "4 times 5 equals 20"

[[5]]
[1] "5 times 5 equals 25"
```
---


Answer
=====


```r
RES_lapply <- lapply(1:5, function(i){
  r <- i * 5
  return(paste(i, "times 5 equals", r))
})
RES_lapply
```

```
[[1]]
[1] "1 times 5 equals 5"

[[2]]
[1] "2 times 5 equals 10"

[[3]]
[1] "3 times 5 equals 15"

[[4]]
[1] "4 times 5 equals 20"

[[5]]
[1] "5 times 5 equals 25"
```

sapply()
=========
Is a more "user-friendly" function than *lapply()* as it will try to output
a *simplified* version of the output.

What happens if you take the last `lapply()` and convert it to an `sapply()`?


```r
RES_sapply <- sapply(1:5, function(i){
  r <- i * 5
  return(paste(i, "times 5 equals", r))
})
RES_sapply
```

```
[1] "1 times 5 equals 5"  "2 times 5 equals 10" "3 times 5 equals 15"
[4] "4 times 5 equals 20" "5 times 5 equals 25"
```

Instead of a list, the output is a vector!

- Now, What happens if the output of each iteration is greater than 1, but of
the same length?

---


```r
resSurprise <- sapply(1:5, function(x){
  c(x, x+2)
})
resSurprise
```

```
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]    3    4    5    6    7
```
The output is a matrix!!

- What if the outputs has different lenghts?

```r
sapply(1:3, function(x) rep(1, x))
```

```
[[1]]
[1] 1

[[2]]
[1] 1 1

[[3]]
[1] 1 1 1
```
The output is a list...  :(

sapply() Final notes
====================

## **WARNING!!:** 

- sapply() can be useful but its output can be unpredictable 
if the function has different outputs on different scenarios. It is always better to have full control using lapply(), and then "simplify" the output into another
data structure as a matrix.

- If the output of lappply() is not compatible with further steps this will
pop a error that can then be debugged. But using sapply will give an output 
which was not expected BUT still work, which make harder to detect errors
and debugging.

---


tapply()
==========

Is a function that will not iterate through **individual** values,
but over **groups** of values specified by a **factor**, or grouping variable.

In this case all values per group will take the place of **x** when we define 
a function in FUN as **function(x){}**

- If the output of each iteration of `tapply()` is of length 1, it will output
an array of 1 dimension. In practice a simple vector.


```r
tmp <- tapply(iris$Petal.Length, iris$Species, function(x){
  mean(x)
})
tmp
```

```
    setosa versicolor  virginica 
     1.462      4.260      5.552 
```

---

- If the output of each iteration of `tapply()` is of length greater than 1,
it will output a LIST (similar to `lapply()`).


```r
tmp <- tapply(iris$Petal.Length, iris$Species, function(x){
  c(mean(x), median(x))
})
tmp
```

```
$setosa
[1] 1.462 1.500

$versicolor
[1] 4.26 4.35

$virginica
[1] 5.552 5.550
```

```r
do.call(tmp, what = cbind)
```

```
     setosa versicolor virginica
[1,]  1.462       4.26     5.552
[2,]  1.500       4.35     5.550
```


Exercise
=========
Calculate the median of Petal widths by species

----

Answer


```r
tapply(iris$Petal.Width, iris$Species, function(x) median(x))
```

```
    setosa versicolor  virginica 
       0.2        1.3        2.0 
```


Weekly Exercise
===============
For the Weekly exercise we will use the following worldrankings dataset

You can download it using the following 
[LINK](https://drive.google.com/uc?export=download&id=1yh4Iw3ec1rijo_5d53IL5RBu_DLEMYj0) 
or in the DATASETS folder in the google Drive folder.

As this is an Rdata object, we can easily load it using the function load() when
the data object is in our working directory.

To get the working directory use the 'getwd()' function



```r
getwd()
```

```
[1] "G:/My Drive/Science/PhD/Courses/Assisting/Rcourse_2021/myStuff/tutorial_Rpres/tutorial_4"
```

```r
load("worldRankings.Rdata")
worldrankings[1:5,1:5]
```

```
               country          subregion region smartphone_adoption happiness
1 United Arab Emirates       Western Asia   Asia                73.8     7.039
2          South Korea       Eastern Asia   Asia                73.0     6.267
3         Saudi Arabia       Western Asia   Asia                72.8     6.480
4            Singapore South-Eastern Asia   Asia                71.7     6.849
5               Norway    Northern Europe Europe                67.5     7.522
```

Further reading
==============

- [Advanced R - Hadley Wickham, Second edition](https://adv-r.hadley.nz/vectors-chap.html)

- [R for Data Science by Hadley Wickham and Garrett Grolemund](https://r4ds.had.co.nz/index.html)

---
