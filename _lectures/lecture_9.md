---
title: lecture 9
---
## Lecture 9

## Quick Summary
In lecture 8, we went over the cubic spline interpolant. Recall that interpolation in general is a strategy to approximate a function given several data points \\((x,y)\\) for that function.  We started with the Lagrange Interpolant, which matches a single polynomial to the data points. However, the Lagrange Interpolant oscillates wildly around the endpoints of the graph, and can occasionally just oscillate extremely wildly around every data point. Cubic splines solve this problem by calculating the Lagrange Interpolant piece-wise. More data about Cubic Splines can be found in the lecture 8 notes.
Today we will be going over:

## Chapter 4: Numerical Differentiation and Integration

The general method of numerical differentiation is called the (n+1)-point formula. Recall that differentiation and integration are defined simply by:  
$$
f'(x_0) \approx \frac{f(x)-f(x_0)}{x-x_0}
$$  
for differentiation
and  
$$
\int_a^bf(x)dx \approx \sum\limits_{n=0}^N f(x_n)\Delta x
$$  
For numerical differentiation specifically, we will use the first equation for the derivative. Let's take a look at where it comes from.  First, we'll introduce a value h, where \\(h > 0 \\) and we'll set \\(x=x_0+h\\).  
Now, our goal is to find the tangent line at a point, i.e. the derivative at that point. In order to get it numerically, why don't we just use the Lagrange interpolant? We can calculate the Lagrange polynomial about two points, \\(x_0\\), and \\(x_0+h\\), which we'll call \\(x_1\\).  
Look to lecture 6 for more examples of calculating the Lagrange Interpolant. For these points we should get:  
$$
L_{1,0}(x) = \frac{x-x_1}{x_0-x_1}
$$
and
$$
L_{1,1}(x) = \frac{x-x_0}{x1-x_0}
$$  
If we substitute \\(x_1 = x_0+h\\) again, we get something like:  
$$
L_{1,0}(x) = -\frac{1}{h}(x-(x_0+h))
$$
and
$$
L_{1,1}(x) = \frac{1}{h}(x-x_0)
$$  
We can combine these to get our Lagrange interpolant:  
$$
f(x) = \frac{f(x_0)}{h}(x-(x_0+h)) + \frac{f(x_0+h)}{h}(x-x_0) + \frac{f''(\xi)}{2!}(x-x_0)(x-x_1)
$$  
With the last term being our error term. Our goal was ultimately to find the derivative at a point, so here we can take the derivative of f(x). That's a hugely annoying formula to write out, so if you don't want to do it yourself, just accept that it simplifies down to:  
$$
f'(x_0) = \frac{f(x_0+h)-f(x_0)}{h} - \frac{h}{2}f''(\xi)
$$  
This approximation is called the forward difference, since we're looking at the point \\(x_0\\) and a point after it \\(x_0+h\\), where \\(h\\) is positive. This is called a two point formula, since we're using two points. Backwards difference is as simple as:  
$$
f'(x_0) \approx \frac{f(x_0)-f(x_0-h)}{h}
$$  
The second term in our forward difference was the error term. It's definitely important, but we can also just say approximately equal (\\(\approx\\)). This two point method converges linearly, which isn't great. We'd like higher order convergence. Recall that earlier we mentioned this was the (n+1)-point formula. To that end, we can extend the method by using more points.  
We'll look at the points:  
$$
x_0, x_0 \pm h, x_0 \pm 2h, x_0 \pm 3h, \textrm{ etc.}
$$  
The method for deriving the three point formula is exactly the same, though even more horrible to write out since the derivatives become higher order. The three point formula is based on the the following points:  
$$
x_0, x_0-h, x_0 + h
$$  
There are two different 3 point formulas.  
$$
\textrm{Midpoint: } f'(x_0) = \frac{f(x_0+h)-f(x-0)}{2h}-\frac{h^2}{6}f''(\xi(x))
$$  
The three point formula converges at a rate of \\(O(n^2)\\). The other three point formula is called the end point formula. You won't have to memorize either of these.  
$$
\textrm{Endpoint: } \frac{f'(x_0) = -3f(x_0)+4f(x_0+h)-f(x_0+2h)}{2h}-\frac{h^2}{6}f''(\xi(x))
$$  
The midpoint formula takes one point in the middle, and one point on either side, while the endpoint formula takes one point and the two points after it.  
### Example
$$
\begin{array}{c|c|c}
x & f(x) & f'(x) \\
0 & 0 \\
0.2 & 0.7 \\
0.4 & 1.3 \\
0.6 & 2.3
\end{array}
$$  
The goal here is to complete the table. You have to choose which 3 point formula is superior in this case. It should be obvious that you can't use the midpoint formula for \\(x = 0\\) or \\(x = 0.6\\) as they have no points before them, or after them, respectively. So we'll start with using the endpoint formula on \\(x = 0 \\).  
$$
\frac{f'(0) = -3f(0)+4f(0+0.2)-f(0+2(0.2))}{2(0.2)}-\frac{(0.2)^2}{6}f''(\xi(x))
$$  
Solving completely we get:  
$$
\frac{-3(0)+4(0.7)-1.3}{0.4} + \frac{0.04}{6}f''(\xi(x))
$$  
And Finally:  
$$
\frac{15}{4} + \frac{0.04}{6}f''(\xi(x))
$$  
As you can see, this is a relatively simple plug and chug method. For the two in the middle, the midpoint formula is easier to use. For the last one, \\(x = 0.6\\), you'll use the endpoint formula again, only this time with \\(-h\\).  
**NOTE: The midpoint formula works with \\(-h\\), meaning you can do it either with the next two points, or previous two points**
### A general formula
Given (n+1) points \\(x_0, x_1,...,x_n\\):  
The (n+1)point formula is given by:  
$$
f'(x_0) = \sum\limits_{k=0}^nf(x_k)L'_{n,k}(x) + D_x\left(\frac{f^{n+1}(\xi(x))}{(n+1)!}(x-x_0)...(x-x_n)\right)
$$  
The \\(D_x\\) term above is the error term. The (n+1)-point formula is an approximation of \\(f'(x_0)\\) of order \\(O(h^n)\\). Because the technique converges faster the more points you use, you might think that more points, and smaller h are better, since you'll get to a more accurate answer faster. However, because of the massive amount of calculations, round-off error is a real concern, so you have to find a balance between accuracy, convergence speed, and avoiding round-off error.  
In fact, you can say that, **In general, Numerical differentiation methods are UNSTABLE**. i.e. because of round-off error, the error does not necessarily approach zero as h approaches zero.
