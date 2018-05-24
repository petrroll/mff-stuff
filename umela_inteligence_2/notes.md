#1
- uncertain information in pure logical approach
    - too big domain space, not everything has complete theory, ..
    -> probabilistic methods

Probability theory:
- knowledge base is represented by full joint distribution
- posterior => marginal probability given observed evidence
- hard to infer by enumeration O(d^n) 
    - d: number of values in domains of each variable
    - n: number of hidden and query variables
- can be hard to obtain full joint ditribution

- Full independence: P(X|Y) = P(X) or P(X, Y) = P(X)*P(Y)
    - enables reduction of full joint distrib. table
- Conditional independence: P(X|Y,Z) = P(X|Y)
    - also enables redutcion of full jb. table

- Bayes rule: P(a|b) = P(b|a)*P(a) / P(b)
    - P(cause | effect) : diagnostic direction
    - P(effect | cause) : causal direction
    - storing causal instead of diagnostic makes it less fragile to sudden epidemics ..
    
Naive Bayes: 
- assumes conditional independence between effects given cause variable
- P(C, E1, E2, ...) = P(C) * Times_i(P(Ei|C))
- used even if effect variables not actually cond. ind. for simplification

#2
Bayesian networks: efficient way to represent full joint probability
- specifies conditional independence relationships
- dag, node X has conditional pribability distrib. P(X|Parents(X))
- CPT (conditional prob. tables) describe cond. probabilities

- Creation: P(Xi | Parents(Xi)) = P(Xi| Xi-1...X1) & discard cond. indep.
    - Order matters, always use the smallest set of parents possible
    - Always consistent, no reduntant information
    - If dependance is at most on d vars, n*2^d instead of 2^n
    - Best to follow causal directions 
- Cond. ind. properties
    - Node is cond. independent of its non-descendats given its parents
    - Node is cond. independent of all other nodes given its parents, children, and children's parents
        - https://stackoverflow.com/questions/45851385/why-does-markov-blanket-contain-the-childrens-parents
- Inference by enum: P(X|e) = alfa*P(X,e) = alfa*SUM_y(P(X,e,y))
    - Remembering already computed subexpressions
    - Infer in the order that keeps the factors smallest
    - NP (3 SAT convertible to bayesian network) / #P hard

- Monte carlo inference
    - sample corresponds to instantiation of random variables
    - generated from known prob. distrib.
    - variables in topological order, conditioned by values already assigned in parents
    P(x1, ... xn) = lim_n->inf (N(x1, ..., xn) / N)

- Rejection sampling
    - from generated samples select only those consistent with evidence
    -> problem: rejecting too many samples
    -> generate only consistent evidence, remember weigth of sample (likelihood weighting)

    - P(X|e) = a*N(X,e) * w(X,e) = a*N(X, e) 
        - w(z, e) = TT_j(ej|parents(ej))
        - P(z, e) = TT_i(zi|parents(zi))

Markov Chain Monte Carlo
- start with randomly generated sample consistent with e
- for random variable X (outside evidence) select new value conditioned on Markov blanket for X
    - for each Zi € non-evidence variable sample new value given its Markov Blanket (MB(Zi)), repeat enough 
    - P(x|mb(X)) = P(x|parents(X)) * TTz€Children(X)(P(z|parents(Z)))
    - the sampling follows stationary distribution of the variables' values
    - Gibss sampling





