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
# ■ Part 1 - Mathematics foundations
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

# Linear algebra

















































