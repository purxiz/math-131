---
title: Lecture 6
---

## Interpolation
Based on some data (a discrete series of points), find some new data (in between, after, before the points, etc.).
In order to "connect the dots," we will be using polynomial approximation. This means we will be drawing polynomials that match as closely as possible to the data we have available. We can then solve for a point on that polynomial to find our "new data."
Polynomials are nice because they are easy to integrate and differentiate, and return other polynomials for either operation. (i.e. the derivative of a polynomial is a polynomial, same for integral).
Another handy thing about polynomials is that we can always find a polynomial that covers every point in our data set. The theorem (Weirestrass Theorem) is as follows:

$$
\textrm{Given } f \in C([a,b]), \forall \epsilon > 0, \exists P(x) \textrm{ such that } |f(x)-P(x)| < \epsilon, \forall x \in [a,b] \\
\textrm{Where } P(x) \textrm{ is a polynomial}
$$

In other words, given some continuous function \\(f\\), for all \\(\epsilon \gt 0\\), there exists some polynomial \\(P(x)\\), such that the absolute value of \\(f(x)-P(x)\\) is less than \\(\epsilon\\) for all \\(x\\) in the given interval \([a,b]\).
In other other words, if you have some function \\(f(x)\\) continous over an interval, and you draw a very small "strip" around that function on that interval, then you can always find a polynomial that fits within that strip (no matter how thin it is).
For this class, we only consider continuous functions therefore we can alsways find a polynomial as close as we want to the function. If you want you can look up a proof!
Anyway, the question we have to ask now is what kind of polynomials we will be using. If you haven't guessed it already, we'll be using Taylor Polynomials. They work really well because if \\(f\\) is differentiable \\(n+1\\) times on the interval [a,b], the Taylor Polynomial of degree \\(n\\) will work. Unfortunately, Taylor Polynomials are somewhat local, meaning they only work well when \\(|x-x_0|\\) is small.
In case you forgot what a Taylor Polynomial is somehow, here it is again (this will also help break up this huge wall of text):

$$
f(x) = f(x_o) + \frac{f'(x_o)}{1!}(x-x_o) + \frac{f''(x_o)}{2!}(x-x_o)^2 + ...+\frac{f^n(\xi(x))}{n!}(x-x_o)^n
$$

Where the first terms are the Taylor Polynomial and the last term (the \(f^n\) term above) is the remainder (or error) term. Because it's local we will need other polynomials. Let's build a basis of polynimials that will be useful.
## Lagrange Polynomial
Create a basis of polynomials. In other words, we will perform a linear combination of this basis polynomials to form our final polynomial.
Given \\(x_0,x_1,...,x_n\\), create \\(n+1\\) polynomials of degree n such that \\(P_k(x)=1\\) when \\( x=x_k \\) and 0 when \\(x=x_j, j \ne k\\).
Let's start with n=2 as a quick example. we will have 3 points, \\(x_0, x_1\\) and \\(x_2\\).
Note: I'm still working on drawing nice graphs, but hopefully this will do for now:
![Lagrange Polynomials](https://i.imgur.com/Eyy4tqk.png)
The black polynomial is the polynomial \\(P_1(x)\\). You can see that it is equal to 1 at \\(x_1\\), and equal to 0 at each other point. The same holds for the red Polynomial \\(P_2(x)\\), which is equal to 1 at point \\(x_2\\), but equal to 0 at both other points. *NOTE:* These should be \\(x_0, x_1, x_2\\) to be consistent with the class notations. 
This is relevant because now we'll be looking at it non-graphically. We know that we are trying to find a polynomial of degree \\(n\\) that is equal to 0 at all \\(x_j\\) except for \\(j =k\\). Note that this is simply the roots of the polynomial. Given all the roots \\(x_0...x_n\\), we can write the Polynomial as:

$$
P(x)=(x-x_0)(x-x_1)...(x-x_{k-1})(x-x_{k+1})...(x-x_n)
$$

We skip \\(x-x_k\\) per our original rules:

$$
P(x) =
\begin{cases}
1 & x = x_k \\
0 & x = x_j, j \ne k
\end{cases}
$$

Then we just normalize at \\(x = x_k\\) for satisfy all requirements:
$$
P(x_k) = \prod_{j=0,j \ne k}^n(x_k-x_j)
$$

To get \\(P(x_k) =1\\) we simply divide by \\(\prod_{j=0,j \ne k}^n(x_k-x_j)\\).

From there, we can move on to actual Lagrange Polynomial. For notation, we are going to call our Polynomials \\(L\\) from now on, because they are Lagrange Polynomials. They will have two indices.

$$
L_{n,k}(x) = \frac{\prod_{j=0,j \ne k}^n(x-x_j)}{\prod_{j=0,j \ne k}^n(x_k-x_j)}
$$


The index \\(n\\) is the degree, and the index \\(k\\) denotes the \\(k\\)th polynomial in the basis. The Lagrange Polynomials form the basis for interpolation. Finally we get the following:

$$
\textrm{Given } x_0, x_1,..., x_n \textrm{ and } f(x_0), f(x_1),...,f(x_n) \\
\textrm{There exists a UNIQUE Polynomial of degree at most n such that } \\
f(x_k) = P(x_k) \textrm{ for } k=0,1,...,n
$$

This polynomial is given by
$$
P(x) = \sum_{k=0}^nf(x_k)L_{n,k}(x)
$$

### Example

$$
x_0 = 0, x_1 = 1, x_2 = 2 \\
f(x_0) = 1.3, f(x_1)=-1.2, f(x_2)=\sqrt{3}
$$

Create the Lagrange Interpolant given the above data.
Let's take a look at the first Lagrange Polynomial, \\(L_{2,0}\\), the rest can be done similarly.

$$
L_{2,0}(x) = \frac{\prod_{j=0,j \ne 0}^2(x-x_j)}{\prod_{j=0,j \ne 0}^2(x_0-x_j)}
$$

Expanding the multiplications, we're looking at something like:

$$
L_{2,0}(x) = \frac{(x-x_1)(x-x_2)}{(x_0-x_1)(x_0-x_2)}
$$

Plugging in our numbers we get:
$$
L_{2,0}(x) = \frac{(x-1)(x-2)}{(-1)(-2)}
$$
And finally simplifying all the way we get:
$$
L_{2,0}(x) = \frac{(x-1)(x-2)}{2}
$$

We have to do two more, \\(L_{2,1}\\) and \\(L_{2,2}\\). Those look similar, like:

$$
L_{2,1}(x) = \frac{\prod_{j=0,j \ne 1}^2(x-x_j)}{\prod_{j=0,j \ne 0}^2(x_0-x_j)}
$$
Expanded, just like \(L_{2,0}\), that looks like:
$$
L_{2,1}(x) = \frac{(x-x_0)(x-x_2)}{(x_1-x_0)(x_1-x_2)}
$$
Finally, \\(L_{2,2}\\) looks like:
$$
L_{2,2}(x) = \frac{(x-x_0)(x-x_1)}{(x_2-x_0)(x_2-x_1)}
$$

Plugging in actual values for \\(L_{2,1}\\) and \\(L_{2,2}\\), and we get something like:
$$
\begin{array}{cc}
L_{2,1}(x) = \frac{(x)(x-2)}{-1} & L_{2,2}(x) = \frac{(x)(x-1)}{2}
\end{array}
$$
Finally to get the interpolant you would plug these all into
$$
P(x) = \sum_{k=0}^nf(x_k)L_{n,k}(x)
$$
Which would give you something like:
$$
P(x) = 1.3 \frac{(x-1)(x-2)}{2} - 1.2 \frac{(x)(x-2)}{-1} + \sqrt{3} \frac{(x)(x-1)}{2}
$$
