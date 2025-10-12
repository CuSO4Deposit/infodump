---
tags:
- Math
- Mathematica
date: 2022-05-05
title: Neat examples of mathematica
categories:
lastMod: 2025-10-12
--- 
## Generate an iterated logistic map

I didn't come out this method, it's in the official documentation of mathematica.

```mathematica
ListPlot[Table[Thread[{r, Nest[r # (1 - #) &, Range[0, 1, 0.01], 1000]}], {r, 0, 4, 0.01}]]
```

![](/images/2022/logistic.png)

output
  
## [Count the number of Wednesdays on Dec 25th in 400 yrs](https://depoze.xyz/2021/10/14/probability-of-christmas-on-wednesday/)

```mathematica
Tally[DayName /@ Tuples[{Range[0, 399], Range[12, 12], {25}}]]
```

Output:

{% raw %}

{{Monday, 56}, {Tuesday, 58}, {Wednesday, 57}, {Thursday, 57}, {Saturday, 56}, {Sunday, 58}, {Friday, 58}}

{% endraw %}
  
## Find the kth Chebyshev Polynomial

```mathematica
Nest[{2 x #[[2]] - #[[1]], 2 x (2 x #[[2]] - #[[1]]) - #[[2]]} &, {1, x}, 3][[2]]// Expand
```

Output:

\-7 x + 56 x^3 - 112 x^5 + 64 x^7

(The 7th Chebyshev Polynomial)
 