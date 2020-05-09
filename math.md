# 1. Linear coordinate system
1)  2x - 3 > 0
        2x > 3
        x > 1.5
2)  |5-x| <= 3
        -3 <= 5 - x <= 3
        -8 <= -x <= -2
        2 <= x <= 8
3)  18x - 3x^2 > 0
        3x(6 - x) > 0
        x = 0; x = 6
        calculate sign for all three intervals (-INFINITY, 0), (0, 6), (6, INFINITY)
        x < 0 => (y < 0)
        0 < x < 6 => (y > 0) => single answer
        x > 6 => (y < 0)
4)  |2x - 5| >= 3
        2x - 5 >= 3, x >= 4
        2x - 5 <= -3, x <= 1

# 2. Rectangular coordinate system
Distance in rectanguar coordinate system is equal to sqrt( (x1 - x2)^2 + (y1 - y2)^2 )
To dind some point laying on a line x1y1 -> x2y2 you need to get differences between
x1 and x2 and y1 and y2, multiply them by the percent you need and add resulting 
values to x1 and y2 respectively.

# 3. Lines
Steepness is measured by a number called a `slope`
slope m = y2 - y1 / x2 - x1

## Point-slope equasion
`y - n1 / x - n2 = m`, where m is slope and n1 and n2 are constants.

## Slope-intercept equasion
`y = mx + b`, where b is a constant

## Parallel lines
Parallel lines have the same slope.

## Perpendicular lines
Lines are perpendicular if the product of their slopes is -1.
m1 * m2 = -1 => `m1 = - 1 / m2`

# 4. Circles
R = sqrt((x-a)^2 + (y-b)^2)     => `R^2 = (x-a)^2 + (y-b)^2`
where (a,b) is the coordinate of the center.

## Exercise

x^2 + y^2 + 8x − 6y + 21 = 0      => 
(x + 4)^2 + (y - 3)^2 - 4 = 0  => r = 2

# 5. Equasions and their graphs
<!-- p33 -->

# ■■■ Mathematics for machine learning
# ■ Day 1
# Introduction and motivation
There are three concepts that are at the core of machine learning:
data, a model, and learning. A model is said to learn from data if its performance
on a given task improves after the data is taken into account.
The goal is to find good models that generalize well to yet unseen data.

`Learning` can be understood as a way to automatically find patterns
and structure in data by optimizing the parameters of the model.

## Finding words for intuitions
While not all data is numerical, it is often useful to consider data in
a number format. 

## Four ways to read this book
1. Bottom - up
    (may be boring but you know what's going on at every stage)
2. Top - down
    (may be hard to understand at times what is going on, 
    but interest to the field growth)
3. Left - right top - down
4. Left - right bottom - up
    Machine learning topics:
        1. regression
            math: 
                vercor calculus
                linear algebra
        2. Dimentionality reduction & Density estimation
            math:
                Probability and distribution
                analityc geometry
        3. Classification
            math:
                Optimization
                Matrix decomposition

We often consider data to be noisy observations of some true underlying signal.

# ■ Day 2
# Linear algebra
Linear algebra is the study of vectors and certain 
rules to manipulate `vectors`. 
There are two kinds of vectors: 
    1. Geometric
        directed segments
    2. Polinomials
        abstract concepts but are vectors nevertheless
    3. audio signals are vectors
        we can add them and multiply them by a scalar
        and as a result we get another audio signal
    4. Tuples of n real numbers are vectors
        
## Systems of linear Equations
In a system of linear equations with two variables x1; x2, each linear equation
defines a line on the x1x2-plane. Since a solution to a system of linear
equations must satisfy all equations simultaneously, the solution set is the
`intersection of` these `lines`.

for three variables, each linear equation determines a plane in three-dimensional space.

For a systematic approach to solving systems of linear equations, we
will introduce a useful compact notation. We collect the coefficients aij
into vectors and collect the vectors into `matrices`.

## Matrices
Matrices play a central role in linear algebra. They can be used to compactly
`represent systems of linear equations` but they also represent linear
functions (linear mappings).

matrix A is an `m * n tuple of elements` which is ordered
according to a rectangular scheme consisting of m rows and n columns

By convention (1, n) matrices are called rows and (m, 1) matrices are called columns.

1. Addition of matrices
    The sum of two matrices is defined as adding the corresponding entries.

2.0 Multiplication by a scalar
    Just multipy each element by the scalar.
    
2.1 Multiplication of matrices
    To get the first value(top left) you need to multiply two vectors: first vector is 
    the first row of the first matrix,
    second vector is the first column of the second matrix

    `[2, -3]` . [`10`, -8] = [`(2 * 10) + (-3 * 12)`, (2 * -8 + -3 * -2)]
     [7,  5]    [`12`, -2]   [(7 * 10) + (5 * 12),    (7 * -8 + 5 * -2)]

    To get the second value you change "first column" to the `second column`.
    (when multiplying in rows we go left-to-right, in columns we go top-to-bottom)
    Third value in a matrix 2 * 2, can be calculated by multiplying `second row` by 
    the first column.

    The result of Matrix multiplication changes depending on `order of matrices`
    **AB != BA** 
    
3. Multiplications constraints
    1. The length of row in the "first" matrix must be the same 
        as the length of column in the same matrix.
    The result matrix will have the amount of `rows` from the `first` matrix
    and the amount of columns from the second one.

    Matrices multiplication is
        `associative`: (ab)c = a(bc) - because the order is preserved
        `distributive`: (a + b)c = ac + bc

4. Inverse and transpose
    Inverse of a matrics is such matrix that when multiplied by the matrix in 
    any order, yields the same result - `I(n)` (I(n) - is an `identity matrix` of size n)

    For any `square` matrics, identity matrix is matrix with 1 on the main diagonal
    and zeroes elsewhere. 

    The inverse of matrix multiplied by that matrix yields identity matrix.
    
    1. Inverse for a 2x2 matrix A
        [a b]
        [c d]
        
        A^-1 = ( 1 / ad - bc ) * [d -b]   // (ab - bc) is called the `determinant - (|A|)`
                                 [-c a]

    2. Inverting a 3x3 matrix A
        [a b c]
        [d e f]
        [g h i]
        For the left top value we need to delete top row and first column. And calculate
        a determinant (`note`, determinant, not 1 / determinant)
        for the 2x2 matrix that's left.
        
        For the next values we do the same thing: delete corresponding 
        row and column and get 2x2 matrix's determinant.
        You have to do that for all the values.
        
        `Then` you have to apply +/- coefficients to that values
        depending on their position (if i+j is an odd number you multiply by -1):
        [+ - +]
        [- + -]
        [+ - +]
        (note, it's not multiplication of matrices, you just map these coefficients in
        the simples way possible)

        `Then` you need to do the **transpose** which is just `switching rows and cols`
        or `replication on the main diagonal`. (swap numbers to the symmetric place
        on another side of the diagonal)
        After that you get the `adjoint` of the matrix A.
        
        A^-1 = 1 / |A| * adjoint(A)
        
        You have adjoint already so you need to solve the |A| part.
        |A| = a * adjoint(A)[a] + b * adjoint(A)[b] + c * adjoint(A)[c] 
        
        here a, b and c represent the value at same position (x, y)
        from the initial matrix.

5. Compact representation of systems of linear equations.
<!-- p26 -->























