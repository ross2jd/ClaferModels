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

```P(Y) = P_AND(A,B,P_OR(C,D)) = 0.1*0.1*0.28 = 0.0028```

## Section 2: Clafer Models for Reliability Example ##
This section contains a number of Clafer models that have been used to model the reliability example described in Section 1. Each model has itteratively built on the previous version with changes suggested by fellow researchers in the GSD lab at UW and at ITU. This section is laid out by describing each model indiviudally. The sub section names coorespond to the file names. While these sections give a nice overview of what each model has to offer, each model contains detail comments to explain methodology and constructs used in the particular model.
### probClaferExample - A first approach ###
This model is the very first attempt to model the reliability example. This model laid the ground work for a lot of the refactoring to come in the later models. The model conatins a few main elements:
- Structural Model : This contains the information about what components are connected together. There was one main abstract Clafer that was used to represent a component in the system, namely Component. This "Component" models a single block in our diagram from Section 1. The Clafer for the abstract Component is shown below. The constraints on the inputs and outputs allow us to only specify either the inputs to a compoent or the outputs. For example, if I say that *Component A* has an output to *Component B* it will all *Component A* to the inputs of *Component B*
```
abstract Component
    inputs -> Component *
        [parent in this.outputs]
    outputs -> Component *
        [parent in this.inputs]
```
- Probability : The probability is the most important thing to model correctly when conducting reliability analysis. In this example we take a simple approach to how a probability value is modeled. We simply say that a probability is an integer and we then have a scale factor that we introduce in the model that will act much like a denominator. This concept is something that is refined in the later models.
- Failures : A failure in this model is given by a FailureExpression which is just a probability. We also introduce an abstract Clafer for a basic event whiich is just a name for a proability (these are the lowest level events that cause failures in a fault tree). Each component in the model has a failure expression which repesents (implicity) the probability that a compoent will fail. This failure expressions are referenced from other components directly to show propogation of failures (another thing that will be refined in later models). Lastly the failure expressions are given by explicity formulas by the user which is not a good way for a user to create a failure model.

### probClaferExample2 - 

