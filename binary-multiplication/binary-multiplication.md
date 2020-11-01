---
title: Binary Multiplication
tags: codenewbie, learning, beginners
---

Imagine if we had only two fingers instead of ten. Would we still use Decimal counting, base-10, zero to nine? Or we would naturally use two digits: zero and one to count: binary numeral system. The following shows some examples of binary numbers (indicated with subscript {% katex inline %}\_2{% endkatex %}) and their Decimal counterparts.

{% katex %}
\begin{aligned}
0_2 &= 0 \\\\
1_2 &= 1 \\\\
10_2 &= 2 = 1+1\\\\
11_2 &= 3 = 1+1+1\\\\
100_2 &= 4 = 1+1+1+1 \\\\
101_2 &= 5 = 1+1+1+1+1\\\\
\end{aligned}
{% endkatex %}

At first sight, the binary system might look confusing. However, the arithmetic operations on the binary number are essentially the same as we have on a Decimal one: we still have carry-over in summation and borrowing in subtraction.

The same applies to multiplication. First, let's try to do simple multiplication on decimal numbers:

{% katex %}
999 \times 111
{% endkatex %}

![Multiplication on decimal numbers.](./images/decimal-mul.png)

Here, we can see that in column 3, we have {% katex inline %}9+9+9+1=28{% endkatex %}
and carry over the "extra digit" 2 to the next decimal place.

Let's try the same on binary numbers:

{% katex %}
1111_2 \times 111_2
{% endkatex %}

![Multiplication on binary numbers.](./images/binary-mul.png)

Again, in column 3, we have {% katex inline %}1+1+1+1=100_2{% endkatex %}. What we didn't see in the example of Decimal numbers above is: Here, we need to carry over the additional digits in {% katex inline %}100_2{% endkatex %} to the next and the following binary places: {% katex inline %}1{% endkatex %} to column 5 and {% katex inline %}0{% endkatex %} to column 4.
