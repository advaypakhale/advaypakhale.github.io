---
date:
  created: 2025-06-12
readtime: 10
pin: true
tags:
  - mathematics
  - analysis
---

# A Note on Differentiability

The purpose of this note is to collect together some basic ideas and intuitions for the concept of differentiability.

We define say that a function $f: \mathbb{R} \to \mathbb{R}$ is differentiable at some point $a$ if the limit

$$
f'(a) = \lim_{h \to 0}{\frac{f(a + h) - f(a)}{h}}
$$

exists, and $f'(a)$ is termed the derivative at the point $a$.

## Linear Approximations

However, the "right" way of thinking about a function being differentiable at some point is that it admits a linear approximation at that point; when we zoom close enough in, the function in fact behaves linearly around that point. Thus when we are sufficiently close to $a$, we have a linear approximation

$$
f(a + h) \approx f(a) + hf'(a)\,.
$$

Rearranging the above equation, we get exactly the usual limit definition of a derivative ($h$ is small here). The derivative is thus the linear approximation of the function.

## Removing the quotient

Extending the above limit definition to higher dimensions, however, isn't immediate, since we don't have a meaningful way to represent the quotient by a vector $h$. However, we may write

$$
g(h) = {\frac{f(a + h) - f(a)}{h}} - f'(a)
$$

where $g(h)$ tends to $0$ as $h$ tends to $0$. Getting rid of the quotient by $h$, we have

$$
f(a + h) - f(a) = f'(a)h + hg(h)\,.
$$

Defining $g(0)=0$, then we can extend the relation to the case when $h=0$. Further noting that we can replace $h$ with $-h$ if we replace $g$ with $-g$, we have shown that if $f$ is differentiable at $a$, then there exists some $g$ satisfying $\lim_{h \to 0}g(h)=0$ such that


$$
f(a + h) - f(a) = f'(a)h + |h|g(h)\,.
$$

One can check that the converse holds as well. Thus, we have an equivalent definition of differentiability that avoids a quotient, and can thus be easily extended to multiple variables; the products become matrix/vector products and $f'(a)$ would become the Jacobian $J_f$.

!!! tip "Key Intuition"
    Sometimes, the term $|h|g(h)$ is notated as $o(|h|)$ (we say $f(x) = o(g(x))$ if $\lim_{x \to \infty}{\frac{f(x)}{g(x)}} = 0$). This captures the idea that the derivative is a "good" linear approximation of the function, since the error term $|h|g(h)$ is "small" in the sense that it goes to $0$ and vanishes faster than the other terms do.

## An equivalent definition

An equivalent definition cuts to the heart of the idea immediately. From one of the StackExchange answers [below](#sources), the key question is how we may define the idea of a "good" linear approximation:

!!! tip "Key Intuition"
    ... in neighborhoods close to the point $a$, the difference between the function and its linear approximations should be incredibly small − in particular, it should be even smaller than the difference $|h|$ between the new and old inputs. Put another way, the approximation should get "infinitely" good, relative to the change in input, as the change in input goes to 0 (in magnitude).

The error between the function and its linear approximation is $|f(a+h) - (f(a) + J_f h)|$, and for this error to become extremely (or even infinitely) small with respect to the error in the input $|h|$, we need

$$
\lim_{h \to 0}\frac{|f(a+h) - (f(a) + J_f h)|}{|h|} = 0\,.
$$

The existence of such a matrix $J_f$ is in fact another equivalent definition for differentiability of $f$ at $a$.

## Sources
Whenever I needed a refresher on the concept of differentiability, I would always go back to these three resources:

1. Serge Lang's [Calculus of Several Variables](https://doi.org/10.1007/978-1-4612-1068-9), particularly chapter III, section 3 on differentiability and the gradient.
2. This excellent Math StackExchange [answer](https://math.stackexchange.com/a/832424).
3. This similarly excellent [answer](https://math.stackexchange.com/a/3298644).

This note is just an attempt to collate these ideas in one place for my own reference. All credit should rightfully go to the above sources.
