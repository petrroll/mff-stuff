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

