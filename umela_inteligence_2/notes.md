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
- DAG, node X has conditional pribability distrib. P(X|Parents(X))
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
- Inference by enum: P(X|e) = alfa * P(X,e) = alfa * SUM_y(P(X,e,y))
    - Infer in the order that keeps the factors smallest
    - NP (3 SAT convertible to bayesian network) / #P hard

    - summing over hidden variables (variable elimination)
    - recursive enumeration of not-yet computed variables
        - pointwise multiplication on the current distrib. factor and enumration of the remaining vars
        - sumation of variable (elimination) of above in case of hidden variable
    - remembering already computed subexpressions
    - sum over variables that will make the next factor smallest (heuristic)


- Monte carlo inference
    - sample corresponds to instantiation of random variables
    - generated from known prob. distrib.
    - variables in topological order, conditioned by values already assigned in parents
    P(x1, ... xn) = lim_n->inf (N(x1, ..., xn) / N)

- Rejection sampling
    - from generated samples select only those consistent with evidence
    -> problem: rejecting too many samples
    -> generate only consistent evidence, remember weigth of sample (likelihood weighting)

    - P(X|e) = a * P(X,e) * w(X,e) 
        - w(z, e) = TT_j(ej|parents(ej)) ~ weight given by evidence
        - P(z, e) = TT_i(zi|parents(zi)) ~ probability of obtaining sample

Markov Chain Monte Carlo
- start with randomly generated sample consistent with e
- for random variable X (outside evidence) select new value conditioned on Markov blanket for X
    - for each Zi € non-evidence variable sample new value given its Markov Blanket (MB(Zi)), repeat enough 
    - P(x|mb(X)) = P(x|parents(X)) * TTz€Children(X)(P(z|parents(Z)))
    - the sampling follows stationary distribution of the variables' values
    - Gibss sampling

# 3
- situation calculus: view the world as series of snapshots
    - hidden and observable variables
    - assume discrete time with uniform steps
- transition model P(X_t|X_0:t-1)
    - markov assumption: current state depends only on finite history
    -> Markov chains 
    - first order Markov chain: depends only on the previous state
    - change in the world are caused by the static markov process
- observation model: how evidence vars depend on hidden variables
    - markov assumption: only on hidden state vars from the same time
- could be viewed as unrolled bayessian network

Inference tasks
- filtering: posterior distribution over the most recent state given evidence to date
- prediction: posterior distribution over future state given evidence to date
- smoothing: posterior distro over past state given evidence to the present
- most likely expl.: find sequence of sates most likely for seq. of observations

- filtering: 
    - Update function: P(Xt+1 | e1:t+1) = f(et+1, P(Xt|e1:t))
    - P(Xt+1 | e1:t+1) = alf * P(et+1| Xt+1 e1:t) * P(Xt+1 | e1:t)
                        = alf * P(et+1 | Xt+1) * sum_xt( P(Xt+1 | xt) * P(xt|e1:t) )
    - message f1:t (current distrib. given evidence sofar) is propagated forward over the sequence

- prediction
    - the same as filtering without adding new evidence
    - after some time the distribution converges to stationary distrib.

- smoothing
    - P(Xk | e1:t) = alfa* P(Xk | e1:k) * P(ek+1:t | Xk)
                    = alfa * f1:k x bk+1:t
    - P(ek+1:t | Xk) = SUM_xk+1 ( P(ek+2:t | xk+1) * P(ek+1|xk+1) * P(xk+1 | xk) )
    - backtracking is basically filtering from the end back

- most-likely sequence: viterbi algorithm
    - states depend only on previous state -> there exists a recursive formula
    - for each state at every time-slice compute its most likely predecessors 
        - For each timeslice we know the probability that some path will end is any if its states
        - In the last timeslice select the highest probability -> backtrack trough predecessors -> find most likely Path
    - distribution
        - m1:t+1: P(et+1 | Xt+1) * max_xt( P(Xt+1 | xt) * m:t ) : vybereme stav, pro který je nejpravděpodobnější že do něj přejdeme z o jedno kratší distribuce
        - m1:t = max_x1..xt-1 P(x1...xt-1, Xt | e1:t) : select the highest probability path

Hidden markov models
- state described by single discrete variable Xt, evidence variable Et
- Transition Tij: P(Xt = j | Xt-1 = i) and sensor model Oij: P(Et = et | Xt = i)
- filtering: f1:t+1 = alpha * Ot+1 * T^t * f1:t
- smoothing: bk+1:t = T * Ok+1 * bk+2:t

- full smoothing: O(t) with dynamic programming, O(|f|t) memory
    - we can first run forward pass and then backward with constant memory
    - computing f1:k from f1:k+1 using inverse matrixes for T and O

- fixed lag smoothing: smoothing for a fixed number of d steps back in time P(Xt-d | e1:t)
    - f1:t-d+1 = alpha Ot-d+2 * T^t * f1:t-d
    - bt-d+2:t+1 = Ot-d+1^-1 * T^-1 * bt-d+1:t * T * Ot+1

# 4
Dynamic bayesian network
- bayesian network that represenets a temporal probab. model
- variables and links replicated from slice to slice

- hidden markov model is special case of a dynamic Bayesian network
- dynamic b. n. can be encoded as hidden Markov model (variable that is tupple of values of vars in DBN)

- DBN can be more compact, doesn't have to contain all transitions for independent variables
- in inference only last slices are neccessary to keep

- aprox inference: generating samples, weighting them with evidence
    - problem: possibly very small weight of each sample
- particle filtering: keeps possible samples
    - starts with prior distribution
    - next state is generated through transition model probability
    - weighted by likelyhood it assigns to evidence
    - resample population given their weights (sampling with replacement)
    - repeat for next state

Senzor failures:
- gaussian error model for noise
- states for transient and persistent failures
    - transient: regardless of state certain probability for completely incorrect value
    - persistent: additional state indicating the senzor will stay broken
        - if senzor broken: assume normal discharge rate

Continuous variables
- discratization into fixed set of intervals
- use standard density functions with finite number of parameters
    - normal distribution
- hybrid networks
    - dependence of cont on cont: mean scales linearly with parent, std fixed
    - dependence of cont on disc: parameters for each value
    - dependence of disc on cont: soft treshold function

Kalman filters: 
- message passing techinque works if (senzor, transition) models are linear Guassian
- non-liearity: multiple filters run in parallel, prediction is weighted sum of indiv. results

Multiple objects: 
- exact reasoning would require summing over n! mappings
- approximate methods used instead
    - maximizing joint probability of current obs. and predicted positions 
        - commits to single best at each step -> problems
    - particle filtering
    - Markov Chain Monte Carlo

# 5
- utility theory: combining preferences with probabilities
- expected utility: utility weighted by probability of corresponding states
    - agent chooses action that maximizes expected utility

- axioms of utility theory
    - orderability
    - transitivity
    - continuity: A > B > C -> Ep: [p,A; 1-p,C]~B
    - substitutability: A~B -> [p,A; 1-p,C] ~ [p,B; 1-p,C]
    - monotonicity: A>B -> more A in comb. A,B > less A
    - decoposability: 

- utility function exists for each rational agent but isn't unique
- normalization: best is 1, worst is 0
- to assess utility for particular prize S: ask agent to choose between
    - S
    - standard lottery: [p, Smax; 1-p, Smin]
    -> p adjusted until the agent is indifferent; U(S) = p

- utility of money etc. doesn't have to be linear 

- certainty effect: people are attracted to gains that are certain
- ellsberg paradox: people prefer static / known uncertainty 
- framing effect, anchoring effect (smth can be expensive but it still makes other things look cheaper)

- multi-goal optimization
    - strict dominance: all possible outcomes of B strictly dominate A
    - stochastic dominance: expected outcomes for all goals are better 
    
    - alternative: combine all goals into one utility value

- preference structure: for complete utility function for n attributes: d^n values
    - preferentiallity independence (preference of A1 and A2 outcomes independent on A3)
    - each pair ind. on all other attrs -> mutual preferential independence
        -> we can just sum individual utilities to get one "additive value function"
        -> Two attributes, 1 and 2, are called additive independent, if the preference between two lotteries (defined as joint probability distributions on the two attributes) depends only on their marginal probability distributions (the marginal PD on attribute 1 and the marginal PD on attribute 2).
    - one utility func. that represets preferences on lotteries of bundles
        - mutual utility independence -> multiplicative utility func (U = k1U1 + k2U2 + k1k2U1U2)
        -> Attribute 1 is utility-independent of attribute 2, if the conditional preferences on lotteries on attribute 1 given a constant value of attribute 2, do not depend on that constant value.

- information value theory
    - ValueOfPerfectInfo(Ej) = SUM_k( P(Ej=ejk |E) * EU(alp_jk | e, Ej = ejk) ) - EU(alp|e)
        - alp_jk: best action given the evidence ejk
        - average over all values for the information we might discover
    - inf. has value only if it changes the plan and the new plan would be substantially better
    - VPI >= 0, not additive, order independent
    - ask until VPI(Ej) > cost(Ej)

# 6
- decision network: chance nodes, decision nodes, unitily nodes
- normal minimax inference (with the difference that there's no mini-ing)

- creating decision networks
    - create causal model, simplify it to a quantitative decision model
    - assign probabilities 
    - assign utilities 
    - verify and refine (using gold data)
    - perfrom sensitivity analysis

MDP: Markov Decision process
- Sequential decision process for:
    - fully obs., stochastic evnironment 
    - Markovian transition model 
    - additive rewards
- Solution for MDP is a policy: pi(s) -> distribution of actions
    - Simple reflex agent
    - optimal policy yields highest expected utility

- finite horizon: fixed time N after which nothing matters -> optimal policy changes
- infinite horizon: optimal policy is stationary

- two coherent ways to assign utilities to sequences
    - additive rewards U(s0, s1, s2, ...) = R(s0) + R(s1) + R(s2) + ...
    - discounted rewards U(s0, s1, s2, ...) = R(s0) + yR(s1) + y^2R(s2) + ..

    - discounted rewards ensure that utility of infinite sequence is finite

- proper policy: is guaranteed to reach a terminal state
- infinite policies can be compared via average reward per time step

- expected Utility of a policy is extected value of sum of discounted rewards for states obtained by following such policy
- if two policies reach the same state they (both being optimal) should agree from that point 

- True utility of state: final utility of state for optimal policy
- Choose the action that maximizes expected utility of subsequent state

- Bellman equation: U(s) = R(s) + y max_a€A(s)( SUM_s'(s'|s, a)U(s') )
    - Utility of state is current reward + discounted utility of next state assuming best action is chosen

- Iterative approach:
    - start with arbitrary initial values for U(s)
    - Update utility U(s) from utilities of its neighbors (Bellman update)
        - converges to one solution of Bellman equations
        - the function has only one fixed point, gets closer to it with every update

- Convergence: 
    - |BUi - BU'i| <= y|Ui - U'i| // The probabilities in exp. val for updates and Rewards are the same for U and U'
    - |BUi - U| <= y|Ui - U| // U: vector of true utilities -> fixed point

- Policy loss: how much can agent loose by executing policy p instead of optimal
    - if |Ui - U| < e -> |Upi - U| < 2ey/(1-y)

- policy iteration alghoritm
    - evaluation: given policy pi, calculate U_pi
        - Upi(s) = R(s) + y SUM_s'( P(s'|s, pi(s))Upi(s') )
        - O(n*n^2)
    - policy improvement: pi_i+1(s) = argmax_a€A(s) (SUM_s'( P(s'|s,a)Upi(s') ) )
    - iterate as long as we're changing the policy
    - finite number of policies -> always terminates

- if the world is only partially observable -> partially observable	Markov decision proces
    - transition model P(s'|s, a), reward function R(s), sensor model P(e|s)
    - current state is potentially uknown -> hard to reccoment action via policy
    -> belief state, probability over all possible states
    -> optimal policy for belief states 

    - forwarding belief state works as expected

    - given belief state execute action a = pi(b)
    - receive percept e
    - set current belief state b' = FORWARD(b, a, e)

    - if observation is not available sum over all possible obs. given their probability
    - reward is expected reward given current belief state

    - POMDP on physical state space corresponds to MDP on belief-state space
    - MDP techniques can't be used on POMDP due to it having infinite number of states (the belief state space if continuous)

    - conditional plans: action selected on observations and previous actions (not belief-state)
    - value iteration can be modified to work -> not efficient
    
    -> Dynamic bayesian networks with look-ahead
    - represents transition and sensor models
    - added nodes for actions and rewards 

    - lookahead via expected minimax alg. through Tree with belief states>-actions->evidence
        - depth of search determined by discount factor
        

# 7
- single move games: players, actions, payoff function
- pure strategies (deterministic policy), mixed strategies (randomized, distribution for actions)

- s strong dominates s', if outcome of s is better than s' for every opponent's action
- weak dominates if it's better for at least one opponent's action and not worse for others

- if both have dominant strategy: dominant strategy equilibrium
- Nash equilibrium: no player can benefit by switching strategies 
- Pareto optimal: there is no other outcome all other players would prefer
    - pareto dominated if all players would prefer the outcome
    - Prisoner's dilema dominant equilibrium (testify, testify) is pareto dominated by (refuse, refuse)

- there can be situations without dominant strategies and with nash equilibria. 
- choose the pareto optimal nash equilibrium if possible

- there's a method for finding opt. mixed strategy for two-player zero sum games
    - maximin: lower and higher bound given by minimax after assuming the turns are sequential

- repeated games: repeated prisoners dilema
    - important to have non-static number of rounds
    - really good strategy: tit-for-tat

Mechanism design - inverse game theory:
- design game whose solutions, consisting of each agent pursuing rational strategy, maximize some global utility function

Auctions: 
- mechanism for selling goods to members of a pool of bidders
- each bidder i has utility value vi for item v
    - private value
    - unknown / estimated common value

- each bidder makes a bid, highest bid wins for some price

- efficient: the bidder that values it the most gets it
- truth-relealing auction: when bidders reveal their vi: how much they value smth

- English auction
    - Starts on some minimum, reapated bids of current + some minimal increment
    - Highest bidder pays his bid
    - Ends when nobody wants to bid anymore

    - efficient and revenue maximizing if: 
        - sufficient number of bidders 
        - no collusion (cooperation to manipulate prices)
    - Properties:
        - Has dominant strategy: keep bidding as long as cost < vi
        - It's not truth revealing: only reveals the lower bound

        - If there's strong bidder -> no-one will enter -> sold for initial cost 
        - High communication cost

- Sealed bid auctions
    - Each makes one secret bid and communicates it
    - Highest bid wins

    - First price variant:
        - No simple dominant strategy: best b0 + e if < vi // b0 is expected max of other bids
    - Second price variant / Vickrey:
        - Highest bid wins, pays second highest bid
        - Dominant strategy: bid vi

Tragedy of commons
- the optimal strategy for everybody is to behave badly -> makes it worse for everyone
    -> the cummulative effect makes it way worse than if everyone behaved nicely

- soluation: all externalities not recognized in indiv. transactions need to be made explicit
    - Charging for common resources: taxes

- Vickrey-Clarks-Groves mechanism
    - Center asks each agent to report its value: bi
    - Center distributes goods to subset of agents A maximize reported values Sum:a(ba)
    - Each agent pays a tax equal to wi' - bi' -> what first looser would've payed
        - wi' = total global utility if i wasn't in the game
        - bi' = total global utility - bi

    - all winners pay less then their value
    - all losers are happy because they value goods less than the required tax
    - truth revealing -> it's optimal to report your true value

# 8
Learning: 
- which component is to be improved
- what's prior knowledge and its representation
- what type of feedback
    - supervised learning: agent observes example input:output pairs
    - unsupervised learning: no explicit feedback is given
    - reinforcement learning: rewards & punishments 

Supervised learning
- function h - hypothesis - is selected
- consistent hypothesis: h(xi) = yi: accuracy of hypothesis func is measured
- regression, classification

- Ockham's razor: prefer simple hypothesis functions -> prevents overfitting

- Decision trees:
    - Sequence of tests on individual input variables
    - Select most important attribute, divide examples, if sets not pure continue recursively 
    - Most important attribute: makes the most difference to the classification of examples 
        - Highest information gain in terms of entropy
        - Gain = Entropy(X) - E[Entropy(Xk)]

    - Pruning: delete unnecessary nodes that have only leaf children
        - unnecessary: doesn't pass chi^2 test for correlation with goal variable
        - don't have to prune if we stop early -> early stopping
                -> prevents finding a good combination of attributes where no single one was good (e.g. xor)
        
    - Problems:
        - Missing data
        - Multivalued attributes: split on just one value
        - Continuos attributes: find split point (in between observed values)
        - When to start doing regression nodes?
    - Good: it's possible to understand the reason for their output, verifiable

- Regression:
    - linear regression has unique solution, usually used L2 loss
    - non-linear regression: gradient descent for loss optimization
    - multivariate linear regression: orth. projection to samples space

- Linear classifiers:
    - treshold function: logistic treshold function (1/1+e^-z)
    - hw(x) = Treshold(w.x), hw is compared with 0 / other treshold to classify
    - updates: wi = wi + alpha * (y-hw(x)) . hw(x) * (1-hw(x)) . xi

- parametric model: summarizes data with set of fixed size parameters
- Non-parametric models: don't have bounded number of "parameters"
    - table lookup, ...
    - (k-)nearest neighbor(s), 
        - neighbors voting based on distance 
        - Minkowski distance, Hamming (for booloean attribs.)
        - Use data normalized with mean and std. dev.
        - Doesn't work well for high-dimens (distances grow fast)
        - Kd-tree, hash-table with locally sensitive hash, ...
    - Nearest neighbors regression (locally weighted via kernel distance func)
    
- Support vector machines
    - Construct maximum margin separator, create linear separating hyperplanes
    - Embedding data into higher dimens. space using kernel methods
    - Non-parametric method: needs to keep subset of the data for classification (support vectors)

    - maximum margin separator: each datapoint has weight: 1 only for support vectors
        - only need to keep a few examples that have non-zero
        - can be solved via quadratic programming optimization
        
    - kernel: if examples not linearly separable -> using kernel function instead of dot product
        - poly: K(kj, xk) = (1+xj.xk)^d
        
- Ensebling: combine a collection of hypothesis
- Boosting (popular ensembling method): 
        - weight 1 for all examples
        - increase weight for misclassified, decrease for correctly classified
        - repeat
        - generate K hypothesis, each hypo contributes with the weight of its accuracy

- Overfitting: more likely as hypothesis space grows, less likely with more training data

# 9
- taking advantage of prior knowledge represented as logical sentences
    - attributes become unary predicates
    - classification becomes goal predicate

- hypothesis: Vx Goal(x) <> Cj(x) : extension of the predicate
    - e.g. WillWait(r) <> Patrons(r, Some) v (Patons(r, Full) and Hungry(r))...

- learning alg. believes that one hypothesis out of all hypoths is correct -> h1 v h2 v ... hn
    - hypoths not consistent with examples can be ruled out: false negatives, false positives

- keep one hypothesis -> adjust with new examples to maintain consistency
    - false negative: generalize
    - false positive: specialize
    - current best: iterates over all possible specializations / generalizations -> DFS

- generalization: h1 is generalization of h2: Vx: C2(x) -> C1(x)
    - if Ci is conjunction of predicates -> dropping conditions / adding disjuncts
    - specialization has inverse property & can be done by removing dijsuncts / adding conds.

- after each modification we should check all previous examples 
    - current best can make the hypothesis non-consistent with previous examples
- we might need to backtrack if there's no simple modification of current hyp possible
-> solution: least-commitment search

Version space learning:
    - assume whole hypothesis space as disjunction of all hypothesis
    - whenever there's hypothesis inconsistent with an example -> remove it
    - remaining set of hypothesis is called version space
    -> incremental: no need to ever backtrack 

- representation of version space: 
    - General bound: consistent with all examples, no more general consistent, init: True
    - Specific bound: consistent with all examples, no more specific consistent, init: False 
    -> everything between S and G is guaranteed to be consistent, nothing else is consistent

- Version space update: 
    - False positive for Si: throw Si out
    - False negative for Si: replace Si by all its immediate generalizations
    - Inverse for Gi

    - repeat until: only one hypothesis left, G or S becomes empty, run out of examples
        - majority vote is possible if they disagree / their disjunction 

- if there's noise in domain / insufficient attributes -> the version space always collapses
    - no simple solution known
- if we allow unlimited disjunction 
    - S will contain disjunction of descriptors of positive examples
    - G will contain negation of disjunction of negative examples' descriptors
    -> limit forms of disjunction -> generalization through hierarchy of more general predicates

Inductive logic programming: 
- combination of inductive methods and first-order representations
- top-down: FOIL
- inductive learning with recursive deduction: PROGOL

- Examples given as prolog fact, known classifications also prolog facts
- Possible hypothesis specified in first-order

- Top down learning: starts with a clause with an empty body
    - starts with classifying every example as positive
    - specialized by adding literalls one at a time to the body
    -> prefer generalized specializations

    - builds clauses covering positive examples
    - algorithms:
        - extended examples: set of examples with values for new variables
        - while non-empty positive examples: add new clause, remove positive examples covered
        - new clause: target is head, empty body : build goal <- f(parts from extended examples)
            - while examples contain negative ones: 
                - choose new literal (must contain a variable already in use), append to body
                - new_examples: extend-example with literal for all previous examples
        - extend-example: creates set of all literals that could be use to build current clause 
            - if example satisfies literal: return all examples with each possible constant value for each new variable in literal | else empty set

- inverse resolution / deduction 
    - classical resulution deduces Classification from Background, hypo, and descriptions
    - run the proof backward and deduce hypothesis given classification, background, and descriptions

# 10
- learning probability models
- bayesian learning: calculates probability of each hypothesis given data
    - prediction using all hypothesis weighted by their probabilities
    - P(hi|d) = alpha * P(d|hi) * P(hi)
    - Prediction is made: P(X|d) = SUM_i P(X|d,hi) * P(hi|d)

    - Prior hypothesis: P(hi), Likelyhood of data under each hypo: P(d|hi)
    - Posterior probability of false hypothesis will eventually vanish
    - The prediction is optimal no matter the data set size

    - Problem: hypothesis space is either very large or infinite -> prior approx

- MAP learning: making predictions based on most probable hypothesis
    - optimization problem instead of (infinite) sumation

- Prior probability can penalize complex models -> prevents overfitting
- When only deterministic Hi present -> P(d|hi) = 0(inconsistent)|1(consistent)

MDL learning: 
- maximizing P(d|hi)P(hi) = minimizing -log(P(d|hi))-log(P(hi))
    - P(hi): bits "required to specify the hypothesis"
    - P(d|hi): additional number of bits to specify data given hypothesis
    -> MAP provides highest compression -> minimum descriptor length learning method

- minimum descriptor length: explicitly tries to minimize binary encoding of hypo + data

- simplification: assume uniform prior -> finding maximum-likelyhood hypothesis (ML): P(d|hi)
    - problem with small data sets, good approx of bayesian for large datasets

Maximum likelyhood parameter learning:
- we can try to learn parameters for fixed structure model that maximizes likelyhood
- maximization through derivative and finding extreme / gradient descent 

- usually used in conjunction with Naive Bayes model: attributes conditionally independent
    - all attributes are leaf nodes of the class variable (root)
    - P(C|x1, x2, x3, ...) = alpha * P(C) * TT P(xi | C)
    - scales linearly, no problems with noisy data, gives probabilistic predictions

- unobservable hidden variables: 
    - modeling it can drastically reduce the number of parameters required
    - we don't know values for examples (idea behind EM): 
        - prentend we know the model's parameters
        - infer values for hidden variables given model
        - find new parameters using maximum likelyhood
        - iterate until convergence

    - EM:
        - starts with random guess for parameters
        - computes P(z|xi) given paramters (i.e. soft-assigns values for hidden variables)
        - recomputes the parameters given computed soft-assignments
        - repeats until convergence

        - "soft k-means" -> clusters data through the hidden variable

        - Oi+1 = argmax_O( SUM_z( P(Z=z | x, O)*L(x,Z=z|O) ) )
        - armgax of expectation of the hidden variable given parameters


# 11
- what if we don't have supervision (gold data) but just feedback (good/bad) 
-> reinforcement learning

- many possible designs:
    - utility-based: learns its utility function, must model enviroment: (state, action -> state)
    - Q-learning: learns action-utility function for each state
    - reflex agent: learns a policy that maps directly from states to actions

- passive learning: learns utility of states given fixed policy
- active learning: must explore to learn policy

Passive reinforc. learning
    - learns how good a policy is
    - agent doesn't have transition model or reward function
    - direct utility est: utility of state is expected total reward onward
        - average for each states if visited multiple times
        - obeys Bellman equations for a fixed policy
        - searching a much larger space than it needs to be -> convergence is slow

- adaptive dynamic programming
    - agent learns transition model & rewards model
    - utility of states is computed from Bellman equations using modified policy iter.
    
    - iteratively computes transition & rewards model (through counting what states it sees)
    - always udpates U to be consistent with updated transition and reward models

    - adjusts state's utility to agree with all of the successors given learned transitions
    - makes as many adjustments as it needs to restore consistency between estimates

- temporal difference learning:
    - use observed transitions to adjust utilities of states to agree with equations
    - utilities of states reachable from current state influence current state
    - general update: U(s) <- U(s) + alpha * (R(s) + y*U(s') - U(s)) 
        - y: discount
        - alpha: learning rate

    - adjust a state to agree with its observed successor
    - single adjustment per observed transition -> less stable

- active learning: improve (randomly initiated) policy while learning Utility func
    - while learning choose best actions recommended by current policy
    - explotation vs exploration
    - greedy agent doesn't explore enough -> choose random action with some p
        - p should decrease with time / with increase of policy fitness

    - good ideas:
        - increasing weight of actions not yet explored 
        - decreasing those believed to be of low utility
    
    - Monte carlo search: U(s) <- R(s) + y*max_a f(SUM_s'( P(s'|s,a) * U(s') ), N(s,a))
        - N(s, a): number of times action a has been tried in s
        - U(s): optimistic estimate of utility
        - f(u, n) exploration function, defines tradeoff between exploration vs explotation

        - propagates the benefits of exploration back so that paths towards unexplored regions are weighted more highly
            - reason why there's the optimistic estimate of utility on the right side

    - for active temporal difference learning agent there's a need for transition model 
        - To select the best action for current state (only have utility func. for states)
        - Needs to learn the transition model as well

- Q learning
    - Q(s, a) denotes value for doing an action a in a state s
    - U(s) = max_a(s, a)

    - TD agent learns with Q function doesn't need explicit transition model P(s' |s, a)
    - At equilibrium: Q(s, a) = R(s) + ySUM_s'( P(s'|s, a) * max_a'Q(s', a') )
    - Update: Q(s, a) <- Q(s, a) + alpha * ( R(s) + y * max_a'Q(s', a') - Q(s,a) )
        - whenever action a is executed for state s leading to state s'

- SARSA
    - close relative to Q-learning
    - instead of maxing over 'a in update it updates after 2 actions & uses true a, a'

    - for greedy agent it's identical to q-learning
    - for exploring agents:
        - Q learning pays no attention to current policy, can learn well even with bad/random exploration policy
        - SARSA uses current policy to in the estimate of Q (follows it to get the quadrouple, s, a,  s', a') in terms of what would actually happen instead of what we would prefer to happen (Q learning) 

- both SARSA and Q-learning slower than ADP agent, local updates don't force consistency
- both converge to optimal policy given enough time

- model free methods (e.g. Q-learning) don't need knowledge base for environment modeling
- for more complex environments knowledge based approach with explicit trainsition model can be better (ADP)




