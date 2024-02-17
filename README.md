# Recurrence Analysis -- Mystery Function

Analyze the running time of the following recursive procedure as a function of
$n$ and find a tight big $O$ bound on the runtime for the function. You may
assume that each operation takes unit time. You do not need to provide a formal
proof, but you should show your work: at a minimum, show the recurrence relation
you derive for the runtime of the code, and then how you solved the recurrence
relation.

```javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n / 3);
        var count = 0;
        mystery(n / 3);
        for(var i = 0; i < n*n; i++) {
            for(var j = 0; j < n; j++) {
                for(var k = 0; k < n*n; k++) {
                    count = count + 1;
                }
            }
        }
        mystery(n / 3);
    }
}
```

Add your answer to this markdown file. [This
page](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
might help with the notation for mathematical expressions.

# Answer

The three recursive calls in the `else` clause will take $\mathrm{T}(\frac{n}{3}) + \mathrm{T}(\frac{n}{3}) + \mathrm{T}(\frac{n}{3}) =3\mathrm{T}(\frac{n}{3})$ steps to complete. Additionally, the nested for loops will take $n^{2} \cdot n \cdot n^2 = n^5$ steps to complete. Finally, there is a constant term beneath the first recursive call which would add a constant of $1$ to the time complexity of the `mystery` function.

First, I will solve the recurrence relation for this implementation.


$$\begin{gather*}
\mathrm{T}(n) =
 \begin{cases}
1 && \text{if $n \leq 1$} \\
n^5 + 3\mathrm{T}(\frac{n}{3}) + 1 && \text{if $n > 1$} \\
\end{cases}
\end{gather*}
$$

$$\begin{gather}
& \mathrm{T}(n) = n^5 + 3\mathrm{T}(\frac{n}{3}) + 1 \\
\implies & \mathrm{T}(n) = n^5 + 3((\frac{n}{3})^5 + 3\mathrm{T}(\frac{n}{9}) + \frac{1}{3}) + 1 \notag \\
\implies & \mathrm{T}(n) = \frac{n^{5}}{3^{4}} + 9\mathrm{T}(\frac{n}{9}) + 2 \\
\implies & \mathrm{T}(n) = \frac{n^5}{3^{4}} + 9((\frac{n}{9})^5 + 3\mathrm{T}(\frac{n}{27}) + \frac{2}{3}) + 1 \notag \\
\implies & \mathrm{T}(n) = \frac{n^5}{3^4} + \frac{n^5}{9^4} + 27\mathrm{T}(\frac{n}{27}) + 3 \\
& \vdots \notag \\
\implies & \mathrm{T}(n) = \frac{n^5}{(3^{i})^{4}} + \dots + \frac{n^{5}}{(3^{i - i})^4} + 3^{i}\mathrm{T}(\frac{n}{3^{i}}) + \text{ some constant $c$} \tag{$i$} \\
\end{gather}
$$

In order to get to the base case of $n \leq 1$, it would make the most sense to substitute $\log_{3}(n)$ for the arbitrary $i$ term because we know that the number of steps required to get from $1$ to $n$ can be modeled as the **height** of a ternary tree. Moreover, we know the height of a ternary tree to be $\log_{3}(n)$ where $n$ represents the discretionary input size.

$$\begin{gather*}
\implies & \mathrm{T}(n) = \frac{n^5}{(3^{\log_{3}(n)})^4} + \dots + n^{5} + 3^{\log_{3}(n)} \mathrm{T}(\frac{n}{3^{\log_{3}(n)}}) + c \\
\implies & \mathrm{T}(n) = \frac{n^5}{n^4} + \dots + n^5 + n\mathrm{T}(\frac{n}{n}) + c \\
\implies & \mathrm{T}(n) = n^5 + \dots + n + n\mathrm{T}(1) + c \\
\implies & \mathrm{T}(n) = n^5 + \dots + 2n + c \\
\implies & \mathrm{T}(n) \in \mathrm{\Theta}(n^5) \\
\implies & \mathrm{T}(n) \in \mathrm{O}(n^5) \\
&& \blacksquare
\end{gather*}
$$

By simplifying the recurrence relation above, we can see that no matter how large $n$ grows, the highest order term in the `mystery(n)` time function will **always** be $n^5$. Therefore, we can disregard the lower order terms and conclude that `mystery` is an element of $\mathrm{\Theta}(n^5)$ which inherently means that `mystery` is also an element of $\mathrm{O}(n^5)$.