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