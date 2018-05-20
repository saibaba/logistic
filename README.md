http://allendowney.blogspot.com/2014/04/bayess-theorem-and-logistic-regression.html
https://www2.isye.gatech.edu/~brani/isyebayes/bank/handout1.pdf
https://math.stackexchange.com/questions/514558/show-posterior-probability-takes-the-form-of-the-logistic-function
https://en.wikipedia.org/wiki/Odds_ratio
http://faculty.cas.usf.edu/mbrannick/regression/Logistic.html


p(h|f) = p(f|h) * p(h) / p(f)

    = 0.1  * 0.9 / ( 0.1 * 0.9 + 0.5 * 0.1 )
    = 0.64
    or numerator  = 0.09

p(h|ff) = p(ff|h) * p(h) / p(ff) = p(f|h) * p(f|h)  * p(h) / [ p(ff|h)*p(h) + p(ff|!h) * p(!h) ]
        = (0.1 * 0.1 * 0.9) / (0.1*0.1*0.9 + 0.5*0.5*0.1)  = 0.265
        or numerator = 0.1 * 0.09 = 0.009

p(!h|ff) = p(ff|!h) * p(!h) / ....
         or numerator = 0.5 * 0.5 * 0.1 = 0.025

or p(h|ff) = 0.009 / (0.009+.025)   = 0.2647

like wise

p(h|fff) = ....
         or numerator = p(f|h) * 0.009 = 0.1 * 0.009 = 0.0009

p(!h|fff) = ...
         or numerator = p(f|!h) * 0.025 = 0.5 * 0.025 = 0.0125

or 

p(h|fff) = 0.0009 / (0.0009 + 0.0125) = 0.067 = 0.07




odds of hypothesis:

O(H) = P(H) / (1-P(H))    = nh/ n-nh


Likelihood ratio LR

LR(F|H) = P(F|H) / P(F|!H)

O(H|F) = P(H|F)/(1-P(H|F)) = P(H|F)/P(!H|F) = P(F|H)*P(H)/(P(F)(P(!H|F)) = P(F|H)*P(H)/(P(F|!H)*P(!H)) = P(F|H)*P(H)/(P(F|!H)*(1-P(H)) = O(H) * P(F|H) / P(F|!H) = O(H) LR(F|H)

Taking logs

LO(H|F) = LO(H) + LLR(F|H)   -- if first student is female
LO(H|M) = LO(H) + LLR(M|H)   -- if first student is male
LO(H|X) = LO(H) + LLR(X|H)   -- if first student sex is X

IF X = 0 for female and = 1 for male

LLR(X|H) = LLR(F|H)   -- if X = 0
LLR(X|H) = LLR(M|H)   -- if X = 1
or one equation

LLR(X|H) = LLR(F|H) + X ( LLR(M|H)  - LLR(F|H) )



LO(H|X) = LO(H) + LLR(F|H) + X ( LLR(M|H)  - LLR(F|H) )
