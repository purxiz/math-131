## Lecture 8
## More Divided Differences
\[
f[x_j,x_{j+1}] = \frac{f(x_{j+1}) - f(x_j)}{x_{j+1}-x_j}
\]
Look at the right side of that equation. If you'll notice, it looks sorta like a derivative. In fact, if you want to get really technical, it is a derivative.
Remember the Mean Value Theorem?
There exists \(\xi\in[x_0,x_1]\) such that:
\[
f'(\xi) =  \frac{f(x_1) - f(x_0)}{x_1-x_0} = f[x_0,x_1]
\]
If you'll recall from the end of last lecture, it's totally possible to calculate larger divided differences like \(f[x_0,x_1,x_2]\). We can use this same concept to calculate higher order derivatives using Newton's Divided Differences. Generally:
If \(f\in C^n([a,b]), \exists\xi\in[a,b]\) such that \(f[x_0,...,x_n] = \frac{f^{(n)}(\xi)}{n!}\)

## Limitations of the Lagrage Interpolant
