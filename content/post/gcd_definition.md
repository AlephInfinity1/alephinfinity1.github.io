+++
title = '[Maths] Some musings on the definition of gcd and lcm'
date = 2024-07-07T00:30:16+08:00
draft = false
tags = ['Mathematics']
isCJKLanguage = false
+++

Hello!

As you all may know, the greatest common divisor ($\gcd$) and the least common multiple ($\operatorname{lcm}$) are crucial functions in the realm of number theory and general mathematics, with a wide range of applications. They also have sets of beautiful identities to go with:

$$\gcd(a, a) = \operatorname{lcm}(a, a) = |a|$$
$$\gcd(0, a) = \operatorname{lcm}(1, a) = |a|$$
$$|ab| = \gcd(a,b)\cdot\operatorname{lcm}(a,b)$$

However, despite their mathematical elegance, there have always been quirks in their definitions that have always confused me. For example, why is it often defined that $\gcd(0,0)=0$, when $0$ can't appear as a divisor? Also, if $\operatorname{lcm}(0,a)$ is defined to be $0$ for all $a$, then why isn't the lcm of all pairs of integers zero, considering that it is the least natural number? I have thought of a tentative explanation that, although not very useful for applicative purposes, does solve the above inconsistencies in a rather simple and uncluttered way.

## The zero oddity: defining 'divisor' and 'multiple'

Before we begin, we need to have a unified definition of the words 'divisor' and 'multiple', concepts that are literally ingrained in the names of the functions gcd and lcm. The definitions of these terms are pretty unambiguous for positive numbers, but when 0 is involved, things become a little bit less clear: is 0 a multiple of all numbers, including zero? If so, is zero a divisor of zero?

It is a well-known fact, that division by zero is undefined. However, that does not stop zero from being a divisor of zero. We can formalise the definition of multiple as follows:

> $a$ is a multiple of $b$ if there exists an integer $k$ such that $a=kb$.

Correspondingly, we can also define 'divisor' in a similar manner:

> $a$ is a divisor of $b$ if there exists an integer $k$ such that $b=ka$.

Note that in neither definition is the concept of division invoked. Therefore, in terms of multiple and divisor relations, zero is no longer an oddity here. Obviously, there exist integers $k$ such that $0=0k$, thereby making 0 a divisor of 0. The only reason we can't divide zero by zero is that in this case the integer $k$ is not unique, so no quotient can be determined. This clarification in concepts allows us to define $\gcd(0,0)=0$ without introducing any inconsistencies, except for the 'greater' part…

## Less is more: rethinking 'greater'

According to the above definition, every integer is a divisor of 0. So why is $\gcd(0,0)=0$ and not any other integer? Since there is no 'greatest' integer to speak of, doesn't that mean $\gcd(0,0)$ is technically undefined? On a related note, why can't $\operatorname{lcm}(4,6)$ be $0$ or even $-12$ instead of $12$? To solve the above inconsistencies, a solution is to create an alternative definition for 'greater' in these contexts.

> Define the divisor set function $\operatorname{ds}(n)$ over all integers to be the set of all divisors of $n$.
> For instance, $\operatorname{ds}(6)=\set{-6,-3,-2,-1,1,2,3,6}$, $\operatorname{ds}(-1)=\set{-1,1}$, and $\operatorname{ds}(0)=\mathbb Z$.

The following observations can be made:
- $\operatorname{ds}(n)=\operatorname{ds}(-n)$ for all $n$,
- $\operatorname{ds}(1)$ is a subset of all $\operatorname{ds}(n)$, while $\operatorname{ds}(0)$ is a superset of all,
- If $d$ is a divisor of $n$, then $\operatorname{ds}(d)\sube\operatorname{ds}(n)$.

Correspondingly, define the multiple set function $\operatorname{ms}(n)=\set{m\in\mathbb Z|n\in\operatorname{ds}(m)}$.

So, instead of defining greater in the traditional sense, we can instead establish a partial ordering over the set of integers based on the numbers' divisor sets. We declare an integer $a$ to be divisor-less than or equal to another integer $b$, if and only if $\operatorname{ds}(a)\sube\operatorname{ds}(b)$. Denote this relation $a\sqsubseteq b$. Under this ordering relation, $0$ is the maximum element in $\mathbb Z$, while $1$ and $-1$ are minimal elements. As sign no longer matters in ordering, we define $\gcd$ and $\operatorname{lcm}$ to only return nonnegative results only.

Then, we can define $\gcd(a,b)$ to be the greatest nonnegative element in $\operatorname{ds}(a)\cap\operatorname{ds}(b)$ and $\operatorname{lcm}(a,b)$ the least in $\operatorname{ms}(a)\cap\operatorname{ms}(b)$, as ordered by $\sqsubseteq$. Although this ordering does not cover all numbers (e.g. $2$ and $3$ are unordered), it can be shown, that intersections of divisor sets always have a maximum element, and intersections of multiple sets a minimum element.

We can also prove that, for positive integers $a$ and $b$, the result of $\gcd$ and $\operatorname{lcm}$ using the above ordering and the regular ordering will always be the same. Since $d\sqsubseteq n$ whenever $d$ is a divisor of $n$, as long as $a$ and $b$ are a multiple-divisor pair, then $a\sqsubseteq b\iff a\le b$ and vice versa.

Now, let's see how we can use the above definition, to explain seeming discrepancies at corner cases:
- $\gcd(0,0)=0$ because $\operatorname{ds}(0)\cap\operatorname{ds}(0)=\mathbb Z$, and as detailed above, under the ordering $\sqsubseteq$, $0$ is the greatest element in $\mathbb Z$.
- Similarly, $\gcd(0,a)=|a|$ because $\operatorname{ds}(0)\cap\operatorname{ds}(a)=\operatorname{ds}(a)$, and $a$ is the greatest element of that set. The absolute value is required since we want $\gcd$ to return nonnegative values only.
- $0$ is the only multiple of $0$. Therefore, for all integers $a$, $\operatorname{ms}(0)\cap\operatorname{ms}(a)=\set{0}$. In this case, the only reasonable value for $\operatorname{lcm}(0,a)$ would be $0$.
- $\operatorname{lcm}(4,6)=12$ and not $0$, not because of any special definitions, but because in the context of $\operatorname{lcm}$, $0$ would be 'greater' than $12$!

## Conclusion

While the above results probably won't matter much during actual computing, I still find them pretty satisfying, as they provide a nice way to interpret the special-case values of the functions $\gcd$ and $\operatorname{lcm}$. Additionally, perhaps this might poke some insight as to extend the above functions to other domains, such as the rational numbers or the Gaussian integers?