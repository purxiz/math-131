---
title: lecture 5
---
## Lecture 5

## Newton's Method
Newton's Method is the most powerful root finding algorithm.
There are several methods to find Newton's Method, the first is to use Taylor's Theorem, reproduced here:  
$$
f(x) = f(x_o) + \frac{f'(x_o)}{1!}(x-x_o) + \frac{f''(x_o)}{2!}(x-x_o)^2 + \frac{f^n(\xi(x))}{n!}(x-x_o)^n
$$

Suppose
$$
f\epsilon C^2([a,b])
$$
To solve a root-finding problem, we are ultimately trying to solve the simple equation \(f(x)=0\).  
So, given that the Taylor Polynomial is equal to \(f(x)\), we are simply trying to solve:  
$$
0 = f(x_o) + \frac{f'(x_o)}{1!}(x-x_o) + \frac{f''(x_o)}{2!}(x-x_o)^2 + \frac{f^n(\xi(x))}{n!}(x-x_o)^n
$$  
If we assume that x and \(x_0\) are close together, then we can say that the \(x-x_0\) term in each part of the Taylor Polynomial is close to zero. And even more importantly, that the error term is very close to zero.  
\[
0 \approx f(x_o) + \frac{f'(x_o)}{1!}(x-x_o)
\]
The above is what we are left with taking a Taylor Polynomial with n = 1, and discarding our small error term. We can algebraically manipulate the above to get:
\[
x \approx x_0 - \frac{f(x_0)}{f'(x_0)}
\]

The above is Newton's Method. Note that an important constraints is that \\(f'(x_0) \ne 0\\). This doesn't make the method weaker, because if \\(f'(x_0) = 0\\), then you can see \\(f(x_0)\\) must equal zero, and therefore \\(x_0\\) is a root.

### Geometric Interpretation of Newton's Method
Start with the equation for the tangent line:
\[
y=f(x_0)+f'(x_0)(x-x_0)
\]
![Tangent Lines](https://i.imgur.com/Um6kQQx.png)
If you take the tangent line at the point \(x_0,f(x_0)\) shown in the image above, you will see that it appears somewhat close to the root. The closer your \(x_0\) gets to the true root x, the closer the tangent line will also be. Much like bisection method, this is an iterative approach where we move closer and closer to the true root with each iteration.<br>
To get our new \(x_0\) for the next iteration, we can simply look at where the tangent line of the previous \(x_0\) intersects with the x-axis.

for 
![Newton's Method](https://i.imgur.com/mbKSRDw.png)
img src: math.lamar.edu

## Pseudocode
```
  x0 = 1 %The initial value
  f = @(x) x^2 - 2 %The function whose root we are trying to find
  fprime = @(x) 2*x %The derivative of f(x)
  tolerance = 10^(-7) %7 digit accuracy is desired
  epsilon = 10^(-14) %Don't want to divide by a number smaller than this

  maxIterations = 20 %Don't allow the iterations to continue indefinitely
  haveWeFoundSolution = false %Have not converged to a solution yet

  for i = 1 : maxIterations

   y = f(x0)
   yprime = fprime(x0)

   if(abs(yprime) < epsilon) %Don't want to divide by too small of a number
   % denominator is too small
   break; %Leave the loop
   end

   x1 = x0 - y/yprime %Do Newton's computation

   if(abs(x1 - x0) <= tolerance * abs(x1)) %If the result is within the desired tolerance
   haveWeFoundSolution = true
   break; %Done, so leave the loop
   end

   x0 = x1 %Update x0 to start the process again

  end

  if (haveWeFoundSolution)
   ... % x1 is a solution within tolerance and maximum number of iterations
  else
   ... % did not converge
  end
```
The code given in lecture is given on the lecture slides.

### Pros and Cons
You only need one guess for Newton's Method to work, it converges very quickly, and it can find roots of functions that only "touch" the x-axis, like \(f(x)=x^2\).<br>
On the other hand, Newton's Method does not always converge. You also need a GOOD guess. You also need \(f'(x) \ne 0\).
### What if we don't know f prime
You can simply approximate f prime by
\[
f' \approx \frac{f(x)-f(x_0)}{x-x_0}
\]
If you use this approximation to replace f' in Newton's Method, it is known as the SECANT Method.
\[
x_i+1 = x_i - \frac{f(x_i)}{\frac{f(x_i)-f(x_{i-1})}{x_i-x_{i-1}}}
\]
This is now your iterated term for SECANT Method.
### Rate of Convergence
If you have an initial guess that is close to the solution, it converges very quickly.
