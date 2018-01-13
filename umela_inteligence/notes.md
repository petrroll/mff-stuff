- Cognitive modeling
  - TopDown: Psychology
  - Bottom-up: Neuroscience

- Doing to right things

Building rational agents
- perceivies enviroment through sensors and acts through actutors
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

Representation of enviroment:
- atomic: states are indivisible black-boxes : search, ...
- factored: states are fixed sets of variables : propositional logic
- structured: states are sets of objects with relationshipts, ... : first order logic

General learning agent: 
- can operate in uknown enviroment
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
- completeness (guaranteed to find a soulution if it exists)
- optimality
- time and space complexity

BFS:
- finds shallowest goal, not necesserily the optimal
- complete (provided finite branching factor)
- O(b^(d+1)) for time and space
- runs out of memory quickly

Uniform cost:
- expands nodes with lowest path cost (dijstra, ...)
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