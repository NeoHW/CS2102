## Common aggregated function

1. **AVG**()
2. **SUM**()
3. **COUNT**()
4. **MAX()** / **MIN**()
5. **STDDEV_POP()**: Computes the population standard deviation of a set.
6. **STDDEV() / STDDEV_SAMP()**: Computes the sample standard deviation of a set.



## Execution Order

1. From/ Join
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. DISTINCT
7. ORDER BY
8. LIMIT/ OFFSET

```
SELECT count(*), s.department
FROM student s
GROUP BY s.department
HAVING count(*) > 10;
```



## ROUND

#### Syntax

The following illustrates the syntax of the `ROUND()` function:

```
ROUND (source, [ , n ] )
```

#### Arguments

The `ROUND()` function accepts 2 arguments:

1) source

The `source` argument is a number or a numeric expression that is to be rounded.

2) n

The `n` argument is an integer that determines the number of decimal places after rounding.

The n argument is optional. If you omit the n argument, its default value is 0.

## STDDEV

In PostgreSQL, both `stddev_pop` and `stddev_samp` are aggregate functions that calculate the standard deviation for a given set of values. The key difference between the two lies in the formula and their purpose:

1. **`stddev_pop` (Population Standard Deviation)**:
   
   - This calculates the standard deviation for an entire population.
   - Uses the formula:
     $ \sqrt{ \frac{1}{N} \sum_{i=1}^{N} (x_i - \bar{x})^2 } $
   - Where \( N \) is the number of data points, \( x_i \) represents an individual data point, and $\bar{x}$ is the mean of all data points.
   - This function assumes that the provided dataset represents the entire population, so it uses $N$ (the full count) in the denominator.
   
2. **`stddev_samp` (Sample Standard Deviation)**:
   
   - This calculates the standard deviation for a sample subset of a larger population.
   
   - Uses the formula:
     
     $ \sqrt{ \frac{1}{N-1} \sum_{i=1}^{N} (x_i - \bar{x})^2 } $
     
   - Here, \( N \) is the number of data points in the sample. The subtraction by 1 in the denominator (known as Bessel's correction) helps provide a more unbiased estimate of the population variance when working with a sample.
   
   - This function assumes that the dataset represents only a sample of the full population. Thus, by using $ N-1$ (known as the degrees of freedom) in the denominator, it gives a slightly larger standard deviation, which helps account for the potential variability in the larger population that hasn't been observed.

**When to use which?**

- If you are working with a dataset that represents an entire population and want to compute its standard deviation, you'd use `stddev_pop`.
- If you are working with a dataset that represents only a sample of a larger population and you want to estimate the standard deviation of the larger population based on this sample, you'd use `stddev_samp`.

In many practical scenarios, especially with large datasets, the difference between these two values can be marginal. However, understanding the difference and when to use each is crucial for accurate statistical analysis.



## IN vs EXISTS

The IN operator is used to check whether a specific value matches any other value in the set of values. These set of values can either be specified separately or be returned by any sub query.



The EXISTS operator is used to look for the existence of a row in a given table that satisfies a set of criteria. It is a Boolean operator that compares the result of the subquery to an existing record and returns true or false.





## Interpretation

Here, we want to ensure that for every book written by Adam Smith, there exists a loan by the student. 

==>

If even one such book doesn't have a loan record for the student, the student is excluded from the results.



### Explanation:

1. Start with each student.
2. For each student, check every book authored by "Adam Smith".
3. For each of those books, check if there is a loan record indicating that the student borrowed that book.
4. If there's even one book by "Adam Smith" that the student hasn't borrowed, then the student will not be included in the final result. Only students who borrowed all books authored by "Adam Smith" will appear in the output.