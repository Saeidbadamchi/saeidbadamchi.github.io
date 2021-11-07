---
title: "Generating Dynamic Constraints for Iterative Algorithms in GAMS"
categories:
  - blog
  - GAMS
tags:
  - Software
  - Optimization
  - GAMS
  - Benders Decomposition
  - Iterative Algorithms
---

So you want to code a Benders Decomposition algorithm in GAMS, but you have no idea how to iteratively generately new Benders cuts in each iteration and also keep the previous cuts in the model. Here I present a simple code that explains what you are looking for.

```gams
set
iteration        set of all iterations     /s1*s6/
iter(iteration)
;
iter(iteration) = NO;
parameter
b(iteration);
loop(iteration,
         b(iteration) = ord(iteration);
);
variable
dummy
y(iteration);
equations
obj
cpm1
;
obj..            dummy =e= 0;
cpm1(iter)..      y(iter) =e= b(iter);

option mip = cplex ;
model cut        /cpm1,obj/;
parameter
k(iteration);

loop(iteration,
         iter(iteration) = YES;
         solve cut minimizing dummy using lp;
         k(iteration) = y.l(iteration);
);
display k;
```
As you see, the point is in creating a dynamic set which the Benders cut is constructed upon. Instead of using the base set, use the dynamic subset and include the new element of the base set in the dynamic subset in each iteration of the algorithm.

Enjoy coding :-)
