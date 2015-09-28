# Computing in R - Practical 2

SBC361: Research Methods and Communication

October 2015

In this session we will build on some of the skills learned in the previous practical, and will move on to consider some more advanced ways of controlling program flow. Today we will be exploring loops and functions, two important features of R that are also present in most programming languages, as well as doing a few exercises with DNA/RNA strings. As before, you will want to refer frequently to the help files and your own notes when exploring these new concepts.

To get your brains warmed up, here a a few questions on the material from last week:

#### Intro Q1. What would be the outcome of the code my.matrix <- diag(5)? Then what would be the outcome of the code colSums(my.matrix)?
#### Intro Q2. Load the data Indometh. Subset this data to return only those fields for which the concentration is between 1 and 2? What is the average (mean) growth rate for this subset?
#### Intro Q3. What is the regular expression code that corresponds to one or more copies of the letter "e", followed by one or more copies of the letter "z"?

## Functions

Functions are pieces of code that are made to take an input (generally known as arguments), do something with it, and give back an output. They are interesting because they allow you to run the same piece of code multiple times without having to write it entirely every time you need to run it.

To take a self-explanatory example:
```R
x      <- c(2,3,4,5)
x.mean <- mean(x)
```
We just used the `mean()` function, one of many functions loaded by default in R. The point of this function is that you can calculate the mean of any vector without explicitly writing the formula for the mean each time. The interesting thing about R is that it is possible to create your own functions. Let's create our own function to calculate the mean:

```R
## Define function named myMean

myMean <- function(my.vector) {
  ## Calculate the mean
  my.mean <- sum(my.vector)/length(my.vector)

  ## Return the calculated mean
  return(my.mean)
}

## Now call the function on some data:
x      <- c(2,3,4,5)
x.mean <- myMean(x)
```

The syntax `functionName <- function(parameter) {code}` is the most common way of defining a function. Remember to add the `return()` bit, otherwise the function will compute the code but it won't return anything!

Now take a look at the following code. This code is designed to take a number in seconds, and convert it into hours, minutes and remaining seconds (don't worry too much about the computation in the middle):

```R
# Input raw number of seconds
number.of.seconds <- 19955

# Convert number.of.seconds into hours, minutes, and seconds
hours |  | <- floor(number.of.seconds /(60*60))
minutes | <- floor((number.of.seconds -hours*(60*60))/60)
seconds | <- number.of.seconds -((hours*60)+minutes)*60

# Create a single vector containing all three quantities
outputvec <- c(hours, minutes, seconds)

# Output the solution to the console
outputvec
```

#### Q1. Modify the code above to make it into a function called `timeConverter()`. Can you run it on the numbers 5s, 50000s and 10000000s? Remember to indent any code inside the curly bracket.

#### Q2. Write your own function for converting distances between different units. Your function should take the distance in kilometres as input, and return the distance in miles as output (8 kilometres is roughly equal to 5 miles). Remember to clearly annotate your code, and make appropriate use of white space.

### More complex functions
Hopefully you can already see how functions can be very useful things. We can make them even more useful by considering some simple extensions.

First, you can define multiple arguments to a function, with the general syntax of a function being:

```R
my_function <- function(argument1, argument2, ...) {
    #code including argument1, argument2, ...
}
```

#### Q3. To understand how a function can have multiple arguments, modify the function `timeConverter()` so that adds a given number of hours to the result. You will have to include a new parameter (you can call it `additional.hours`), and add it to the `hours` variable.

Note that functions do not have to take single numbers as input. They can take vectors, matrices, data frames, or any other type of object, and they can also take character and logical data as well as numerical.

#### Q4. Make a function takes a vector of words as input, and outputs the number of characters in the longest word. Hint: you are going to need to find out how R counts the number of characters in words and how it finds the maximum value in a vector - use Google!

#### Q5.A Extra arguments:
Another neat thing that we can do is set default values for our arguments. Have another look at the `timeConverter()` function you modified in Q9. Most of the time, you will probably want run it with `additional.hours` being 0. To do this, you can make `additional.hours=0` as a default. With default valus for arguments,function take the syntax `myFunction <- function(argument1, ..., argument2=default) {code including argument1, argument2, ...}`. The default argument generally goes in the end of the argument list.

#### Q6.B Make the `additional.hours` be defined as 0 by default in the `timeConverter()` function. Run it without defining `additional.hours` and defining it to different number of seconds.

#### Q7. This task is a bit more challenging! Go back to your function for converting kilometres to miles; make a copy with an appropriate new name. The new extended function you must create should:

* take a distance in kilometres and a time in minutes as input.
* convert the distance into miles, and the time into hours. It should then calculate a speed in miles per hour.
* return the speed in miles per hour as output.

All of the skills required to complete this task this are given above. Take your time, and approach this problem one step at a time.


## Loops
Imagine you need to run the function `timeConverter()` on different numbers. You could type `timeConverter()` many different types, each time with a different number. But now imagine you had to type that thousands of times. It would be impossible.

Fortunately, computers were built to perform same task over and over again many times. They do this using a construct called a *loop*. Although there are several types of loops, we are going to learn about the for loop. It works this way:

```R
## For loop exmaple
for (seconds in c(1000,2000,3000,4000,5000)) {
  time.in.hours <- timeConverter(number.of.seconds=seconds)
  print(time.in.hours)
}
```
The loop will run 3 times. Each time, it will define the variable `seconds` as a different number from the vector `(1000,2000,3000)`. It will compute a new time in hours and print it.


### Slightly more complex loops
There are some interesting ways in which we can stretch our understanding of loops. First of all, it is important to recognise that the values that we are indexing over can be anything that goes in a vector. The vector can be defined outside the loop definition line:

```R
## Non-sequential loop values
seq.values <- c(1:50)
for (i in loop.values) {
  print(i)
}

## The same as writing:
for (i in 1:50) {
  print(i)
}

## Sequential loop values
loop.values <- c(15,5,2.3,100,16)
for (index in loop.values) {
 | print(index)
}

## Character based loop values
loop.text <- c("Hey", "Hi", "Hello")
for (word in loop.text) {
 | print(word)
}
```

### Storing loop results

More often than not, you will not want to just print the loop results. Instead, you may want to keep them in a separate variable. Check the following example. What's the result from the 4th iteration?

```R
## Vector to loop through
practical.attribute.vec <- c("great", "boring", "very long", "informative", "here")

## Empty vector to keep loop results
phrase.vec <- c()

for (practical.attribute in practical.attribute.vec) {
  phrase     <- paste("This practical is", practical.attribute, collapse=" ")
  # Add the loop result to end of the vector of results
  phrase.vec <- c(phrase.vec, phrase)
}
```
A different way of approaching a for loop is to loop through the indexes of a vector, rather than the vector itself. In the following code, we also create a vector for the result with the same length as the vector we are looping through, and we use the index to 'populate' it:

```R
## Vector to loop through
practical.attribute.vec <- c("great", "boring", "very long", "informative", "here")

## Empty vector to keep loop results
phrase.vec <- rep(NA, length(practical.attribute.vec))

## Loop through the index rather than the vector
for (i in 1:length(practical.attribute.vec)) {

  ## Create phrase, getting attribute from practical.attribute.vec[i]
  phrase     <- paste("This practical is", practical.attribute.vec[i], collapse=" ")

  ## Add phrase to the right index of the results vector
  phrase.vec[i] <- phrase

}
```

Once you are comfortable with loops, have a go at the following tasks:

#### Q8A. Write a loop that iterates over the numbers 10 to 100, and prints out the index of the loop each time through.

#### Q9B. Write a loop that iterates over the numbers 10 to 100, and store the results to a separate vector. What's the number in the 20th iteration?

#### Q10A. Write a loop that iterates over the numbers 16 to 49, and prints out the square root of the index each time through (you may have to search around for the square root function).

#### Q10B. Make the last loop store the results to a separate vector called `sq.root.vec` instead of just printing the results. What's the value the 3rd iteration? What's the sum of the square roots of the numbers 16 to 49?

#### Q11. Write a loop that iterates over all even numbers between 30 and 90. Each time round evaluate the function timeconverter() on the indexed value, and store it to  separate vector.

#### Q12. Write a loop that calculates the product of all numbers from 1 to 10, and then subtracts the numbers 1 to 10 in turn.  i.e.  (product)-1, then (product)-2, followed by (product)-3…  etc).

### Nested Loops

Another important way of extending loops is to consider nested loops - in other words, loops within other loops! Have a look at the following code:
```R
for (i in 1:5) {
 | for (j in 7:9) {
 |  | print(c(i,j))
 | }
}
```

Here we have one loop (with index j) nested within another loop (with index i). I have also defined the values that i and j can take directly within the loops, rather than outside of the loops as in previous examples - this is simply a way of saving space. Evaluate this code and try to make sense of the output. Fiddle around with the different elements of this code until you are comfortable with nested loops. Warning - loops require your computer to perform many operations, and as such it is quite easy to crash R using loops. A simple block of code evaluated 100,000 times amounts to quite a big job. If you want to force R to exit a loop part way through, simply press Esc. Nested loops are particularly hazardous!

#### Q13. Create a nested loop. The outer loop should be over the words "Angry", "Lazy" and "Happy". The inner loop should be over the words "birds", "dogs", and "horses". The code inside the inner loop should print out a vector containing the indexes of both loops (for example "Angry" and "birds" in the first instance).

#### Q14. Write a third-degree nested loop. Be careful not to loop over too many values or you will crash R!

### Using loops to reformat a data set

Loops tend to be particularly useful to reformat data sets. By looping through all of the fields of a particular data set, and at each iteration dropping the relevant entry into a new data structure, it is possible to convert from one data format to another.

The data set that we will use in this example is typical of the sort of data that you might be faced with in the future. Load the data by running the following line of code:
```R
helianthus.data <- as.matrix(read.table("http://www.antgenomes.org/~yannickwurm/tmp/HelianthusData_num.txt ", header=T))
```

Each row in this data set represents a different strain of *Helianthus annuus* (sunflowers), grown under controlled conditions. The first column tells us the Strain (these are numbered 1 to 5). The remaining columns describe the number of plants found in the study area at six different points in time. For example, looking at the first row, we can see that strain 1 started out with 12 plants, but by the final time point contained 57 plants.

#### Q15 The aim of the following exercice is to get the data from the current format into what's generally called the 'long format'

We want to get this data into a new format - sometimes called long format - in which we have a matrix of three columns; the first column describes the strain, the second column describes the time point, and the third column describes the number of plants observed. The first few lines of this new data structure should look like this:

|Strain | Time | Count|
|-------|------|------|
|1 |0 |12 |
|1 |1 |33 |
|1 |2 |71 |
|1 |3 |61 |
|1 |4 |73 |
|1 |5 |57 |
|2 |0 |10 |
|2 |1 |27 |
...

We can make the transition from the wide format of helianthus.data to the long format described above using a nested loop. But first, let us create an empty matrix, which we will eventually fill with our new values.

#### Q15.a. Create an empty matrix, called `long.data`. This matrix must have 3 columns, and 30 rows (the number 30 comes from the fact that we have five strains at six time points each). Name the columns `c("Strain", "Time", "Count")`.

With this empty matrix created, we can move on to the next part of the problem - populating it with values. We want to look at each of the elements of helianthus.data one after the other, using a nested loop. The basic structure of this nested loop is as follows:
```R
# Loop through all rows
for (myrow in 1:5) {
	# Loop through all columns except the first
	for (mycol in 2:7) {

		# This is where the main code goes.

	}
}
```
Here we are using loops to index through each of the rows of the matrix `helianthus.data`, and for each row we are indexing through columns 2 to 7 (as these are the columns that contain relevant data). At any point in the two loops, the value that we are focusing on is given by `helianthus.data[myrow,mycol]`.

Hopefully you can already see that these are the exact values we want to drop into the third column of our matrix `long.data`. However, we are presented with a problem - how do we drop these values one after the other into the right place in the matrix` long.data`? We cannot use the index myrow to help us, as this only goes through values `1:5`. Similarly, we cannot use the `index mycol`, as this only goes through values `2:7`. What we really need is a new index that goes all the way from 1 to 30, irrespective of the row or column that we are focusing on.

#### Q15.b. Change the for loop above to include a variable `myindex`. This variable should be defined as being 0 before the loop starts. At every iteration of the inner loop, you should add 1 to it.

It should look something like this:
```R
# myindex defined as 0 before the loop starts
myindex <- 0

# Loop through all rows
for (myrow in 1:5) {
	# Loop through all columns except the first
	for (mycol in 2:7) {

		## Change myindex here:
    # make myindex equal to myindex plus one

    # If you want to check what you are doing so far,
    # remove the comment from the following line:
    # print(c(myindex,myrow,mycol))
	}
}
```

Now that we have three indices - one going through the rows of `helianthus.data`, one going through the columns of `helianthus.data`, and one simply going from 1 to 30 - we have all the ingredients we need to populate the matrix `long.data`. The code we need at each iteration of the loop is the following:
```R
# Get Count
long.data[myindex,3] <- helianthus.data[myrow,mycol]
```

This is fairly straightforward. We also want to drop the time point in the second column of long.data. Although we do not have a vector describing each of the time points, in fact the timings are very simply given by mycol minus two. For example, if we are looking at the fourth column then we are looking at the second time point. Therefore, we need the following line of code to extract the timings:
```R
# Get Time
long.data[myindex,2] <- mycol-2
```

Finally, we want to drop the strain type into the first column of long.data. The strain type is given by the first element in every row, meaning it is given by helianthus.data[myrow,1]. Therefore, we need the following line of code to extract the strain types:
```R
# Get Strain
long.data[myindex,1] <- helianthus.data[myrow,1]
```
#### Q15.c. Bring all of this together to finish the for loop, and run it!
It should look something like this:
```R

helianthus.data <- as.matrix(read.table("http://www.antgenomes.org/~yannickwurm/tmp/HelianthusData_num.txt ", header=T))

# Create an empty matrix
long.data           <- matrix(0, nrow=30, ncol=3)
colnames(long.data) <- c("Strain", "Time", "Count")

# myindex defined as 0 before the loop starts
myindex <- 0

# Loop through all rows
for (myrow in 1:5) {
	# Loop through all columns except the first
	for (mycol in 2:7) {

		## Change myindex here:
    # make myindex equal to myindex plus one

    ## Populate the long.data matrix
    long.data[myindex,1] <- helianthus.data[myrow,1] # Add Strain
    long.data[myindex,2] <- # Add Time
    long.data[myindex,3] <- # Add Count

    # If you want to check what you are doing so far,
    # remove the comment from the following line:
    # print(long.data[myindex,])
	}
}

# To check what you've done, you can print the start and end of long.data
head(long.data)
tail(long.data)
```

## Working with DNA data

#### Q16. Write a function that converts a short DNA sequence of 15 bases (eg ACCTGTCATCATCCC) to RNA, and splits the string into triplets. You will need to:

   		 1. replace T with U  (thymine with uracil to convert DNA to RNA)
        		 2. use `substring()` to split the sequence into triplets, and `seq()` within `substring()`
        		 3. return the RNA triplets string

Note `substring()` takes a "first" and "last" argument. The "first" would be `seq()` indicating where the beginning of your first triplet is and where the beginning of your last triplet is. The "last" argument would be `seq()` indicating where the end of your first triplet is, and the end of your last triplet.  In `seq()` you will also indicate you want triplets.  

As an example:
```R
dna.string<-c("AAATTT")
substring(dna.string, seq(1, 4, by=3), seq(3, 6, by=3))
```

Once you have created the function, see if you can modify the function to work for a sequence with any number of characters. Tip: you can use `nchar()` to create a variable such as `num.characters`.


#### Q17 (Bonus question). Write a function to obtain the reverse-complement of a DNA sequence.

You can use this sequence: `"ATTACGACGCGATTCCCGGTTAATCGAATTCCCA"`. As an example, the complement of ATGC is TACG.

The tricky part here is reversing a single string of characters. Search around for `strsplit`.  You will need to:
* replace bases with their complement
* split the string of DNA bases into separate characters with `strsplit`. Note that `strsplit` returns a list, so you will need to use `unlist()` to obtain a string
* reverse the sequence
* remove the spaces to get your single string of DNA bases
* return the reverse-complement sequence

#### Q18 (Hacker question). Translate your triplet string from Q12 into amino acids
Note: depending on your method, you may not need to convert it to RNA first.  You can use a codon-to-amino-acid table and code the translation yourself. Alternatively, you could  search around and load a specific package for the manipulation of biological sequences.

## Bringing it Together

Well done for making it this far! Now try to bring all of your knowledge together. Have another look over your previous notes, and make sure these ideas are still fresh in your mind. Have a go at writing your own scripts, preferably combining a number of different concepts (for example, you could try making a function that acts on a matrix, and then using this function within a loop). Another good idea would be to come up with a problem that you want to solve (either for fun or for other coursework), and then write a function that solves it.
Many of the ideas presented in this session and the last are designed to challenge you. Do not expect to understand them from a single read through the material. Rather, you will have to play around with many different examples and applications before the penny drops completely. On the plus side, structures such as loops and functions are common to almost all programming languages, and so once you understand these concepts the world of programming is your oyster!
