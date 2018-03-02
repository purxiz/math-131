---
title: lecture 10 (Starting Numerical Integration, Trapezoid Rule, and Simpson's Rule)
---
## Lecture 10
## Analytic vs Numerical Method

In calculus, you do something like:  
$$
f^n(x) \leftarrow f(x) \rightarrow \int_a^bf(x)dx
$$  
In Numerical, we start with the Lagrange Interpolant, and do some other stuff:  
$$
????? \leftarrow \textrm{Lagrange Interpolant} \rightarrow ????
$$  
Actually, it's pretty simple, we essentially just transform the function into a polynomial using the Lagrange Interpolant, and then we can differentiate that polynomial. It's really easy to differentiate polynomials, so that's a pretty solid method.  
So for our differentiation, we simply have:  
$$
f'(x)=P'(x) + error' \leftarrow \textrm{Lagrange Interpolant} \rightarrow ????
$$  
Where P is our Lagrange Polynomial. Integration, should be obvious, is just given by:  
$$
f'(x)=P'(x) + error' \leftarrow \textrm{L I} \rightarrow \int_a^bf(x)dx=\int_a^bP(x)dx+\int_a^b\textrm{error }dx
$$  
## Numerical Integration
Today we'll be looking at the (n+1) point formula again. This time the Newton's-Cotes version, which converges as a rate of \\(O(h^{2n+1})\\).  
Suppose our goal is to compute  
$$
\int_a^bf(x)dx
$$  


![graph](https://i.imgur.com/MWKwbqS.png)
Graphically, our goal is to calculate the area under the curve between a and b. If we look at this for n = 1, i.e. using only two points to approximate, you can see we get a trapezoid! This is known as the **Trapezoid Rule**.  
![graph](https://i.imgur.com/lARMhEv.png)
The area of this trapezoid is simply \\(\frac{f(a)+f(b)}{2}(b-a)\\), so it's pretty easy to calculate. Wowza. You can already do numerical integration. But it's not very accurate. How inaccurate? Let's find out!  
Back to the Lagrange Interpolant:  
$$
f(x) = f(x_0)L_{1,0} + f(x_1)L_{1,1}(x) + \frac{f''(\xi(x))}{2}(x-x_0)(x-x_1)
$$  
Now we just have to integrate it from \\(x_0\\) to \\(x_1\\). To make that more obvious, let's expand the Lagrange Interpolants  
$$
f(x) = f(x_0)\frac{x-x_1}{x_0-x_1} + f(x_1)\frac{x-x_0}{x_1-x_0} + \frac{f''(\xi(x))}{2}(x-x_0)(x-x_1)
$$  
Finally let's replace \\(x_1 - x_0\\) with h  
$$
f(x) = f(x_0)\frac{x-x_1}{-h} + f(x_1)\frac{x-x_0}{h} + \frac{f''(\xi(x))}{2}(x-x_0)(x-x_1)
$$  
\\(f(x_n)\\) and \\(h\\) are both constants, so we can pull them out of the integral we're about to do:  
$$
\frac{-f(x_0)}{h}\int_{x_0}^{x_1}(x-x_1)dx+\frac{f(x_1)}{h}\int_{x_0}^{x_1}(x-x_0)dx + \frac{1}{2!}\int_{x_0}^{x_1}f''(\xi(x))(x-x_0)(x-x_1)dx
$$  
If we solve that integral:  
$$
\int_{x_0}^{x_1}f(x)dx = \frac{-f(x_0)}{h}\left[\frac{x^2}{2}-x_1x\right]_{x_0}^{x_1} + \frac{f(x_1)}{h}\left[\frac{x^2}{2}-x_0x\right]_{x_0}^{x_1} + \textrm{error}
$$  
If we plug in the x's, and then simplify, we get something like:  
$$
\int_{x_0}^{x_1}f(x)dx = \frac{h}{2}f(x_0)+\frac{h}{2}f(x_1) + \textrm{error}
$$  
If you're having trouble doing that simplification, don't forget that \\(x_1-x_0 = h\\). That might help.  
Now we just have that pesky error term remaining.  
$$
\frac{1}{2!}\int_{x_0}^{x_1}f''(\xi(x))(x-x_0)(x-x_1)dx
$$  
The \\(f''\\) term is definitely the most ugly part of that integral, so let's just bring it out. We can do this because \\(\xi\\) is bound by the \\([x_0,x_1]\\) interval, so we can use some Mean Value Theorem magic to just bring it out: We'll end up with:  
$$
\frac{-h^3}{12}f''(c) \textrm{ where } c \in [x_0,x_1]
$$  
as our error. Remember that was all for n = 1, aka using two points, aka the Trapezoid rule. If we want to get a little bit fancier, we can use **Simpson's rule**, which is using n = 2, aka 3 points. Now, instead of having a straight line to cross the gap between \\(a,f(a)\\) and \\(b,f(b)\\), we'll make a quadratic polynomial using the three points. You can solve for the error in Simpson's Rule in the same way as we did for the Trapezoid Rule, but the integration is a lot uglier, because there's so many points. It's not really particularly difficult, and is a great exercise to do yourself, but, it is rather long. Anyway, here's the final result of those mathematics. Note the error here converges at order 5 when using three points (n = 2), as we mentioned at the very beginning, these formulas converage at a rate of \\(O(h^{2n+1})\\).  
$$
\int_{x_0}^{x_1}f(x)dx = \frac{h}{3}(f(x_0)+4f(x_1)+f(x_2))-\frac{h^5}{90}f^4(c)
$$  
#### Example
$$
\int_0^2x^2dx=?
$$  
using
1. Exact
2. Trapezoid
3. Simpson's
Exact is pretty simple, just integrate to get  
$$
\int_0^2x^2dx=\left[\frac{x^3}{3}\right]_0^2 = \frac{8}{3}
$$  
For Trapezoid, we just plug in to the trapezoid formula from above:  
$$
\int_0^2x^2dx \approx \frac{2}{2}(f(0)+f(2)) = 4
$$  
For Simpson's Rule, we plug in again:  
$$
\int_0^2x^2dx \approx \frac{1}{3}(0 + 4 + 4) = \frac{8}{3}
$$  
The take-away? Simpson's Rule is exact for a polynomial of degree three. Also, it's generally more accurate than the trapezoid rule, without being terribly more difficult. These are all called **Newton's-Cotes** methods. In Newton's-Cotes, you can have open or closed.   
* Closed Newton's-Cotes: Take endpoints into account
* Open Newton's-Cotes: No endpoints (aka midpoint)
