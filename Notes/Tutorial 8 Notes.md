## Tutorial 8 Notes

### 1.Guidlines:

1. Informally, each tuple in a relation should represent one entity or relationship instance. (Applies to individual relations and their attributes).  A.K.A Attributes from entities should not be mixed inappropriately

2. Design a schema that does not suffer from the **insertion**, **deletion** and **update** **anomalies**. If there are any anomalies present, then note them so that applications can be made to take them into account.

   what are insertion/deletion/update anomalies (https://stackoverflow.com/questions/18961118/how-can-anomalies-insertion-deletion-and-updation-be-introduced-into-oracle-da)

3. Relations should be designed such that their tuples will have as few NULL values as possible

4. The relations should be designed to satisfy the lossless join condition.





### 2. Lossless Join

Two conditions:  R -> R1, R2	

1. 

$$
\displaystyle R_{1}\bowtie R_{2}=R
$$

2. 

$$
{\displaystyle R_{1}\cap R_{2}\rightarrow R_{1}}

\\
or
\\
R_{1}\cap R_{2}\rightarrow R_{2}
$$

### 3.Dependency Preserving

If we decompose a relation R into relations R1 and R2, All dependencies of R either must be 

1. a part of R1 or R2 
2. or must be derivable from a combination of functional dependency of R1 and R2. 

For Example, A relation R (A, B, C, D) with FD set{A->BC} is decomposed into R1(ABC) and R2(AD) which is dependency preserving because FD A->BC is a part of R1(ABC)



### 4.Armstrong Axisoms

![Screen Shot 2023-11-03 at 12.00.24 AM](/Users/c.s./Library/Application Support/typora-user-images/Screen Shot 2023-11-03 at 12.00.24 AM.png)

reference link: https://en.wikipedia.org/wiki/Armstrong%27s_axioms



### 5. Check the equivalence of sets of functional dependencies

To chece where F1 and F2 are equivalent, we just need to prove F1 -> F2 And F2 -> F1



Consider another example where two functional dependencies are equivalent.

R=(A,C,D,E,H)

F1={A->C, AC->D, E->AD, E->H},

F2={A->CD, E->AH}



Check whether F1 and F2 are equivalent or not?

To check F1 covers F2 −

A+={A,C,D} contains C,D

E+={A,D,E,H} contains A,H

So F1 covers F2

To check F2 covers F1:

A+={A,C,D} contains C

{ A,C}+={A,C,D} contains D

E+={A,C,D,E,H} contains A,D,H

So F2 covers F1.

Hence F1 and F2 are equivalent.



### 6 Find the minimal Cover

1. Keep only one attribute on the right hand side of each FD
2. Try to simplify the right hand side
3. ##### Remove any redundant FD 