---
title: lecture 8 (More Divided Differences and Cubic Splines)
---
## Lecture 8
## More Divided Differences
$$
f[x_j,x_{j+1}] = \frac{f(x_{j+1}) - f(x_j)}{x_{j+1}-x_j}
$$  
Look at the right side of that equation. If you'll notice, it looks sorta like a derivative. In fact, if you want to get really technical, it is a derivative.  
Remember the Mean Value Theorem?  
There exists \\(\xi\in[x_0,x_1]\\) such that:  
$$
f'(\xi) =  \frac{f(x_1) - f(x_0)}{x_1-x_0} = f[x_0,x_1]
$$  
If you'll recall from the end of last lecture, it's totally possible to calculate larger divided differences like \\(f[x_0,x_1,x_2]\\). We can use this same concept to calculate higher order derivatives using Newton's Divided Differences. Generally:  
If \\(f\in C^n([a,b]), \exists\xi\in[a,b]\\) such that \\(f[x_0,...,x_n] = \frac{f^{(n)}(\xi)}{n!}\\)

## Limitations of the Lagrage Interpolant
The more points (x_0,x_1,...,x_n) you have, the less accurate your interpolant becomes (especially at the extrema). This is because more points means a higher degree polynomial, so in order to match all those roots near the extrema, the polynomial must oscillate wildly.
How do we fix this issue?  
Well, the idea is just to use polynomials with lower degrees, to avoid the too many points issue. In order to still cover the entire interval we want to interpolate over, we will divide it into subintervals, so that for any given problem we are working with a more limited number of points.
**TODO:** make a nice graph displaying an interpolant with subintervals
The idea is to divide the subinterval at several points, which we will call nodes. For each pair of nodes \\([x_i,x_{i+1}]\\), we will create a polynomial interpolant.  
The most obvious method is to simply create a Lagrange Interpolant for every subinterval (i.e. every pair of nodes). This is simpler, and requires less calculation, but it doesn't match slopes.  
In order to make our interpolant more accurate, we have to try something more advanced. So far, we have based our interpolant only on the points at \\(f(x_k)\\), supplied by the originally given data.  
However, to make our interpolant more accurate, we can try to also match the derivatives, to help our "slopes don't match" error. We want to construct our polynomial interpolant such that:  
$$
P_0'(x_1) = f'(x_1) = P_1'(x_1)
$$  
To have the ability to match slopes according to the above definition, you need quadratic polyonmials. This method of matching the points, and the slopes (i.e. first derivatives) is known as the Hermite Interpolant. You can read more about them in section 3.4 in the book.  
However, today we will be going over a concept known as **Cubic Splines**. To do that, we will make a polynomial of degree three that matches the point, the slope (first derivative) and the second derivative.  
$$
P_0''(x_1) = f''(x_1) = P_1''(x_1)
$$  
Let's make the meaning of the above equivalence a little bit more clear. Consider the right edge of the first sub-interval. To the left is a polynomial approximation \\(P_0\\), and to the right is another polynomial approximation \\(P_1\\). They meet at the edge of the subinterval, at the point \\(x_1\\). At that point, we want to make sure of the following:  
$$
P_0(x_1) = f(x_1) = P_1(x_1)
$$  
$$
P_0'(x_1) = f'(x_1) = P_1'(x_1)
$$  
$$
P_0''(x_1) = f''(x_1) = P_1''(x_1)
$$  
In plain English, and just to drive the point home, at the edge of the subinterval, the polynomial and the polynomial on the right don't just meet up at the point \\(f(x_1)\\), they also both match the first and second derivative of \\(f\\) at \\(x_1\\).  
Up until now, we've been looking at the edge of one subinterval, but now let's start working towards a general definition. In fact, here's the definition right here:  
#### Definition of a Cubic Spline Interpolant
A cubic spline interpolant of \\(f\\) is a function \\(S(x)\\) with the following characteristics:  
$$
S(x) = S_j(x) \textrm{ over } [x_j,x_{j=1}] \textrm{ for } j=0,1,...,n-1
$$  
**TODO:** Create a nice graph showing multiple S_j(x).  
\\(S_j(x)\\) is a cubic polynomial i.e. \\(S_j(x) =A_j+B_j(x-x_j)+C_j(x-x_j)^2+D_j(x-x_j)^3\\)  
We add the \\( -x_j\\) so that at each point, we can easily calculate \\(A_j\\), since the other terms easily go to \\(0\\).  
The third condition is that \\(S_j(x_j) = f(x_j)\\) and \\(S_j(x_{j+1}) = f(x_{j+1})\\). We must also include the condition that the subintervals match each other at their extrema.  
$$
S_j(x_{j+1})=S_{j+1}(x_{j+1}) = f(x_{j+1}) \textrm { for } j=0,...,n-1
$$  
The next two conditions are very similar, except for the first and second derivatives instead of the points.  
$$
S_j'(x_{j+1}) = S_{j+1}'(x_{j+1}) \textrm { for } j=0,...,n-2
$$  
$$
S_j''(x_{j+1}) = S_{j+1}''(x_{j+1}) \textrm { for } j=0,...,n-2
$$  
For the first and second derivative, we don't always know \\(f'\\) or \\(f''\\), so we don't need \\(S_j'\\) or \\(S_j''\\) to match the function f. We will see that having them match each other guarantees a continuity of the derivative and second derivative, which helps with accuracy.  
To summarize:  
1. \\(S(x) = S_j(x)\\) over \\([x_j,x_{j=1}]\\) for \\(j=0,1,...,n-1\\)
2. \\(S_j(x) =A_j+B_j(x-x_j)+C_j(x-x_j)^2+D_j(x-x_j)^3\\)
3. \\(S_j(x_{j+1})=S_{j+1}(x_{j+1}) = f(x_{j+1}) \textrm { for } j=0,...,n-1\\)
4. \\(S_j'(x_{j+1}) = S_{j+1}'(x_{j+1}) \textrm { for } j=0,...,n-2\\)
5. \\(S_j''(x_{j+1}) = S_{j+1}''(x_{j+1}) \textrm { for } j=0,...,n-2\\)

There are two extra conditions we can take a look at to further improve the performance of our splines.  
First, a **natural cubic spline** is one where:  
$$
S_0''(x_0) = 0 = S_{n-1}''(x_n)
$$  
Or in other words, because we don't know anything about the second derivatives at the end points, we will just zero them out.  
Second, a **Clamped Cubic Spline** is one where:  
$$
S_0'(x_0) = f'(x_0) \textrm{ and } S_{n-1}'(x_n) = f'(x_n)
$$  
Or in other words, the first derivative of our polynomial at the end points should be equal to the slope of the actual function.

### Error in cubic Splines
The real error term is more general, but we're going to suppose that all nodes are equidistant. This makes it a lot easier to understand. We will say that \\(h\\) is the distance between two points, i.e. \\(h=x_{j+1} - x_j\\).  
Then we can say that:  
$$
f(x) = S(x) + O(n^4)
$$  
This means that we have a fourth order error bound. In other words, for small \\(h\\), error will be very small.
### example
Build the natural cubic spline  
$$
\begin{array}{c|c}
x & f(x) \\
-1 & 0.8 \\
-0.5 & 0.9 \\
10 & 1.1 \\
0.5 & 1.3
\end{array}
$$  
First, let's just start with \\(S_0(x)\\).  
$$
S_0(x) =A_0+B_0(x-x_0)+C_0(x-x_0)^2+D_0(x-x_0)^3
$$  
If we evaulate \\(S_0\\) at \\(x_0\\), we get that:  
$$
S_0(x_0) = A_0 = f(x_0) = 0.8
$$  
Let's switch gears over to looking at the coefficients. It should be relatively obvious that:  
$$
A_1 = 0.9, A_2 = 1.1
$$
