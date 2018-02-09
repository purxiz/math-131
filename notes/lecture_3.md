
## Lecture 3

## Rate of Convergence
Suppose:
\[
\beta_n \rightarrow 0 \textrm{ as } n \rightarrow \infty \textrm{ and } \alpha_n \rightarrow \alpha \textrm{ as } n \rightarrow \infty
\]
If \(|\alpha_n-\alpha| \leq C|\beta_n|\), for \(n\) large enough, \(C\) being a constant, then \(alpha_n\) converges to \(\alpha\) with the rate \(O(\beta_n)\), aka
\[
\alpha_n=\alpha+O(\beta_n)
\]
NB: \(O(f(n))\) is referred to as "Big-O" notation
### example
\[
y_n=a+\frac{b}{n}\rightarrow a \textrm{ as } n \rightarrow \infty
\]
AND
\[
\frac{1}{n} \rightarrow 0 \textrm{ as } n \rightarrow \infty
\]
plug it in to the above definition:
\[
|y_n-a|=|\frac{b}{n}| \leq |b||\frac{1}{n}|
\]
Therefore \(y_n\) converges to \(a\) with a rate of convergence of \(O(\frac{1}{n})\)
### Notations (other way (not big-O))
Suppose
\[
G(h) \rightarrow 0 \textrm{ as } h \rightarrow 0 \textrm{ and } F(h) \rightarrow L \textrm{ as } h \rightarrow 0
\]
Then, if
\[
|F(h)-L| \leq C|G(h)|
\]
F(h) converges to L with a rate of convergence \(O(G(n))\)
## example
\[
f(h)=cos(h)
\]
Find the rate of convergence of \(f\) as \(h \rightarrow 0\)
1. Using Taylor's Theorem with n=1
2. Using Taylor's Theorem with n=3

Reclal that the Taylor Theorem expansion is of the form
\[
f(x) = f(x_o) + \frac{f'(x_o)}{1!}(x-x_o) + \frac{f''(x_o)}{2!}(x-x_o)^2 + \frac{f^n(\xi(x))}{n!}(x-x_o)^n
\]
The Taylor Theorem of for 1. above, would therefore include term 0 and 1, and the remainder term, so:
\[
f(x) = f(x_o) + \frac{f'(x_o)}{1!}(x-x_o) + \frac{f''(\xi(x))}{2!}(x-x_o)^2
\]
plugging in values for cos(x) as given in the example
\[
f(x) = cos(0) -sin(0)(h-0) - \frac{cos(\xi(h))}{2}h^2
\]
\(cos(h) = F(h)\) from our notation definition above, so we can plug into the formula, also above
\[
|cos(h)-1|=|\frac{cos(\xi(h))}{2}h^2| \leq \frac{1}{2}h^2
\]
The \(\frac{1}{2}\) comes from the fact that cos is bound between -1 and 1, so the maximum value of \(\frac{cos(\xi(h))}{2}h^2\) is \(\frac{1}{2}\).
The rate of convergence is therefore \(O(h^2)\). Problem 2 from the example will be left to the reader.
## Root Finding Problems
Goal: Solve \(f(x)=0\)
for an easy example, Solve \(x^2-1=0\). However, for more complicated examples, the solution cannot easily be calculated without numeric solutions. For example, try to solve:
\[
cos(x) -x =0
\]
If you can't do it, don't worry. That's why numeric methods exist. We will study three methods for root finding problems. They are all very efficient, but all have benefits and drawbacks.
### Method 1: Bisection Method
Let's say I wanted to solve \(f(x) = 0\) over some interval \([a,b]\). For now, we will simply assume \(f\in C([a,b])\), i.e. that \(f\) is continuous over the interval \([a,b]\). Let's say that:
\[
f(a)*f(b)\lt0
\]
We can easily see that if this is true, then \(f(a)\) and \(f(b)\) have opposite signs. For an extremely simple example, we can look at the graph of \(f(x)\):
![Graph](https://i.imgur.com/6UMJ2gv.png)
Let's say that \(a=-1\) and \(b=1\), so \(f(a)=-1\) and \(f(b=1)\). From here it should be obvious that somewhere between -1 and 1, the line must cross the x axis at least once. This is called the Intermediate Value Theorem. In math terms:
\[
\exists c \in [a,b] / f(c) = 0
\]
Or in English, there exists some point \(C\) in \([a,b]\) such that \(f(c)=0\). The idea of the bisection method is to continue halving the interval, and comparing the signs of \(f(a)\) and \(f(b)\) to get closer and closer to the root. <br>
Bisection is nice because it always finds a solution. However, it doesn't necessarily find ALL solutions. For example, if there are multiple zeros in \([a,b]\), bisection will not find more than one. It is also relatively slow to converge.
### Basic Pseudocode for Bisection Method
```
  INPUT: Function f, endpoint values a, b, tolerance TOL, maximum iterations NMAX
  CONDITIONS: a < b, either f(a) < 0 and f(b) > 0 or f(a) > 0 and f(b) < 0
  OUTPUT: value which differs from a root of f(x)=0 by less than TOL

  N ← 1
  While N ≤ NMAX # limit iterations to prevent infinite loop
    c ← (a + b)/2 # new midpoint
    If f(c) = 0 or (b – a)/2 < TOL then # solution found
      Output(c)
      Stop
    EndIf
    N ← N + 1 # increment step counter
    If sign(f(c)) = sign(f(a)) then a ← c else b ← c # new interval
  EndWhile
  Output("Method failed.") # max number of steps exceeded
  ```
The pseudocode gone over in class can be found in the lecture slides.
### Rate of Convergence for Bisection method
If c is the actual root, we know that the error is bounded by:
\[
|x_i-c| \leq \frac{b_i-a_i}{2}
\]
Now for a bit of a logical leap, it is also okay to say that:
\[
\frac{b_i-a_i}{2} = \frac{b-a}{2^i}
\]
This is okay to say because the \(b_i-a_i\) term is always half of \(b_{i-1}-a_{i-1}\). Plugging back in we get:
\[
|x_i-c| \leq (b-a)(\frac{1}{2^i})
\]
Going all the way back to our Big-O notation definition at the top of this page, we can see that it converges at a rate of \(O(\frac{1}{2^i})\).
