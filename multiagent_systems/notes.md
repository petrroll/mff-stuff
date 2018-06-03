General
- agent: system capable to work independently on behalf of its user
    - autonomus: not everything is defined beforehand
    - can have their own interrests 

- different views
    - MAS as part of AI
    - AI as part of MAS (AI tries to create intelligent agents)

- main problem
    - distributed communication 
    - individual action selection 

- agent:
    - he who has ability for autonomus execution vs. he who performs domain orient. reasoning
    - anything that perceives thourgh sensors and acts upon that
    - persistent routine with its own agenda and special purpose
    - ...

- environment
    - observability
    - static/dynamic
    - determinism
    - discrete/continuios

- notes
    - autonomy: needs to be able to choose autonomously, choose and follow subgoals, ...
    - deliberation: needs to select the best actions to fullfil its goals

- intelligent agent is: reactive, proactive, social
    - hard to balance reactivity (respond to changes) and proactivity (planning for goals)

- intentional system: entity whose behavior can be predicted by assigning properties s.a. beliefs, desires, ...
    - I.s of first order, second order (beliefs about beliefs), ...

- intentional stance
    - physical statence: understand the complete laws of the system
    - design stance: understand reasons why and how it works, what's the purpose, not the underlying physical laws (teleological expl.)
    - intentional stance: use intentional system view for prediction
        - physical design / descr might not be available / practical
        - can be very sipmle, though sometimes too simplifying
        - Is there intenionality by itself or it only an interpretation?


Abstract archs:
- Environment: can be in a finite number of discrete states -> discretize if needed
- Actions: means of the agent to interact with environment and change it
    - We don't know what environment state will be the result of curr. state and some action

- Run: sequence of env. states and actions 
    - r=e0,a0,e1,a1, ...
    - R: set of all possible sequences
    - RA: those that end with action
    - RE: those that end by env. state 

Env:
- state transofrmer function: T: RA -> 2^E
    - maps the influence of an egent to environment: lsat action -> possible new env. states
    - new env. state can depend on history of prev. actions & states
    - T(r) = {} -> system has finished a run, no more possible env. states

- Env = (E<possibleStates>, e0<initialState>, T<stateTranspformerFunc>)

Agent: RE -> A
- function that maps runs to actions, selects one action (usually defined as deterministic)
- generally can decide based on whole run history


System: Pair of environment and agent
- R(Ag, Eng): Set of runs of agent Ag in environment Env, only those that end

- agents func. equivalent with respect to Env if: R(A1, Env) = R(A2, Env)
- agents simply func. eq if: func. eq. with respect to all Env.

Types of agents:
- Pure reactive (tropist) agent:
    - E -> A, only takes the last env. state into action, no history, no memory
- agent with a state
    - has internal state I, sees percepts Per
    - see: E -> Per     // sees the environment
    - action: I -> A    // selects best action
    - next: I x Per -> I // creates new state

Utility of run:
- utitlity of state: u: E -> Real
- success of an agent: average utility of states, the utility of worst of the states

- utility of run: u: R-> Real
- better for agents running independently for a longer time

- combination: continuous rewards, run based return

- optimal agent should maximize expected utility: argMax{Ag€AG} SUM( u(r)*P(r|Ag, Env) )

Predicate specification of a task:
- R(r) is true <> u(r)=1 -> successful run: criteria if agent fulfills the task 
- Environment of a task is (Env, F), TE: set of all environments of a task
    - RF(Ag, Env) = {r|r€R(Ag, Env) & F(r)}

- what does solving a task mean?
    - All runs satisfy F
    - At least one run satisfies F
    -> let's compute the probability that agent will be successful: P(F|Ag, Env) = ...


Deductive reasoning agent
- symbolic repres. of environment & behaviour
- syntactic manipulation of such repre -> logical deduction

- deduction: derivation of conclusions that are true based on preconditions
    - general -> specific
- induction: if predictions are true, conclusion is more likely true
    - specific -> general

Agent as theorem prover:
- agent has inner state DB of first order logic formulas
- deliberation is done through deduction/derivation rules P
    - DB |-P f : f can be derived from DB using rules P
- action selection as proving:
    - returns action that can be proven via Do(a)
    - or tries to find an action that isn't in contradiction with DB

- takes a lot of time, hard to convert environment to formulas, temporal representation problem
- nice semantics, tho

Practical reasoning agent:
- two phases
    - deliberation: what do we want to achieve
    - means-end reasoning: how to achieve such goal

- BDI architecture 

- [I]ntention: state of the env. agent wants to achieve
    - persists until: are achieved, start seem unachievable, change due to reasons
    - constrain further deliberation, influence what agent will believe in the future

- [D]esire: one of possible intentions
- [B]elief: agent's knowledge 
    - subjective, not necessarily true, can change
    - might contain inference rules

- Deliberation: 2^B x 2^I -> 2^Des  // intentions & beliefs -> desires
- Filter: 2^B x 2^D x 2^I -> 2^I    // All I know -> less intentions
- Belief refresh 2^B x Per -> 2^B

- Means-end reasoning: 
    - how to reach goal (selected intention) based on possible actions
    - outputs a plan: sequence of actions that if followed fulfills the goal

    - STRIPS
        - world is modeled as set of first order logic formulae
        - set actions: preconditions & effects (add~new true facts, delete~makes facts false)
        - planning alg: trough actions decrease distance between curr state & goal
    

- plan: set of actions Ac {a1, a2, ...}
- descriptor of an action a is [Pa, Da, Aa]
    - precodnitions, delete effects, add effects

- planning problem is (B0, O, G): 
    - initial beliefs, O: descriptors of all actions, G: set of formulae representing goal

    - plan (a1, a2, ...) determines a sequence of belief databses B1, B2, ...
        - Bi+1 = (Bi\Dai u Aai) // add new facts, delete other facts given the action
    
    - plan: 2^B x 2^I x 2^Ac -> Plan, 
    - instead of creating plan from scratch -> use library of plans -> iterate and check
        - precodnitions correspond to current beliefs
        - effects correspond to current goal

- generic planning loop
    - v = see(), B = brf(B, v), D = options(B, I), I = filter(B, D, I)
    - p = plan(B, I, Ac)
    - while not (empty(p) or success(I, B) or impossible (I, B)) 
        - a = hd(p), execute(a), p=[1:] //choose action and execute it
        - v = see(), B = brf(B, v)
        - if reconsider(I, B) then D=options(B, I), I = filter(B, D, I)
        - if not sound (p, I, B) then p = plan(B, I, Ac) // if curr plan doesn't work for reconsidered intention / changed beliefs -> new plan

- commitment of agents: when to abandon goal and reconsider
    - blind commitment: never reconsider
    - single minded commitment: follows until achieved / believed to be impossible to achieve
    - open-minded: follows as long as it's believed to be achievable 

- when should agent stop and reconsider?
    - deliberation takes time, environment can change during it
    - too ofter -> busy reconsidering, doesn't do anything; too little -> follows wrong plans

    - before reconsidering ask whether I should recosnider: simpler, faster
    - good only if reconsider -> I will actually change to plan most of the times

    - bold agents: reconsider only after current intention finishes -> good for static worlds
    - cautios agent: reconsiders after every action -> good for very dynamic worlds

Procedural reasoning system: PRS
- implementation of BDI architecture
- agent has a library of ready plans, representing its procedural knowledge 
- no full planing, just selecting from the library

- plan: goal, context (preconditions), body (actions to execute)
    - plan's body doesn't have to linear sequence of actions
    - can contain sub-goals, achieve f, achieve f or g, keep achieving f until g

- planning in PRS: 
    - init: beliefs B0, top-level goal
    - stack of intentions
        - cotnains current goals in the state of partial completion
        - takes goal on top, finds a matching plan with consistent context

- deliberation in PRS
    - original PRS had meta plans that modifiet intentions, too complicated
    - every plan has certain utility, plan with highest utility is selected
    - execution of selected plan can add more intentions on the stack

    - if plan fails, choose another intention from options and continue ~ DFS

Reactive and hybrid architectures:
- symbolic repre and reasoning is problematic 

- emergence: inteligent behavior emerges by interaction of many simple behs.
- embodiment: inteligent beh. is not just the logic but product of agent and its body

- reactive agent: only reacts on the environment, doesn't do deliberation
    - simple if then rules, finite automata, ...

Subsumption agenta architecture:
- probably the most successful reactive approach

- each behavior is simple action selection mechanism
    - receives perceptions and transforms them into actions
    - reponsible for some goal
    - rule like structure
    - competes with others for control over agent
    - parallel to other behavior
    - subsumption hierarchy defines their priorities

- action selection
    - for each behavior checks if there's applicable with higher priority, if not -> selected
    - setting priorities can be tricky, otherwise simple & fast implementation

- good for swarm-like behavior (ants, ...)

Network architecture
- each agent is set of competence modules (resembling subsumption behaviors)
    
- preconditions, postconditions
- activation treshold (relevance to current situation, priority in selection process)
- connected in a network based on their condition 
    - matching pre and post conditions represented as oriented edge
    - further connections representing time precedence / conflicts, ...

Limitations of reactive approach:
- do not create model of the world, have to derive everything from environment
- to remember things, need to change the environment

- short-term view of the world: current state & local information

Hybrid architectures:
- combines reactive and deliberative components
- usually in hierarchy, reactive components tend to have precedence

- horizontal: parallel, need mediator function that resolves conflict between layers
- vertical: one-pass (bottom-up), two-pass (up go perceptions, down go actions)

- each layer can have it's own knowledge base with different abstraction level

- E.g.: horizontal modeling layer (models environment, sends goals to planning layer), planning layer (selects preprogrammed plans), reactive layer (immediate, fast, e.g. obstacle avoidance)
    - similar architecture could work vertically as well (InteRRaP)

IDA
- intelligent distribution agent, complex architecture
- combines many approaches from MAS and AI to achieve complex tasks

- inspired by the idea that mind is a multiagent system created of many simple processes
    - communication is seldom, usually via shared memory
    - dynamically create higher-order coalitions
    - most useful coalition will get command over body (consciousness)
    - hierarchy of contexts


Ontologies: 

