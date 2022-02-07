### [Languages] Data Manipulation using dplyr package in R
This tutorial will introduce basic dataframe manipulation and later expose you to a more professional way of doing data manipulation on a large scale. So, first, we will create a demo datafarme that will help us see how we can use elementary approaches to extract data we are interested in that dataframe.
### Creating a dataframe in R
Dataframe is a list of variables that may contain different data types but of the same length. First, to create a dataframe, we create a budge of variables we want to concatenate into a list. For example, the following are variables we created to use in our list. Note they are of the same length but different data types.

```r
Age = c(20, 21, 24, 26, 27, 20)
Gender = c("F", "M","F","M","F","F")
Course = c("BAS","BAS","BST","BAS","BST","BST")
Height = c(1.4, 1.3, 1.2, 1.0, 1.3, 1.4)

```
To create a dataframe using these variables, we will pass them in the `data.frame()` function. The code below carries out this task.

```r
my_data = data.frame(Age, Gender, Course, Height)
my_data

```
#### Output:

![dataframe](dataframe.png)

The dataframe was successfully created. We will try to use elementary techniques and extract some data of interest from this dataframe.

Suppose you want to extract a certain column from this dataset, say `Age`. Then you can do so using the following two commands.

```r
my_data$Age # dataset dollar sign followed by the column you want to extract

```
#### Output:

```bash
[1] 20 21 24 26 27 20

```

As we know, a column consists of all rows in the dataset. So we can extract all rows and specify that specific column we are interested in, i.e.,

```r
my_data[,1]

```
#### Output

```bash
[1] 20 21 24 26 27 20

```

Using the `attache()` function ones can column names of a dataset, i.e.,

```r
attach(my_data)

```
#### Output

```bash
The following objects are masked _by_ .GlobalEnv:

    Age, Course, Gender, Height

```
If a particular column is categorical data, we might want to know the count of each category. This is an everyday activity in machine learning where you check how balanced the study variable is. To check the count of each category in a variable, one can use the `table()` function. For example, suppose we're interested to know the count of each gender in the `Gender` column, then we can do this with a code similar to the one provided below.

```r
table(my_data[,2])

```
Running this program returns;

```bash
> table(my_data[,2])

F M 
4 2 
```
Lastly, one may want to know the basic statistics of the data they are working with. In R, we can run the data through the `summary()` function and get all basic statistics that can help us understand the data we are working with. Let's run our demo dataset through this function and see the output by doing.
```r
summary(my_data)

```
#### Output
```bash
> summary(my_data)
      Age           Gender             Course              Height     
 Min.   :20.00   Length:6           Length:6           Min.   :1.000  
 1st Qu.:20.25   Class :character   Class :character   1st Qu.:1.225  
 Median :22.50   Mode  :character   Mode  :character   Median :1.300  
 Mean   :23.00                                         Mean   :1.267  
 3rd Qu.:25.50                                         3rd Qu.:1.375  
 Max.   :27.00                                         Max.   :1.400  

```
For numerical features, the code returned summary in terms of the `Min` for the minimum value in the column, `1st Qu` which is the lower quartile, median, mean, `3rd Qu` for upper quartile and `Max` for maximum value in the data. For the string variable, we obtained a summary of the length of the variable class and the mode.

This was an introduction to dataframe manipulation in R. 

Now, when dealing with a large scale dataset, we need some professional techniques that can help us navigate all complexity associated with such datasets until we obtain a subset of interest. Manipulating a dataset is a flexible task that cannot be mustard; instead, it requires one to be able to apply the core subsetting techniques until the intended objective is met. To guarantee optimality in this task, the dplyr provides users with various functions that, together with the pipes, we can write a precise code that establishes an excellent task.

Let's get started with dplyr.

### Data Manipulation with dplyr

We shall discuss the following verbs of the dplyr package.
1. select
2. filter
3. arrange
4. rename
5. mutate
6. group_by


Let's discuss what these verbs work in the order outlined above.
1. **select()**
This function selects a column(s) of a dataframe that we want to focus on.
Also, it's possible to omit a variable using the `select()` function by adding a negative sign before the variable name we want to omit in the `select()` function.

*Examples:*
Assume you want to subset the 'Gendre' and the 'Coures' from the dataframe we just created, then you can do so as follows.

```r
# intall the dplyr packages if not already installed on your R
# install.packages("dplyr")
library(dplyr) # load the library
my_data %>%
  select(Gender, Height)

```
This code returns:

```bash
  Gender Height
1      F    1.4
2      M    1.3
3      F    1.2
4      M    1.0
5      F    1.3
6      F    1.4

```
As we can see, with the `select()` function, we didn't create a lot of new variables to assign each step we make toward our destination. We only need to access the data and select the desired subset with the aid of pipelines, i.e., `%>^`.

Let's see how we can use the `select()` function and omit a particular variable from a dataset.

```r
my_data %>%
select(-Gender)

```
#### Output
```bash
  Age Course Height
1  20    BAS    1.4
2  21    BAS    1.3
3  24    BST    1.2
4  26    BAS    1.0
5  27    BST    1.3
6  20    BST    1.4

```
The Gender column was omitted from our dataset.

With the `select()` function, one can select variables starting or ending with a particular character.

*Example:*
```r
# select all variables starting with letter A
my_data %>%
  select(starts_with("A"))

 ```
 ### Output
 ```bash
   Age
1  20
2  21
3  24
4  26
5  27
6  20
 ```
 From our demo dataset, it's on the `Age` variable that starts with character A. So, the `select()` function was able to choose that variable.

 Next;
 Select variables that end with a particular character say `e`. Let's run the code below.

 ```r
 my_data %>%
   select(ends_with("A"))

```
#### Output
```r
  Age Course
1  20    BAS
2  21    BAS
3  24    BST
4  26    BAS
5  27    BST
6  20    BST

```
The explanation is obvious in this case.

2. **filter()**
The filter function extracts a subset of rows from a dataframe. For example, we can filter all the individuals whose age is above 21 years from the dataframe we just created. 

Below is how we go about this.

```r
my_data %>%
  filter(Age > 21)

```
This code returns:

```bash
  Age Gender Course Height
1  24      F    BST    1.2
2  26      M    BAS    1.0
3  27      F    BST    1.3

```
We successfully filtered our data by age.

Also, its possible to filter a dataframe by a character in R. For example;
```r
my_data %>%
  filter(Age > 21, Gender == "F")

```
Executing this code, we get:

```bash
  Age Gender Course Height
1  24      F    BST    1.2
2  27      F    BST    1.3

```
Here we filtered our data by the age and the gender of the individual.

3. **arrange()**

This function is used to reorder rows of a dataframe according to one of the columns (variables).

By default, this functions orders values of a column (numeric data) in ascending order.

For example,

```r
my_data %>%
  arrange(Height)

```
Executing this code, we get:

```bash
  Age Gender Course Height
1  26      M    BAS    1.0
2  24      F    BST    1.2
3  21      M    BAS    1.3
4  27      F    BST    1.3
5  20      F    BAS    1.4
6  20      F    BST    1.4

```
Note that the rows have been rearranged in ascending order of the entries `Height` column.

Suppose we want the values of the `Height`  arranged in descending order. How would we go about it?

Well, here we are only interested in the `Height` column. Therefore, we use the `select()` function to select that column of interest and then arrange its values using the `arrange()` function. However, we are required to arrange those values in descending order. Since we know the `arrange()` function returns values in ascending order, we need to give a command on which order the values should be arranged. To get values arranged in descending order, we pass the column we want to arrange in the `desc()` function before arranging it. The following three lines of code summarize all this.

```r
my_data %>%
  select(Height) %>%
  arrange(desc(Height))

```
### Output
```bash
  Height
1    1.4
2    1.4
3    1.3
4    1.3
5    1.2
6    1.0

```
4. **rename**
This function renames the columns of a dataframe. For example, suppose we want to rename our dataset's `Gender` column as `Sex`. Below is the code that demonstrates how we do so.

```r
my_data %>%
  rename(Sex = Gender)

```
#### Output
```bash
  Age Sex Course Height
1  20   F    BAS    1.4
2  21   M    BAS    1.3
3  24   F    BST    1.2
4  26   M    BAS    1.0
5  27   F    BST    1.3
6  20   F    BST    1.4

```
From the output, it's clear that the previously known as the `Gender` column is now labelled as the `Sex` column.

5. **Mutate()**
This function enables one to create a new variable based on the existing ones and concatenate them to the same dataframe. This is an essential feature as the dataset we work with usually miss some of the columns needed for analysis, i.e., when we need to compute the variance of a distribution, the data only contains X,s and Y,s; we cant compute the variance directly from this. Therefore it's our task to create convenient variables for any problem. 

Using our demo dataframe, let's see how this function work:

```r
my_data %>%
mutate(Age_diff = Age - mean(Age))

```
#### Output

```bash
  Age Gender Course Height Age_diff
1  20      F    BAS    1.4       -3
2  21      M    BAS    1.3       -2
3  24      F    BST    1.2        1
4  26      M    BAS    1.0        3
5  27      F    BST    1.3        4
6  20      F    BST    1.4       -3

```
The `mutate()` function automatically created a new column from the existing ones and added it to the dataframe.

6. **group_by()**
This function generates summary statistics from the dataframe within strata defined by a variable.

For example,
Suppose we are required to group our data by the Course one does and summarize the value for each group by the mean of height, then below is the code that performs this task.

```r
my_data %>%
group_by(Course) %>%
summarise(MeanAge = mean(Age), mediaHeight = media(Height))

```

#### Output
```bash
  Course MeanAge mediaHeight
  <chr>    <dbl>       <dbl>
1 BAS       22.3         1.3
2 BST       23.7         1.3

```

As we can see, our data were grouped into two classes of the course. From each class, we get only the summary of all elements in it, which in our case we took mean for the age and media for the Height columns.

This is how the dplyr verbs work. Now, this was a basic illustration. We can't claim to know the full power of these functions unless we use them in a complicated situation. With that said, we will challenge ourselves using a real-life dataset that will force us to use combinations of these techniques and extract the required subset of data to perform the required task.

#### Application

Consider the mpg data set in the ggplot2 package. For the purpose of this question, consider each observation a different car.

 a. Which car(s) had the highest highway gas mileage?
 b. Compute the mean city mileage for Audi cars
 c. Compute the mean city mileage for each model of cars, and arrange in decreasing order
 d. Which cars have the smallest absolute difference between highway and city miles?
 e. Compute the mean highway mileage for each year and arrange in decreasing order
 
 *Solution*
 First, we need ensure we have the `ggplot2` package installed on R so that we can load the data it contains. The following line installs this package. Before you can install this package, make sure you're connected to the network. If it is already installed on your R environment, you don't need to download it again. Skip this stage and load it instead to your workspace.

 ```r
#install.packages("ggplot2")
 library(ggplot2)

 ```
 To walk smoothly through this challenge, it's essential first to print the head of the data so that we know the columns we are working with. So let's visualize our dataset by printing its head.

 ```r
head(mpg)

 ```
 #### Output

 ![mpg-dataset](head.png)

 From this output, it's clear that the dataset has 11 columns.

 The description of these dataset is as shown in the figure below.

 ![mpg-dataset](mpg-data.png)
 [Image Source](https://rstudio-pubs-static.s3.amazonaws.com/322396_ca6932a8cca04ee2b33d9cebdef8142b.html)

 Now, let's solve our problem.

 a. Which car(s) had the highest highway gas mileage?

 To solve this problem, we only need two columns: the `manufacturer` and `hwy` columns. After selecting these columns, we arrange the values of the `hwy` in descending order. Now, we get the first row that corresponds to the car with the highest `hwy`. Below is the code for carrying out this activity.

 ```r
mpg %>%
  select(manufacturer, hwy)%>%
  arrange(desc(mpg))%>%
  head(1)

 ```

 ```bash
 # A tibble: 1 × 2
  manufacturer   hwy
  <chr>        <int>
1 volkswagen      26

```
b. Compute the mean city mileage for Audi cars

We need the `manufacturer` and `cty` columns from our dataset. After selecting these two columns, we filter the `manufacturer` column by `audi`. Once we do so, we then group Audi by its mean. This will return the mean value of the Audi cars.

```r
mpg  %>%
  select(manufacturer, cty) %>%
  group_by(manufacturer)%>%
  summarise(mean_cty = mean(cty))%>%
  filter(manufacturer == "audi")

```

### Output
```bash
  manufacturer mean_cty
  <chr>           <dbl>
1 audi             17.6

```

c. Compute the mean city mileage for each model of cars, and arrange in decreasing order

To solve this problem, we need to slightly modify the program we used to compute the mean city mileage for the Audi model and add something little. Since we need the mean city mileage for all models, we only get rid of the filter function in our previous code and introduce the `arrange()` function for arranging our mean in descending order. Below is the modified program.

```r
mpg  %>%
  select(manufacturer, cty) %>%
  group_by(manufacturer)%>%
  summarise(mean_cty = mean(cty))%>%
  arrange(desc(mean_cty))

```
#### Output
```bash
# A tibble: 15 × 2
   manufacturer mean_cty
   <chr>           <dbl>
 1 honda            24.4
 2 volkswagen       20.9
 3 subaru           19.3
 4 hyundai          18.6
 5 toyota           18.5
 6 nissan           18.1
 7 audi             17.6
 8 pontiac          17  
 9 chevrolet        15  
10 ford             14  
11 jeep             13.5
12 mercury          13.2
13 dodge            13.1
14 land rover       11.5
15 lincoln          11.3
> 

```

d. Which cars have the smallest absolute difference between highway and city mileages?
*solution*
Here we need:
`manufacture`, `hwy`, and `cty` columns.
Then, we will compute the difference between the `hwy` and `cty`, take an absolute of the results and create a column that represents this difference. Finally, we will arrange the new column and print the first observation.

```r
mpg %>%
  select(manufacturer, hwy, cty) %>%
  mutate(Abs_diff = abs(hwy-cty)) %>%
  arrange(Abs_diff) %>%
head(1)

```
#### Output
```bash
  manufacturer   hwy   cty Abs_diff
  <chr>        <int> <int>    <int>
1 nissan          17    15        2

```

e. Compute the mean highway mileage for each year and arrange in decreasing order

We need to select the `year` and `hwy` columns from our mpg dataset in this problem. After selecting these columns, we group them by the `year` and summarize them by the mean `hwy`.

The code below will establish this task.

```r
mpg%>%
  select(year, hwy) %>%
  group_by(year) %>%
  summarize(mean(hwy))

```
#### Output

```bash
# A tibble: 2 × 2
   year `mean(hwy)`
  <int>       <dbl>
1  1999        23.4
2  2008        23.5
```
### Summary
This article covered all needed to get started with data manipulation using the dplyr package. As you have seen in the above challenge, the order in how you combine this function to land on the required destination is not defined. It all depends on the dynamics of the problem at hand. You can combine them in different ways depending on where you want them to end you. This is all about data manipulation with the dplyr package.

The full code for this session is available [here]().
