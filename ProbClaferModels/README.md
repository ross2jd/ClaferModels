Probabalistic Clafer Model
==========================

This folder contains Clafer models related to the exploration of modeling probability in Clafer. This README is structured as follows: 
* Section 1 gives an overview of the example that I am using in many of the models
* Section 2 gives an overview of each of the models that implements the example given in Section 1.


## Section 1: Reliability Example ##
This is just an example for prototypying a Clafer model for reliability analysis. We
consider the following example taken from Figure 4.14 in "Reliability Engineering and Risk
Analysis" by Mohammad Modarres, Mark Kaminskiy, Vasiliy Krivtsov

*Structural model::*
```
                   ---
           |----->| A |------->|
  ---      |       ---         |        ---
 | X | ----|       ---         |------>| Y |
  ---      |----->| B |------->|        ---
           |       ---         |
           |    ---     ---    |
           |-->| C |-->| D |-->|
                ---     ---
```

Assume that blocks A, B, C, and D all have a basic event "InternalFailure" but we denote it
as the block letter in the fault tree (i.e. "InternalFailure of A" will just be "A"). In this
example we are interested in seeing if we send data from X (not including in the reliablilty
analysis) what is the probability that the data will be read at Y (also not included in the
analysis). Thus, we are really just interested in the probability that the subsystem made up
of components A, B, C, and D fails.

Let us denote the top event as Y which is the event that our subsystem (A, B, C, D) fails.

The fault tree then would look as follows:

*Fault Tree::*
```
                  ---
                 | Y |
                  ---
                   ^
                   |
                 (AND)
                   ^
       |------|----|----------|
      ---    ---             (OR)
     | A |  | B |             |
      ---    ---        |------------|
                       ---          ---
                      | C |        | D |
                       ---          ---
```

Now, let us assume that the basic events of A, B, C, D have the following probabilities:

| Component     | Probability   |
| :-----------: |:-------------:|
| A             | 0.1           |
| B             | 0.1           |
| C             | 0.1           |
| D             | 0.2           |

We can find the probability of C or D failing using the formula for the OR Gate.
Note that the probability at the OR gate is given by the following formula if x,y,z are
independent but not mutually exclusive events (Eq 2.15):

```P_OR(x,y,z) = P(x) + P(y) + P(z) - P(x,y,z)```

Thus,

```P_OR(C,D) = P(C)+P(D)-P(C)*P(D) = 0.28```

We can find the probability of T then by using the previous result in our AND Gate.
Note that the probability at the AND gate is given by the following formula if x,y,z are
independent events (Eq 2.13):

```P_AND(x,y,z) = P(x)*P(y)*P(z)```

Thus,

```P(Y) = P_AND(A,B,P_OR(C,D)) = 0.1*0.1*0.28 = 0.0028``

