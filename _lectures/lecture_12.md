---
title: Lecture 12 (Round Off Error Stability and ODEs)
---

Today we will be talking about round-effor error stability and other quadrature rules.
## Round off Error Stability
Suppose that we make an error while evaluating \\(f(x_k)\\) on \\(k = 0,...,n\\).  
$$
f(x_k) = {\tilde{f}}(x_k) + e_k
$$  
In this case, \\(\tilde{f}(x_k)\\) is our approximation, and \\(e_k\\) is our error. Let's say:  
$$
E(h) = \textrm{`Composite Simpsons rule'} - \textrm{`Composite Simpsons Rule with '} \tilde f(x_k)
$$  
$$
= \frac{h}{3}\sum\limits_{k=0}^{\frac{n}{2}-1} f(x_{2k})- \tilde f(x_{2k})+4(f(x_{2k+1})-\tilde f(x_{2k+1}) + f(x_{2k+2})-\tilde f(x_{2k+2})
$$  
$$
E(h) = \frac{h}{3}\sum\limits_{k=0}^{\frac{n}{2}-1}(e_{2k}+4e_{2k+1}+e_{2k+2})
$$  
$$
\|E(h)\| = \|\frac{h}{3}\sum\limits_{k=0}^{\frac{n}{2}-1}(e_{2k}+4e_{2k+1}+e_{2k+2}) \|
$$  
$$
\leq \frac{h}{3}\sum\limits_{k=0}^{\frac{n}{2}-1}(\|e_{2k}\|+4\|e_{2k+1}\|+\|e_{2k+2}\|)
$$  
Hopefully you followed all of that. We are now going to make one more logical leap, which is that \\(\|e_k\| \leq \epsilon \\). Once we've accepted this, you can see that the following holds:  
$$
E(h) \leq 2h \sum\limits_{k=0}^{\frac{n}{2}-1} \epsilon
$$  
Epsilon at this point is just a constant, so we can simply multiply it \\(\frac{n}{2}\\) times:  
$$
E(h) \leq 2h\frac{n}{2}\epsilon
$$  
Now recall that \\(h=\frac{b-a}{n}\\), so if we replace the h, and cancel out the twos, we get:  
$$
E(h) \leq (b-a)\epsilon
$$  
Our final result is that our error does not depend on \\(h\\), so it is is Stable! 
## Initial Value Problems for ODEs
Well-posed Initial Value Problems are of the following form:  
$$
\frac{dy}{dt}=f(y,t), \textrm{ } a \leq t \leq b,\textrm{ } y(a) = \alpha
$$  
### example
$$
\frac{dy}{dt} = 2y + e^{-t}+\frac{y^2}{t}
$$  
This is a really terrible problem to solve, so who cares about the exact solution. Let's just figure out a numerical solution! 
Our first questions to ask before doing any solving:  
1. Does the IVP have a solution? (Existence)
2. Is this solution uniqueness (Uniqueness)
3. Is the solution STABLE? (Stability)
We will talk about stability in many different ways, so be careful about which test for stability you are applying in the future. Note: Stability implies that a small perturbation of the initial condtion and/or equation only imply small perturbations of the solution. 
