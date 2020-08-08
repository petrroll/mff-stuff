- Cognitive modeling
  - TopDown: Psychology
  - Bottom-up: Neuroscience

- Doing to right things

Building rational agents
- perceives environment through sensors and acts through actutors
- agent function V*->A
- rational one selects action that is expected to maximize its performance measure

Types:
- observable
- deterministic / stochastic
- episodic (does not depend on previous episodes) / sequential
- static / dynamic
- discrete / continuous 
- single / multi-agent

agent = architecture + program
- architecture: computing device with sensors and actuators
- program: implementation of agent function

Types: 
- simple reflex (only current percept)
- table driven (remembers complete sequence of percepts)
- mode-based (handles partial observability, keeps model of the world)
- goal based (search, planning)
- utility based (internalization of the performance measure)

Representation of environment:
- atomic: states are indivisible black-boxes : search, ...
- factored: states are fixed sets of variables : propositional logic
- structured: states are sets of objects with relationships, ... : first order logic

General learning agent: 
- can operate in unknown environment
- problem generator (generates actions that lead to learning), critic, mechanism for making improvements (changes performance element), performance element (initial structure that selects actions)

Problem solving:
- goal formulation
- problem formulation
- problem solving
- solution extraction

Well-defined problem:
- initial state
- transition model (successors)
- goal test
- path cost

Abstraction: process of removing detail from representation 
- valid: can we expand simplified solution into one in more detailed work
- useful: does it simplify the actions

Search space
- node: current state, link to parent (opt.), cost of path to root, depth, action (from parent)
- fringe (frontier) set of not-yet expanded nodes

Measures:
- completeness (guaranteed to find a solution if it exists)
- optimality
- time and space complexity

BFS:
- finds shallowest goal, not necessarily the optimal
- complete (provided finite branching factor)
- O(b^(d+1)) for time and space
- runs out of memory quickly

Uniform cost:
- expands nodes with lowest path cost (dijkstra, ...)
- complete if all steps >= e
- O(b^(1+c/e)), c cost of best solution time and space
- Can be much worse than BFS (prefers cheap not short)
  - expands more nodes, but optimal

DFS:
- O(b^m) time, O(bm) space
- not complete, not optimal
- backtracking: modify states, generate only one successor, ... O(1) space complexity
- we can limit depth : depth-limited search¨

Iterative deepening
- always increase limit by the one minimal path cost delta that was not taken
- complete, optimal
- memory O(bd), time O(d*b^1 + (d-1)b^2 ...) = O(b^d)
- good for large unknown search spaces

Bidir search:
- with BFS complete algh, lower memory needed O(b^(d/2))
- intersection of frontiers through hash table

Backward search:
- from goal to initial state
- if multiple goals: dummy goal that connects to all of them, meta-state, ...
- hard to apply when there's abstract description of goal state (n-queens)
- need to have a definition of all possible predecessors

Graph vs. tree search:
- expansion of already visited states, remember closed states

Search types:
- uninformed (blind)
- informed (heuristic) : exploit domain-specific knowledge

Best-first search
- chooses node with smallest f(n)
  - some evaluation function : quality of a node
  - part of f(n) is usually some heuristic h(n) estimating length to the cheapest goal state
  - if h(n) == 0 : n is goal state

Greedy best fit:
- expand the node that closest to some goal state (takes only the node's heuristic into account)
- usually don't have that type of info
- is not optimal, is not complete
- O(b^m) time and complexity

Astar:
- f(n) = g(n) + h(n) // g(n) is traveled distance
  - cost of path through n

Types of heuristics 
- admissible: h(n) <= cost of the cheapest path from n to goal
  - the heuristic is optimistic
- consistent (monotonous): h(n) <= c(n, a, n') + h(n') (triangle inequality)
- monotonous heuristic is always admissible
  - telescopic sum of h(ni) - h(ni+1) <= c(ni, a, ni+1) for each path
  - values of f(n) are not decreasing for monotonous heuristic

- for admissible heuristic Astar is optimal in tree-search
- needs to be monotonous for general graph
  - reaching the same state using better path for second time, monotonous heuristics prevents this

- contours : areas of the graph with the <= f cost
  - the more accurate h(n) the more stretched towards optimal goal
- Astar expands all nodes f(n) < contour
- can expand exponential number of nodes
  - the alg is good if the heuristics is reasonably good (diff between h(n) and opt <= O(log(path)))

IDA*
- iterative deepening where the search limit is defined using f(n)
- actually not that bad in terms of repetition

Recursive BFS (RBFS):
- remember & keep updated an alternative minimum value (above) at each level
  - when there's no path better on current level than the alternative minimum go one level up (and update alternative min one level above with the current path)
- optimal for admissible heuristic
- space O(bd), time (exp)

Memory bounded Astart (SMA):
- drop the worst leaf node (highest f-value) when OOM
  -backup its value to the parent, expand when needed again
- finds optimal path that fits into memory

Admissible heuristics:
- effective branching factor: what's the average branching factor the heuristic lead to
- both admissible, Vn: h2(n) >= h1(n): h2 dominates h1
  - "gives better estimates"
  - all nodes expanded by h2 get expanded by h1 as well

Heuristics creation:
- problem relaxation
- pattern database: taking cost of the worst sub-problem (pattern)
  - do NOT do sum, does not have to be admissible
- max of multiple simpler heuristics (if all are admissible)

Local search:
- don't need the path, only result
- e.g. 8 queens problem

Hill climbing:
- take the state with highest value of the objective function from neighborhood
- greedy, local optimas, cycling on plateauxes
- variants
  - stochastic: random probability ~ steepness
  - first-choice: random until a better than current successor
  - random-restart

Simulated annealing:
- moves randomly, accepts if its better / worse with some p given by temprerature (decreases over time) & steepnes

Local beam search:
- create multiple successors for multiple states
- not truly "parallel" alg. abadonds unfruitful searches -> keeps more successors of good positions 

Genetic alghs: viz EVA

Online search:
- interleaves computation and action
- good for dynamic & non-deterministic domains

- compatitive ratio: quality of online solution / best solution
- online algs can't avoid dead ends if there're irreversible actions
- even for safely explorable no bounded competetive ratio can be guaranteed

Online DFS:
- need to remember "callstack" manually, also need to remember results of <state, action> to be able to go back
- need to remember unexplored successors for each node
- each link is travelled twice at worst (forward, backward)
- only for state spaces with reversible actions (otherwise can't go back)

Learning in online search:
- Remember visited states and update their heuristics to escape local optima (update them trough info from their neighbours)
  - needs permissible heuristics

LRTA (learning realtime Astar):
- when I leave a node I update it's heuristic using its neighbours (e.g. new current node)
- when the action hasn't been taken yet (no info about what node it leads to), use heuristic for origin node

CSP: 
- variables, each has domain (finite), constrains
  - consistent state : assigment (partial) that does not violate any constraint
  - applicatble actions: search through them
  - goal always in depth "n" (number of variables)
- simple backtracking: DFS

Variable ordering:
- the most restricted first: smallest current domain (dom)
- the most constrained first: most constraints (deg)

Value ordering
- restict least of the other variables
- problem dependent heuristics

Forward checking:
- check constraints between (newly) assigned variable and not-yet assigned variables
- remove values violating constraints
- (possible) check constraints between future variables

Arc consistency:
- filter domain X whenever Y changes, repeat until no changes made: AC3 alg
- keeps queue of constraints that needs to be checked
- time: ed^3 (e: number of constraints, d size of domains)

K-consistency
- for any consistent assigment of k-1 variables there's consistent value for one more
- if a problem is i-consistent for i = n, it's solvable in backtrack-free way

- global constraints can be good to replace consistency
- soft constraints (heuristics)

Games:
- multi-agent, zero-sum, deterministic, perfect info
- MAX moves first (us), MIN moves second
  - utility function for MAX
- optimal strategy : expects perfect opponent

Minimax:
- MIN selects min, MAX selects MAX, from terminal leaves up (BFS)
- a,b pruning: eliminating branches that can't influence outcome (remember bounds for each node & root)
  - stop evaluating min when it's value is worse (smaller) than in its parent (MAX)
  - a is value of the best (highest) choice we have found along the PATH
  - does not miss optimum, with perfect ordering the avg. branching factor gets squared
- end before terminal nodes, compute utility (heuristic)
  - expected value (based on features of the state)
  - sum of values of certain features
- horizont effect: big change can happen after cut-off
  - don't cut-off before big events (capturing in chess, ...)

Stochastic games
- add chance nodes, compute expected value over them

Knowledge based agent:
- TELL, ASK, inference: deriving new info from old

Inference
- propositional variables describe the properties of the world
- formulas describe known information about the world
- Algs
  - model checking (enumaration of truth table)
  - inference rules (resolution alg)

  DPLL: 
  - sound and complete alg for verifying satisfiability of formulas in CNF
  - DFS with heuristics 
    - for symbols that has the same sign in all clauses
    - unit clauses
  
WALK-SAT
- sound but incomplete
- starts with random assigment
  - with p flips a random symbol in false clause
  - else flips the symbol from a false clause that maximizes number of satisfied clauses
- way faster than DPLL, even DPLL works reasonably well

Resolution:
- proves unsatisfiability of (KB and !alfa)
- sound, complete

Horn clauses: 
- disjunction of literals, at most one positive (implications)
- forward chaining: 
  - linear time complexity
  - sound, complete
  - keeps list of symbols known to be true, inferns from them

- backward chaining:
  - query is decomposed via horn clauses into subqueries until facts from KB are obtained

  First order logic
  - objects and relations
  - TELL adds axioms, definitions, and theorems (speeds up induction)
  - reasoning: 
    - grounding (instantiate with all possible terms)
    - replace quantifiers with skolemization (E: constant, V: all instances)
  - full instanciation can lead to infinite number of terms
    - Herbrand: only finite subset is enough, don't know how large


Inference in FOL
- lifting: do only needed substituions
- modified Modus ponens with substitution 
- the most general unifier: most general substitution
  - build MGU recursively
  - complex terms must have the same name, unit args
  - x, f(x) not unifiable (check not done in Prolog)
- standardizing apart: rename variables so they don't colide 

- Forward chaining: infer all valid sentences through Modus Ponens
  - sound, complete, does not have to finish if sentence not backed by KB
  - pattern matching in NP complete problem
  - incremental: verify only relevant rules (whose body facts were added to KB in previous step)
  -> dependency network (precomputed)
  - magic set: keep only relevant constant for our goal (constructed via backward exploration of rules)

- Backward chaining: find for facts supporting our goal, used in logic programming
  - not complete if implemented as DFS (can cycle)

- resolution
  - needs CNF first
  - sound, complete
  - does not always produce constructive proofs
  - unit resolution, input resolution (isn't complete), subsumption (removes more specific clauses)

Knowledge engeneering:
- identify the task
- assemble relevant knowledge
- decide on vocabulary
- encode general knowledge
- encode specific problem
- pose queries
- debug 

- agents manipulate objects
- reasoning in the world of categories
  - MemberOf, SubsetOf
- taxonomy: hierearchical structure to categorize objects

Planning: 
- dynamic world
- in PL we'd need copy of each action for each timestep
  - we can do better in FO logic

Planning in FO
- actions are terms
- situations are also terms
- need to reason about sequences of actions
- fluent a predicate that changes with time (needs situation as last argument : represents time)
- tasks:
  - projection task: what's the result of these actions
  - planning task: what actions do I need to get this state
- each action described using two axioms
  - possibility
  - effect of possibility
- frame problem: axioms describe what has changed but not that everything not changed is still the same
  - need it for Fluents and Actions

- successor-state axiom:
  - one axiom for each fluent with O(AE) literals
  - Poss(a,s) => (fluentHoldsIn Result(a,s) <> fluent is effect of a) or (fluent holds in s and a does not change it)
  - beware of implicit effects: ramification problem

  - can be simplified with meta axiom that contains PosEffect and NegEffect axioms
  

  - closed world assumption: what's not explicitely true is false

Classical representation
- state is set of inited atoms (no variables)
- closed world assumption (not included -> does not hold state)
- o(name, precond:literalsThatMustHoldTrue, effects:onlyFluents)
- action is fully instantieted operator

- action is applicable if precodn is subset of current state
  - result is original state - effects- + effects+

- planning domain: (S,A,y)
  - states, actions, transition function
- planning problem (E, s, g)
  - planning domain, initial state, set of gloal literalls
  - plan is sequence of actions that satisfie goal condition

- forward search: apply actions until reaching goal state
  - randomly choose
- backward search (regression): apply actions in revers order from goal
  - apply only partially applied actions (lifting)
  - remember regression set ("intermediate goals")

planspace planning:
- start with empty plan, add actions to satisfy not yet covered goals
  - initial state is an action with no pre-requisites
  - goal is an action with no effects
- partial plan
  - tuple: partially inited operators, set of constraines, set of causal relations, partial ordering
- ordering (just order) and causal relation (actions has to be directly after each other)
- flaws:
  - open goal: not yet satisfied precondition (binding variables, finding new operator, ...)
  - threat: action that can't be inbetween a causal relation

- partial plan is solution if no flaws, ordering consistent, and instantiated


state space planning
- search through discrete states







