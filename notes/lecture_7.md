## Lecture 7

## Error Analysis of Lagrange Interpolant
Let's remember that the Lagrange polynomial is given by:
\[
L_{n,k}(x) = \frac{\prod\limits_{j=0,j \ne k}^n(x-x_j)}{\prod\limits_{j=0,j \ne k}^n(x_k-x_j)}
\]
The error term is pretty straightforward:
\[
f(x) = P(x) + \frac{f^{n+1}(\xi(x))}{(n+1)!}\prod\limits_{j=0}^n(x-x_j)
\]
This should remind you of a Taylor series, since it's a polynomial (in this case the Lagrange polynomial), plus an error term. The general form of a Taylor Polynomial is
\[
f(x) = P_n(x) + R_n(x)
\]
In general,
\[
R_n(x) = \frac{f^{n+1}(\xi(x))}{(n+1)!}(x-x_0)^{n+1}
\]
For now, let's just realize that the Lagrange Polynomial Error is very similar to that of a Taylor Series, and we'll move on.

## Divided Differences
Goal: Rewrite \(P(x)\) from our Lagrange Polynomials in a more efficient way. We're going to do exactly the same thing, but we want to be able to compute it faster. The Lagrange Interpolant method we discussed in the previous lecture does *a lot* of multiplication. It's nice that P is unique, but we need to calculate every \(L_{n,k}\).
Let's get more specific with our goal. We want to express \(P(x)\) in a more specific way.
\[
\begin{aligned}
P(x) = a_0 + a_1(x-x_0) + a_2(x-x_0)(x-x_1) + a_3(x-x_0)(x-x_1)(x-x_2) + ... + a_n(x-x_0)(x-x_1)(x-x_n-1)
\end{aligned}
\]
A simple example is factor expression:
\[
P(x) = (x-a)(x-b) = ab - (a+b)x + x^2
\]
You can see that the last function noted above matches our desired definition.
Let's start out by figuring out how to find \(a_0, a_1\), etc.
#### To find \(a_0\)
\[
P(x_0) = a_0 + 0 + 0 + ... + 0
\]
Since every other term has an \(x-x_0\) term.
From there, we can simply think about the definition of a Lagrange Polynomial. The goal of a Lagrange Polynomial is to approximate a function given some points \(x_0, x_1, ..., x_k\), and their corresponding \(f(x_0), f(x_1),...,f(x_k)\). Given that we are currently evaluating around \(x_0\), we can simply say that
\[
P(x_0) = a_0 = f(x_0)
\]
If this result is confusing to you, I recommend re-reading the notes from the last lecture.
#### To Find \(a_1\)
We just do the same thing, except evaluate around \(x_1\).
\[
P(x_1) = f(x_0) + a_1(x_1-x_0) + 0 + ... + 0
\]
We can then say in the same way that
\[
P(x_1) = f(x_1) = f(x_0) + a_1(x_1-x_0)
\]
Solving for \(a_1\), we then get
\[
a_1 = \frac{f(x_1) - f(x_0)}{x_1-x_0}
\]
#### To Find \(a_2\)
Believe it or not, same thing again, but this time evaluated around \(x_2\).
To make things a little bit simpler, we're going to start using brackets.
\[
f[x_0] = f(x_0) \textrm{ and } f[x_1] = f(x_1) \textrm{ and etc.}
\]
What makes this notation interesting is that it can take multiple arguments.
\[
f[x_0,x_1] = \frac{f(x_1) - f(x_0)}{x_1-x_0}
\]
Or more generally:
\[
f[x_j,x_{j+1}] = \frac{f(x_{j+1}) - f(x_j)}{x_{j+1}-x_j}
\]
With our new handy dandy notation, we can easily say that \(a_1=f[x_0,x_1]\). Anyway, now that all that notation business is out of the way, let's take a glance at actually finding \(a_2\).
\[
P(x_2) = f(x_2) = a_0 + a_1(x_1-x_0) + a_2(x_2-x_0)(x_2-x_1)
\]
We can then solve for \(a_2\) using algebra.
\[
a_2 = \frac{f(x_2) - a_0 - a_1(x_2-x_0)}{(x_2-x_0)(x_2-x_1)}
\]
A cool exercise is to actually solve this all out by replacing \(a_0\) with \(f[x_0]\) and \(a_1\) with \(f[x_0,x_1]\), and so on. From here a pattern should start to become apparent. You'll eventually be able to see the general form emerge if you do \(a_3\) and so on. If you're lazy though, this is what you get for \(a_2\).
\[
a_2 = \frac{f[x_1,x_2] - f[x_0,x_1]}{x_2-x_0} = f[x_0,x_1,x_2]
\]
Then, we can get all the way to the idea of Divided Differences. Here's the general form:
\[
f[x_j,x_{j+1},...,x_{j+k}] = \frac{f[x_{j+1},...,x{j+k}]-f[x_j,...,x_{j+k-1}]}{x_{j+k}-x_j}
\]
You'll notice the term on the right of the numerator in the above equation is just the term on the left, but shifted left by one.

#### Putting it all together
Anyway, if you remember all the way back, our original goal was to write \(P(x)\) in a more computationally efficient way.  We'll just start right out with the answer:
\[
P(x) = f(x_0) + \sum\limits_{k=1}^n\left( f[x_0,...,x_k]\prod\limits_{i=0}^{k-1}x-x_i\right)
\]
If you'd prefer an alternate notation for the product operator, you could also write that as:
\[
P(x) = f(x_0) + \sum\limits_{k=1}^nf[x_0,...,x_k](x-x_0)(x-x_1)...(x-x_{k-1})
\]
This Lagrange Interpolant can be computed easily by using **Newton's Divided Differences**
Let's start with a quick example:
\[
\begin{array}{c}
x_0 = 0, x_1 =1, x_2 = 2, x_3 = 3 \\
f(x_0) = 1.3, f(x_1) = 1, f(x_2) = 0.5, f(x_3) = 0.2
\end{array}
\]
What does \(P(x)\) equal?
Well, let's let's start with \(k=1\) for the above equation, and get:
\[
f(x_0) + \frac{f(x_1) - f(x_0)}{x_1-x_0}(x-x_0) + \sum\limits_{k=2}^3f[x_0,...,x_k](x-x_0)(x-x_1)...(x-x_{k-1})
\]
Note that the sum now goes from \(k=2\) to \(3\), since we just expanded the sum for \(k = 1\). It becomes apparent that we will need to solve for \(f[x_0,x_1,x_2]\) next. In order to do that, we also need to know \(f[x_1,x_2]\). You can see the notes for the general form of \(f[]\) notation above if that's confusing. It becomes apparent that we can solve this problem with the following to help us out:
**TODO:** Make a graphic for the solution tree thing
After solving through all but the last row those, we can find the following:
\[
f[x_0,x_1,x_2] = -0.1
\]
From that knowledge, our knowledge of \(f[x_0]\), and our knowledge of \(f[x_0,x_1]\), we can say that:
\[
P(x) = 1.3 - 0.3(x) - 0.1 (x)(x-1) + f[x_0,x_1,x_2,x_3](x-x_0)(x-x_1)(x-x_2))
\]
We haven't solved for \(f[x_0,x_1,x_2,x_3]\) yet, so the solution isn't quite complete, but if you were to compute the value, the problem would be solved, and we would have found the Lagrange Interpolant for the given values.
