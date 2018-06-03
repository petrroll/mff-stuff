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
