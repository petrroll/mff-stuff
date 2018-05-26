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

    - P(X|e) = a * N(X,e) * w(X,e) = a * N(X, e) 
        - w(z, e) = TT_j(ej|parents(ej))
        - P(z, e) = TT_i(zi|parents(zi))

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
- smoothing: posterior distro over past tate given evidence to the present
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

- most-likely sequence
    - states depend only on previous state -> there exists a recursive formula
    - viterbi algorithm: maximalizační cesty pro všechny koncové stavy
    - distribuce pro m1:t+1: P(et+1 | Xt+1) * max_xt( P(Xt+1 | xt) * m:t )

Hidden markov models
- state described by single discrete variable Xt, evidence variable Et
- Transition Tij: P(Xt = j | Xt-1 = i) and sensor model Oij: P(Et = et | Xt = i)
- filtering: f1:t+1 = alpha * Ot+1 * T^t * f1:t
- smoothing: bk+1:t = T * Ok+1 * bk+2:t

- full smoothing: O(t) with dynamic programming, O(|f|t) memory
    - we can first run forward pass and then backward with constant memory
    - computing f1:k from f1:k+1 using inverse matrixes for T and O

- fixed lag smoothing> smoothing for fixed number of d steps back in time P(Xt-d | e1:t)
    - f1:t-d+1 = alpha Ot-d+2 * T^t * f1:t-d
    - bt-d+2:t+1 = Ot-d+1^-1 * T^-1 * Bt-d+1:t * T * Ot+1

# 4
Dynamic bayesian network
- bazesian network that represenets a temporal probab. model
- variables and links replicated from slice to slice

- hidden markov model is special case of a dynamic Bayesian network
- dynamic b. n. can be encoded as hidden MaSrkov model (variable that is tupple of values of vars in DBN)

- DBN can be more compact, doesn't have to contain all transitions for independent variables
- in inference only last slices are neccessary to keep

- aprox inference: generating samples, weighting them with evidence
    - problem: possibly very small weight of each sample
- particle filtering: keeps possible samples
    - starts with prior distribution
    - next state is generated through transition model probability
    - weighted by likelyhood it assigns to evidence
    - resample population given their weights
    - repeat for next state

Senzor failiures:
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
    - maximizing joint probability of current obs. and predicted positions (commits to single best at each step)
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
    - average over all values for the information we might discover
    - information has value only if ot can change the plan and the new plan would be significantly better
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
- Sequential decision process for 
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
    -> converges to one solution of Bellman equations
    - the function has only one fixed point, gets closer to it with every update

- Convergence: 
    - |BUi - BU'i| <= y|Ui - U'i| // The probabilities in exp. val for updates and Rewards are the same for U and U'
    - |BUi - U| <= y|Ui - U| // U: vector of true utilities -> fixed point

- Policy loss: how much can agent loose by executing policy p instead of optimal
    - if |Ui - U| < e -> |Upi - U| < 2ey/(1-y)

- policy iteration alghoritm
    - evaluation: given policy pi, calculate U_pi
        - Upi(s) = R(s) + y SUM_s'( P(s'|s, pi(s))Upi(s') )
        - O(n^3)
    - policy improvement: pi_i+1(s) = argmax_a€A(s) (SUM_s'( P(s'|s,a)Upi(s') ) )
    - iterate as long as we're changing the policy
    - finite number of policies -> always terminates

- if the world is only partially observable -> partially observable	Markov decision proces
    - transition model P(s'|s, a), reward function R(s), sensor model P(e|s)
    - current state is potentially uknown -> hard to reccoment action via policy
    -> belief state, pribability over all possible states
    -> optimal policy for belief states 

    - forwarding belief state works as expected

    - given belief state execute action a = pi(b)
    - receive percept e
    - set current belief state b' = FORWARD(b, a, e)

    - if observation is not available sum over all possible given their probability
    - reward is expected reward given current belief state

    - POMDP on physical state space corresponds to MDP on belief-state space
    - MDP techniques can't be used on POMDP due to it having infinite number of states (the belief state space if continuous)

    -> conditional plans: action selected on observations and previous actions
    - value iteration can be modified to work -> not efficient
    
    -> Dynamic bayesian networks with look-ahead
    - represents transition and sensor models
    - added nodes for actions and rewards 

    - lookahead via expected minimax alg through Tree with belief states>-actions->evidence
        - depth of search determined by discount factor
        

# 7

# 8
