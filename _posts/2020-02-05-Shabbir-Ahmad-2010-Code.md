In this post I want to show you a sample code of the model presented in the paper `Expectation and Chance-Constrained Models and Algorithms for Insuring Critical Paths` written by Shabbir Ahmed et al.
Note that the code is just written in abstract mode so you need to do some modifications of your desire and add some sample data to run the model.
So let's dive into the model and code it.

# Cost function model
The cost function model is represented by the name `CP` in page 1796 in the paper.
![image](https://user-images.githubusercontent.com/63361235/140661929-f4781da7-52f4-4bfa-83a5-10156d3cf445.png)
The code would be like this (which is a supersimple code):

```gams
* Create cost function model 

set
i        project graph nodes     /i1*i6/
s        scenarios               /s1*s5/
;
alias(i,j);
set
a(i,j)   /i1.i2, i1.i3,
         i2.i5, i3.i4,
         i4.i6,i5.i6/;
parameter
c(i,j)   cost of arc (i j)
e(s)     probability of realizing a scenario
m1       intercept of sample linear penalty function
m2       slope of sample linear penalty function
psi(s)   critical path length in scenario s
;
binary variable
x
free variable
cp
positive variable
theta(s)
;
equations
obj      objective function
c1       penalty function formula
;
obj..    cp =l= sum(a,c(a)*x(a)) + sum(s,e(s)*theta(s));
c1(s)..     theta(s) =e= m1 + m2*psi(s);

model cost       /obj,c1/;
```

# Critical Path Model
By this code you can calculate critical path length of a project or a directed acyclic graph if you have the option to decrease the length of each activity (arc).
Note that some of the parameters and variables are define in the previous model.
```gams
* Critical path method GAMS model

parameter
du(i,j,s)
gu(i,j,s)
d(i,j)         normal duration
g(i,j)         insured duration
;
free variable
length
positive variable
y(i,j)         if arc (i j) is insured 1 o.w. 0
;
equation
cpm1
cpm2
cpm3
;
cpm1.. length =e= sum(a, (d(a) - (d(a)-g(a))*x(a))*y(a));
cpm2(i)$(ord(i)=1)..        sum(j$a(i,j),y(i,j)) =e= 1;
cpm3(i)$(ord(i)>1 and ord(i)<card(i)).. sum(j$a(j,i), y(j,i)) =e= sum(j$a(i,j),y(i,j));
cpm4(i,j)$(a(i,j))..     y(i,j) =l= 1;

model cpm /cpm1,cpm2,cpm3/;
```
