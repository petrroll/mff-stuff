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
    - he who has ability for autonomous execution vs. he who performs domain orient. reasoning
    - anything that perceives through sensors and acts upon that
    - persistent routine with its own agenda and special purpose
    - ...

- environment
    - observability
    - static/dynamic
    - determinism
    - discrete/continuos

- notes
    - autonomy: needs to be able to choose autonomously, choose and follow subgoals, ...
    - deliberation: needs to select the best actions to fullfil its goals

- intelligent agent is: reactive, proactive, social
    - hard to balance reactivity (respond to changes) and proactivity (planning for goals)

- intentional system: entity whose behavior can be predicted by assigning properties s.a. beliefs, desires, ...
    - I.s of first order, second order (beliefs about beliefs), ...

- intentional stance
    - physical stance: understand the complete laws of the system
    - design stance: understand reasons why and how it works, what's the purpose, not the underlying physical laws (teleological expl.)
    - intentional stance: use intentional system view for prediction
        - physical design / descr. might not be available / practical
        - can be very simple, though sometimes too simplifying
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

BDI architecture 

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

- Touring machines: horizontal 3-layer
    - modeling: selects goals for planning layer
    - planning: PRS with preprogrammed subgoals
    - reactive: e.g. obstacle avoidance, immediate reactions

- interrap: vertical 3-layers
    - cooperative (interaction w. agents)
    - local planning
    - reactive behavior layer

- Stanley
    - won DARPA challenge
    - layers: sensors, inputs, planning, control (driving), car interface, UI, global services, ...


IDA
- intelligent distribution agent, complex architecture
- combines many approaches from MAS and AI to achieve complex tasks

- inspired by the idea that mind is a multiagent system created of many simple processes
    - communication is seldom, usually via shared memory
    - dynamically create higher-order coalitions
    - most useful coalition will get command over body (consciousness)
    - hierarchy of contexts


Ontologies: 
- part of philosophy dealing with nature of being
- in cs: explicit and formalized description of part of reality
    - glossary: definition of concepts, thesaurus (relations between concepts)

- ontologies in general: classes, instances, properties, relations, ...
    - structurela part
    - facts about concrete items

- ongology of ontologies (types of ontologies):
    - dictionary of selected terms
    - glossary: definition of meanings by selected terms: in natural language
    - thesaurus: definition of synonyms, ...

    - formal is-a hierarchy: formal classes
    - clauses with properties
    - value restrictions
    - arbitrary logical contrains

    - upper: ultimate ontology 
        - base for domain ontologies
        - Wordnet, ideas: not suitable for formal reasoning, machine usage
    
- ontological languages
    - formal declarative languages for knowledge repre
    - cotain both facts and reasoning rules
    - based on first order / description logic

    - frames: predecessor of ontologies: visualisation of human reasoning and lang. processing

- examples of ontologies:
    - xml: not developed for ontology repre.,used sometimes, tag definition:dictionaries
    - RDF(resourcedefinition framework)
        - simple, not very expressive, represents triples (subject, predicate, object)
    - OWL(Web ontology language)
        - collection of several formalisms to describe ontologies
        - can use xml, rdf syntax, ...
        - open world assumption: everything we don't know is undefined 

        - OWL-lite: close to RDF, many axiom contraints to maintain simplicity
        - OWL-DL: corresponds to DescrLogic, allows class disjunction, complete decidable
        - OWL-FUll: strongest expressive power, many problems undecidable

        - OWL-EL: polynomial reasoning time complexity
        - OWL-QL: specilized to queries in knowledge bases
        - OWL-RL: special form of axiom rules
    
    - KIF: representation of knowledge in first order logic ~ content -> refers to ontologies
    - DAML+OIL: Darpa agent markup language + ontology interchange language, predecessor of OWL

- Description logics: 
    - copcepts (classes), roles (properties, predicates), individuals (objects)
    - axiom: logical expression about roles/copcepts ~ not about whole class but just properties
    - possible extensions: hierarchy of roles, transitivity of roles, class disjunction 
    - limited existential quantifier, contraints, ...


Agent communication
- communication in OOP: calling object methods with parameters
- agents can't directly communicate -> message passing -> other agents don't have to comply

- Speech act: parts of language with character of actions -> they change the state of the world
    - verbs as requests, informs, promises, orders, ...

    - what was said 
    - what was meant (location + performative meaning) 
    - what really happened (effect of the speech act)

    - can have precoditions: 
        - hearer must be able to perform action that is the subject of speech act
        - ...

- searle speech acts categories
    - oder: request, advice, statement, promise
    - newer: 
        - assertives: information
        - directives: request
        - comissives: promise by a speaker
        - expressives: speaker expresses mental state
        - delcarations: change the state of things
    
    - speech acts should have: performative word (request, inform, ...), propositional content 

- planning theory of speech acts:
    - speech acts can be seen as actions ~ planning systems are applicable
    
    - STRIPS: preconditions & postconditions
    - modal operators: beliefs, abilities, wants
    - semantics of speech acts is defined by means of precondition delete-add approach from STRIPS

- agent communication languages
    - KSE: knowledge sharing effort
        - KQML: knowledge query and manipulation language: 
            - outer communication: envelope
            - contains what was meant

            - performative, content, receiver, language, ontology
            - performatives: achieve, advertise, ask-about, ask-all, forward, ...
            - parameters: content, force, reply-with, sender, receiver, ...

        - KIF: knowledge interchange format: inner language, propositional contents of message, knowledge repre

    - FIPA ACL: simplification of KQML semantics, better performance, practical implementation in JADE
        - more expressive pefromatives: proxy, refuse, agree, cancel, propose, ...


Cooperation of agents: 
- agent have different goals, are autonomous
- work in time, not hard-wired, decisions made at runtime

- sharing tasks, sharing information

- coherence: how the system performs as a whole
- coordination: how well agents minimize overhead related to sync

- CDPS: cooperative distributed program solving
    - cooperation of indiv. agents when solving a problem
    - benevolence: agents implicitely share common goal, no conflicts
        - agents helps toward common goal even if it is disadvantageous for it
        - simplifies system design 

    - focus on parallel solving, homogenous and simple processors
    - differences to MAS
        - MAS has society of agents with their own goals, not one shared
        - MAS agents should still cooperate: resolving conflicts, negotiation, bargaining, ...

    - task & results sharing
        - hierarchial, recursive
        - new agent for each sub-problem, untill instruction level
        
        - agents usually share some information, might need to sync their actions
        - hierarchical solution synthesis

- CNET: ContractNet
    - metaphore for task sharing via contract mechanism
    - agent recognizes it has a problem it can't solve on its own
        - broadcast annoucnement of problem description & contraints 
        - other agents bid on the contract, original agent selects a winner and awards contract
    - can lead to hierarchical cascades of sub-contracting

- BBS - blackboard systems
    - all agents share a common data structure - blackboard, all can read/write
    - tasks dynamically appear on BB
    - when agent can solve some task/part will solve it & write (partial) solution to BB

    - requires mutual exclusion over BB - sync -> bottleneck
    - can contain implicit hierarchy

    - arbitr: selects experts who can come to BB, reactive or considers plans maximizing exp. utility, higher level problem solving (motivation)
    - experts: agents that can solve (parts) of problems through cooperation, execute actions when selected
    - BB: shared memory, information representation is important

    - E.g. wargame: BB with open missions, experts are soldiers that look for attack-location missions, commander transforms larger attack-city to multiple attack-location tasks

    - simple, experts do not need to know about other experts, rewriting tasks on BB (subtasks, ...)
    - distributed hash-tables for BB can help BB-sync bottleneck

    - Results sharing: can help
        - confidence: indep. results of similar/idtentical solutions can be compared
        - completeness: agents share their local views to create global idea of the problem
        - precision: sharing can improve overall precision of the solution

- FELINE
    - sharing of knowledge, distribution of sub-tasks
    - each agent is rule-based system: skills (I can prove/contradict), interests (I'm interested if the following is true/false)
    - communication through messages: hypothesis + speech acts, request, response, inform

Self-interested agents
- agent doesn't have to be honest about capabilities, doesn't have to finish an assigned task
- if possible benevolence is a good strategy

- if system too comples -> common goal can be hard to identify
- possible conflicts between common goal and goals of individual agents

- sometimes better to consider selfish agents

Selfish agent
- maximizes expected utility, all other agents want the same
- suppose me and all other agents act rationaly
- doesn't have to maximize common utility, is robust


Game theory
- strat is dominant if it provides better/same result than any other strat against all opponent's strats
- Nash equilibrium: s1 and s2 are in Nash eq: if neither agent would be better off switching strategies
    - not all games have Nash eq. in pure strategies
    - some games have multiple
    - Nash thm: every game with finite number of strategies has Nash eq. in mixed strategies -> ~NP hard
- pareto opt: no other strategy exists that would improve result without worsening others' results
    - front of pareto strategies

- agents could maximize common utility: social wellfare: sum of utilities of all agents
    - useful only in cooperation scenarios 

- prisoner's dilema
    - DD is Nash eq
    - DD is the only non pareto-optimal solution
    - CC maximizes social wellfare


Voting mechanisms:
- plurality: 
    - each voter submits preferences, 
    - each candidate gets one point for every preference ranking them first
    - most points win 

    - for two candidates it's simple majority election
    - with more winner doesn't have to be preferred candidate

    - leads to tactical voting: voting against some candidate, not following my preferences
    - condorcet paradox: no matter which outcome we choose, a majority of voters can be unhappy
        - social wellfare can be cyclic although individual preferences are linear
        - A > B > C, B > C > A, C > A > B

- sequential majority elections
    - plurality with pariwise tournament rounds, winner moves further
    - linear or tree tournaments, order of tournament influences election

- borda count
    - each voter submits complete preferences
    - aggregation of of orders of all candidates

- slater system
     - select a ranking of cadidates to minimize the number of paris of cadidates such that the ranking disagrees with the pairwise majority vote on these two candidates
     - NP complete

- properties of voting procedures:
    - pareto property: if everybody's preference is X > Y then X >sw Y
        - holds for majority and Borda
        - doesn't hold for sequential majority

    - condorcet winner: candidate that beats opponents in pairwise comparisons should win elections
        - sounds reasonable, holds true only for sequential majority
    
    - independence of irrelevant alternatives: whether X > sw Y should depend only on relative orderings of X and Y
        - if the orderings of X and Y remain their outcome ordering should also not be affected
        - different outcomes for other candidates shouldn't affect ordering 
        - does not hold for majority, sequential majority, nor borda

        - might be too strong, given preferences paper, scisor like relations

    - unrestricted domain or universiality: 
        - for any set of individual voters preferences the social welfare function should yield unique and complete ranking of societal choices
        - all preferences of all voters are allowed
    
    - dictatorship
        - social outcome is dtermined by one of the voters - dictator
        - non-dictatorship: there's no voter that can influence preference betwen all x and y states given all possible orderings 

- arrow's theorem: for 3 and more candidates, no ranked voting electoral system can convert ranked preferences while meeting:
    - unrestricted domain
    - non-dictatorship
    - pareto efficiency
    - independece of irrelevant alternatives

    - the only voting proc. satisfying pareto cond. and IAA is dictatorship (one voter selects outcome)


Auctions:
- mechanism for resource allocation

- seller: maximizes price
- buyer: minmizes price, has its own utility function

- protocols: first price x second price, open x sealed, one x more rounds

- First-price sealed bid
    - one round, closed offers
    - highest wins, pays its price

    - market price not estimated, others preferences not estimated
    - dominant strategy is to go epsilon below utility

- Vickrey
    - sealed bid, one round, second price
    - dominant strategy is to offer true value
    - AdWords, stamps, ...

- English
    - open cry, increasing price, first price
    - most common, buyers usually overshoot the price
    - dominant strategy: increase epsilons until utility

- Dutch:
    - open cry, decreasing price, first price
    - used for perishable items (flowers, fish)

    - not possible to estimate market price / buyers prereferences

    - dominant strategy: wait until utility minus epsilon is reached

- other posibilities
    - paying all the bids (lobbing and bribes research, ...), combinatorial (more items, subsets)
    - amsterdam: start english, when two buyers remain switch to Dutch with double the price
    - Tokyo: offers at once, conflicts by scissors-stone-paper

