Problem statement 
-----------------

For a binary categorical (say Y = 1 or 0) dependent variable based on observing some data (say x), posterior probability of category

```
P(Y=1 | X=x) and P(Y=0 | X=x) are used to determine the category.

P(Y=1|X=x) = P(X=x|Y=1) * P(Y=1) / P(X=x) where P(X=x) = P(X=x|Y=1) * P(Y=1) + P(X=x|Y=0) * P(Y=0)
P(Y=0|X=x) = P(X=x|Y=0) * P(Y=0) / P(X=x) where P(X=x) = P(X=x|Y=1) * P(Y=1) + P(X=x|Y=0) * P(Y=0)
```

In general we want to write a program that takes 'x' in input and gives above probabilities in the output.


Warmup
------

Consider the problem from the opening para of 1st reference (Allen Downey).

Here ~ represents not.

```
P(H)     = 0.9
P(F|H)   = 0.1
P(M|H)   = 0.9
P(F|~H)  = 0.5
P(M|~H)  = 0.5
```

So, JPT:

```
      M          F

H    0.81       0.09    ---> P(H) = 0.9
~H   0.05       0.05    ---> P(~H) = 0.1
      |          |
      |          |
      v          v
    P(M)=0.86  P(F)=0.14
```

Some basic calculations (discussed in the 1st section of the same reference) :

```
P(H|M) = P(H, M) / P(M) = 0.81/0.86  = 0.942
P(H|F) = P(F, H) / P(F) = 0.09/0.14 = 0.643

```
More calculations in side bar.


sidebar: demonstrating Bayes rule in on-line mode to update posteriors
---------------------------------------------------------------------

Posteriror (numerator portion) from previous step is used as prior (numerator portion) for the next data item!

```
P(H|F) = P(F|H) * P(H) / P(F)

    = 0.1  * 0.9 / ( 0.1 * 0.9 + 0.5 * 0.1 )
    = 0.64
    or numerator  = 0.09

P(H|FF) = P(FF|H) * P(H) / P(FF) = P(F|H) * P(F|H)  * P(H) / [ P(FF|H)*P(H) + P(FF|~H) * P(~H) ]
        = (0.1 * 0.1 * 0.9) / (0.1*0.1*0.9 + 0.5*0.5*0.1)  = 0.265
        or numerator = 0.1 * 0.09 = 0.009

P(~H|FF) = P(FF|~H) * P(~H) / ....
         or numerator = 0.5 * 0.5 * 0.1 = 0.025

or P(H|FF) = 0.009 / (0.009+.025)   = 0.2647

like wise

P(H|FFF) = ....
         or numerator = P(F|H) * 0.009 = 0.1 * 0.009 = 0.0009

P(~H|FFF) = ...
         or numerator = P(F|~H) * 0.025 = 0.5 * 0.025 = 0.0125

or 

P(H|FFF) = 0.0009 / (0.0009 + 0.0125) = 0.067 = 0.07
```

And, likelihoods:

```
Likelihood of first student being F given H = P(F|H) = 0.1 or 10%
Likelihood of first student being F given ~H = P(F|~H) = 0.5 or 50%
Likelihood of first 3 student being F given ~H = P(FFF|~H) = 0.5 * 0.5 * 0.5 =  0.125 or 12.5% [assuming conditional independency; not true in reality, won't girls come in groups ?]
```


Odds
----

```
Odds of hypothesis:
O(H) = P(H) / (1-P(H)) = 0.9/0.1 = 9


Odds of H given F (meaning in the F group odds of H vs. ~H) 
  = O(H|F) 
      = P(H|F) / 1-P(H|F)
      = P(H|F) / P(~H|F) 

      =  [ P(F|H) * P(H)/P(F)  ]   / [ P(F|~H) * P(~H) / P(F) ]
      =  [ P(H) / P(~H) ] * [ P(F|H) / P(F|~H) ]

Defining likelihood ratio LR(F|H) = P(F|H) / P(F|~H), we get:

O(H|F) = O(H) * LR(F|H)
and 
O(H|M) = O(H) * LR(M|H)

For above problem:
O(H|M) = 9 * P(M|H) / P(M|~H) = 9 * 0.9 / 0.5 = 16.2
O(H|F) = 9 * 0.1 / 0.5 = 1.8


OR(H|M) = Odds ratio : odds of the H if we observe male student relative to the odds if we observe a female student
  = OR(H|M)
  = O(H|M) / O(H|F)
  = 16.2/1.8 = 9


Another way (due to symmetry will be the same):
OR(M|H) = Odds of seeing male student assuming hypothesis, H is correct relative to odds of seeing male student assuming H is wrong
OR(M|H) = O(M) * LR(H|M) = [ 0.86 / (1-0.86) ] * P(H|M) / P(H|F) 
    =  ( 0.86 / (1-0.86) ) * 0.942 / 0.643 = 9 (checks!)
```

Exercise: prove symmetry of odds ratio
------

Another useful fact:

```
Odds ratio = likelihood ratio !!!!

OR(H|M) = O(H|M) / O(H|F) =  [ O(H) * LR(M|H) ] * [ O(H) * LR(F|H) ]
                  =  LR(M|H) / LR(F|H)

```

Posteriors
---------

```
P(H|F) = [ P(F|H) * P(H) ] / [ P(F|H) * P(H) + P(F|~H) * P(~H) ]
       = 1 / [ 1 + ( P(F|~H) * P(~H) / P(F|H) * P(H) ) ]
       = 1 / (1+ exp(-a))
       = p1 (say)
where
a = ln {  P(F|H) * P(H) / P(F|~H) * P(~H) }

Notice:

   P(H) / P(~H) = O(H)
   P(F|H)  / P(F|~H) = LR(F|H)           

So,

a = ln { O(H) * LR(F|H) } = ln { O(H|F) } = LO(H|F)


Likewise,

P(H|M) = [ P(M|H) * P(H) ] / [ P(M|H) * P(H) + P(M|~H) * P(~H) ]
       = 1 / [ 1 + ( P(M|~H) * P(~H) / P(M|H) * P(H) ) ]
       = 1 / (1+ exp(-b))
       = p2 (say)

where
b = ln {  P(M|H) * P(H) / P(M|~H) * P(~H) }

Notice:

   P(H) / P(~H) = O(H)
   P(M|H)  / P(M|~H) = LR(M|H)           

So,

b = ln { O(H) * LR(M|H) } = ln { O(H|M) } = LO(H|M)

Also, notice that :

p1 / (1-p1) = exp(a)


Say X=1 represents M and X=0 represents F.
Now, how do we convert the two formulas P(H|F) and P(H|M) into one equation based on input x?

P(H|X=x) = <a function of x>. This is the essence of original problem.
Here X = 0 or 1 only.

Consider:

LO(H|X=x) = LO(H|F) + x [ LO(H|M) - LO(H|F) ]  (rhs is LO(H|F) for x = 0 and LO(H|M) for x - 1)
          = LO(H|F) + x ln [ O(H|M) / O(H|F) ]

From the fact above:
   OR(H|M) = O(H|M) / O(H|F) =  [ O(H) * LR(M|H) ] * [ O(H) * LR(F|H) ]
                  =  LR(M|H) / LR(F|H)

So,

LO(H|X=x) = LO(H|F) + x ln [ LR(M|H) / LR(F|H) ]
          = LO(H|F) + x [ LLR(M|H) - LLR(F|H) ]
          = LO(H|F) + x LOR(H|M)
          = b0 + x * b1 (say)


So,

P(H|X=x) = 1 / [ 1 + exp{-b0 - x * b1} ] = p (say)

Hence,

logit(p) = ln (p/1-p) = b0 + x * b1 = LO(H|X=x)

```
So, 

```
p = posterior probability of H given data, X.
b0 = LO(H|F) = log odds of hypothesis, H.
b1 = LOR(H|M) = log odds ratio of hypothesis, that represent odds of hypothesis when data, X = 1 relative to when X = 0.
logit(p) = LO(H|X=x) = posterior log odds of H, hypothesis, given observed data, X.

```

Generalize
----------

```
P(Y=1|X=x) = 1 / [1 + ( P(X=x|Y=0) * P(Y=0) / P(X=x|Y=1) * P(Y=1) ) ]
           = 1 / (1+ exp(-a))
           = p (say)
where
a = ln {  P(X=x|Y=1) * P(Y=1) / P(X=x|Y=0) * P(Y=0) }

Notice:

   P(Y=1) / P(Y=0) = O(Y=1)
   P(X=x|Y=1)  / P(X=x|Y=0) = LR(X=x|Y=1) 

So,

a = ln { O(Y=1) * LR(X=x|Y=1) } = LO(Y=1) + LLR(X=x|Y=1) = LO(Y=1|X=x)

ln(p/1-p) = logit(p) = a = LO(Y=1|X=x)

```
Exercise:  Solve the problem posed in reference 3 (stackexchange)

------------------

CPT (grouping by H) :

```
            M                 F
H    P(M, H) / P(H)       P(F, H) / P(H)
~H   P(M, ~H) / P(~H)     P(F, ~H) / P(~H)
```

or
```
        M              F
H    P(M|H)        P(F|H)
~H   P(M|~H)       P(F|~H) 

```


References
----------

http://allendowney.blogspot.com/2014/04/bayess-theorem-and-logistic-regression.html
https://www2.isye.gatech.edu/~brani/isyebayes/bank/handout1.pdf
https://math.stackexchange.com/questions/514558/show-posterior-probability-takes-the-form-of-the-logistic-function
https://en.wikipedia.org/wiki/Odds_ratio
http://faculty.cas.usf.edu/mbrannick/regression/Logistic.html
https://www.unm.edu/~schrader/biostat/bio2/Spr06/lec11.pdf
https://pdfs.semanticscholar.org/presentation/ed97/041e2e770e16113ebf3d36210b8b1ee6e802.pdf
http://calcscience.uwe.ac.uk/w2/am/Ch07/7_6_Bayesian.pdf
