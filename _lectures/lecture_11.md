---
title: Lecture 11   (Composite Rules for Numerical Integration)
---
## Lecture 11

### Last time
We took a look at the **Newton's Cotes Formula**, which is defined for all \\(n \geq 1\\), and has an order of \\(O(h^{2n+1})\\).  
There are some special cases we discussed in the previous lecture:  
* **Trapezoid Rule** for \\(n = 1\\) we look at the points \\(x_0 \\) and \\(x_1 \\).
* **Simpson's Rule**  for \\(n = 2\\) we look at the points \\(x_0 \\), \\(x_1\\) and \\(x_2 \\).

## Composite Rules
Today we will be looking at composite rules, which involve splitting our original problem into sub-intervals, and applying Simpson's or Trapezoid rule to each of those subintervals.
![Simpson's and Trapezoid rules](https://i.imgur.com/kKfPqxw.png)  
In the above image, you can see a simple application of Trapezoid (blue), and Simpson's (pink) rules to a graph of some function. You can see that in this case, we only consider the entire interval.   
![more intervals](https://i.imgur.com/RMGhkxa.png)  
If we add some intervals, you can see that our estimation is actually made up of multiple trapezoids, in the case of Trapezoid Rule, or multiple quadratic approximations, in the case of Simpson's Rule.   
![Even More Intervals](https://i.imgur.com/3dr1tcL.png)  
Here you can see that including even more intervals further increases the accuracy of our numerical integration.   
Let's take a look at how that would work with the Trapezoid Rule. Recall that the Trapezoid Rule is represented by:  
$$
\int_{x_0}^{x_1}f(x)dx = \frac{h}{2}\left(f(x_0)+f(x_1)\right) + \textrm{error}
$$   
The following term would then be:  
$$
\int_{x_1}^{x_2}f(x)dx = \frac{h}{2}\left(f(x_1)+f(x_2)\right) + \textrm{error}
$$   
and so on.  
You can see that if we add all these terms together, we'll get something like:  
$$
\int_{x_0}^{x_n}f(x)dx = \sum\limits_{k=0}^{n-1} \left( \int_{x_k}^{x_{k+1}} f(x)dx \right)
$$  
If we separate the formula and error term into two separate sums, we get the **Composite Trapezoid Rule**  
$$
\int_a^bf(x)dx = \frac{h}{2}\sum\limits_{k=0}^{n-1}\left( f(x_k) + f(x_{k+1}) \right) - \frac{h^3}{12}\sum\limits_{k=0}^{n-1}f''(\xi_k(x))
$$  
If you expand that out, you'll notice that for every value except the endpoints, it is added twice. i.e. if you look at the first two terms listed out above, \\(x_1\\) appears twice, and if we continued to list out terms, the same would be true until the last term. Therefore, we can also write the rule as:  
$$
\frac{h}{2}\left( f(x_0) + 2\sum\limits_{k=1}^{n-1}f(x_k) + f(x_{k+1}) \right) - \frac{h^3}{12}nf''(\xi(x))
$$  
Let's take a quick gander at the error term above. We've somehow ended up with an \\(n\\) hanging out in there. From way back when, remember that h is the "length" of each of our subintervals, and therefore \\(h = \frac{b-a}{n}\\). From there some simple algebra reveals that \\(n=\frac{b-a}{h}\\). If we look at our error term:  
$$
\frac{h^3}{12}nf''(\xi(x))
$$  
You can replace n with \\(\frac{b-a}{h}\\), and get our final error term:  
$$
\frac{h^2}{12}(b-a)f''(\xi(x))
$$  
You can see that because of this, when we use composite rule with the trapezoid rule, we only converge at \\(O(h^2)\\), as opposed to the \\(O(h^3)\\) for the Trapezoid rule.  
The only question remaining is why we slapped an n into the error term in the first place. This is a result of using the mean value theorem. Our claim is that because \\(\xi(x)\\) is bound on the interval [a,b], we can simply evaluate it once in that interval, for it's maximum, and then multiply it by the number of error terms, in this case \\(n\\).  

### Composite Simpson's Rules
We will derive the composite Simpson's Rule in a very similar way. Start with listing out a couple of terms:  
$$
\int_{x_0}^{x_2} f(x)dx = \frac{h}{3}\left( f(x_0) + 4f(x_1) + f(x_2) \right)
$$  
  
$$
\int_{x_2}^{x_4} f(x)dx = \frac{h}{3}\left( f(x_2) + 4f(x_3) + f(x_4) \right)
$$  
I've left out the error terms here for brevity, but you can look at the previous lecture to see them, or just keep reading.  
This sum is actually interesting, since we have to go interval by interval, and each interval contains 3 points, we actually have to "skip" some values in our sum. You can see above how a simple sum that incremented by one every time wouldn't work.  
$$
\sum\limits_{k=0}^{\frac{n}{2}-1}\int_{x_{2k}}^{x_{2k+2}}f(x)dx
$$  
The above is one way of solving that problem. We just take our limits as 2k, to skip odd numbers. We have to adjust our maximum to \\(\frac{n}{2}-1\\) to ensure that \\(2k\\) doesn't go above n. If we expand that sum out, we get:  
$$
\frac{h}{3} \sum\limits_{k=0}^{\frac{n}{2}-1}\left( f(x_{2k}+4f(x_{2k+1}) + f(x_{2k+2})) \right) - \frac{h^4}{180}(b-a)f^{4}(\xi(x))
$$    
The error term was derived in the same way as the one for Composite Trapezoid Rule. You'll notice that the original error term for Simpson's Rule had \\(\frac{h^4}{90}\\), but since Simpson's looks at 3 points, our intervals are now 2h wide, which is where the factor of two comes from.)
## Pseudocode

Here is an implementation of composite Simpson's Rule in Python. Everything should be pretty obvious, except maybe `for i in range(1, n, 2)`. In Python, this simply means count from 1 to n, with a step size of 2. It's just ordered differently from matlab
```python
def simpson(f, a, b, n):
    """Approximates the definite integral of f from a to b by the
    composite Simpson's rule, using n subintervals (with n even)"""

    if n % 2:
        raise ValueError("n must be even (received n=%d)" % n)

    h = (b - a) / n
    s = f(a) + f(b)

    for i in range(1, n, 2):
        s += 4 * f(a + i * h)
    for i in range(2, n-1, 2):
        s += 2 * f(a + i * h)

    return s * h / 3c
```
