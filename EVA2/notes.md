General EVA scheme:
- Init
    - Create population
    - Evaluate
- Repeat
    - Parent selection
    - Recombination
    - Mutations
    - Evaluate new individials
    - Environmental selection

Parameter tuning: 
- static tuning
- change them via some strategy
    - fixed: time dependent, stochastic (simmulated annealing, ...)
    - adaptive: based on current quality of solution
    - self-adaptive: it's part of the evolution

- adaptive solutions:
    - precision of binary encoded real number individuals
        - when diversity goes bad save best individual, run new evolution with more bits      
    - changing mutation parameters: evo strategies
        - std-dev o guassian: increase if succ. rate of mutation > 1/5, dec if < 1/5
        - directly encode the parameters to genom, let them evolve normally (self-adaptence)
    - selection: 
        - Boltyman: p that worse can win ~ exp(ratio of fitness diff & temperature (annealing))
    - population:
        - life span of individuals proportional to their fitness

    
SGA: Simple genetic algs: 
- start with random bin population
- compute fitness
- repeat until new population is filled: 
    - select 2 indivs
    - crossover with pc, discard one of them (the simplific.)
    - mutate each bit with pm
    - insert new (one) into new population
- repeat until finished

SGA formalization: 
- each binary string is represented by a number 0...2^l
- population is represented by
    - vectors p(t) and s(t) of lenght 2^l
    - pi(t): part of population t occupied by a certain string
    - si(t): probability of selecting string i (already contains fitness & popul. info)
- capital G operator
    - F: fitness matrix with f(i) on diagonal 
    - s(t): F * p(t) / (SUM(Fj,j)*pj(t)): proportion of overall fitness given population distr.
    - Looking for G: operator that realizes SGA operation, G=F°M (fitness ° mutation)
    - F:
    - E(p(t+1)) = s(t) & s(t+1) ~ F p(t+1) -> E(s(t+1)) ~ F s(t)
        - for small (finite) populations statistical errors can cause deviations
    - M (crossover and mutation operator)
    - r(i, j, k): p that offspring k is crated from parents i, j
    - E(pk(t+1)) = SUMi, SUMj si(t) sj(t) r(i,j,k)
- SGA works by applying G to each popoluation in time -> dynamic system
- we could try to find fixed points 
    - fixed points f: populations with identical fitness
    - stable fixed points: maximal identical fitness
    - fixed of M: identical probabilities s

    -> combination of shaking by M and focusing by F -> punctuated equilibria
    - composition of F and M into G is still open problem

Finite SGAs: markov chains
- each state one population, n+2^l-1 over 2^l-1 number of populations of n indivs, l length (comb. with replacement)
- matrix Z(y, i): number of individuals y in population i
- matrix Q: transition matrix between states, NxN -> can be derived but too large to be used

- too large matrixes to actually use, theoretical results regarding convergence, ...
- in limit correspondence between infinite and finite SGAs

Evolutionary programming: EP
- goal to evolve artificial intelligence
- agent in environment, predicts its states, derives suitable action, represented as finite automaton

- EP over Finite automata:
- population of finite automata (genotype = phenotype for this), environment sequence of letters
- inputs presented to automaton, outputs compared to next symbol in seq, fitness is prediction acc

- mutation:
    - change output symbol
    - change successor state
    - add / remove a state
    - change initial state

- no crossover
- half population replaced by new automata
- fitness penalization for large automata

- example primes prediction (predicting which of 1:n) numbers is prime: 
    - penalization for a lot of states
    - iteratively trying to learn larger sequences 
    - with > n simpler and simpler automata, ultimately being 1-state predictings 0 all the time


- EP today
    - benvolent to encoding of individuals: problem specific, genotype=phenotype
    - collection of smart mutations tailored to specific problem
    - no crossover / very limited
    - parental selection: every parent is selected once 
    - enviromental selection: tournament between parents and offspring

    - meta-evolution: see above ~ mutating parameters within the evolution itself

EP vs GA:
- idea behing crossover: exchange of building blocks
- in practise: strange big random mutation, sometimes hard to respect encoding 

- Jones 1995 experiment:
    - normal crossover vs. crossover with random string
    - in cases with _clear idea_ of building blocks crossover is good
    - in cases without that random crossover is better (better macro-mutation)

    - no clear building blocks: wrong encodding, wrong crossover, ?

Genetic programming: evolution of programs
- normal genetic algh structure

- Tree based GPs: programs are syntactic trees
    - Terminals: variables and constants
    - Non-terminals: operations
    - Crossover: subtree exchange 
    - Mutation: replacement of a subtree with a random one
    - Fitness: determined by running the program


- Mutations:
    - necessary to have more mutation types
        - e.g. specialized mutation to fine-tune numeric values for variables (e.g. local search, ...)
        - swapping terminal -> non-terminal
        - exchange of a node with a same-arity random node

- Different types of inits:
    - create random tree until certain number of nodes (grow) / depth (full)

ADF: Automatically defined functions
- evolve subprograms that can be composed 
- program has main() and one or more ADF subtrees
- evolution doesn't mix main and ADF subtrees

- specilized code constructs for ADF (loops, iterations, ...)
- alternative approach: co-evolution of population of mains and ADFs

GP:
- GP programs have tendecies to grow in size
    - limit size, penalize it in terms of fitness
    - operators that make the individuals smaller

Linear GP:
- program is prepresented as sequence of instructions
- fast to emulate run, simple operationn
- high risk of creating nonsense programs by operations
- used in artificial life, code control in games, ...

Graph GP:
- program is general graph (usually DAG)
- extension of tree GP
- used for a lods of things
    - evolution of circuits, finite automata, NN, ...
- complicated genetic operators, especially crossover 

Neuroevolution: 
- first attepts late 1980: learn everything (parameters, topology, hyperparameters, ...)
- useful for reinforcement learning

- learning weights: 
    - normal floating point GA
    - slower than specialized gradient descent via backprop
    - can be parallelized heavily, used for reinforcement tasks

- learning architecture:
    - fitness: build architecture, train (BP / EA, doesn't matter), see accuracy
    - possible encodings:
        - direct: binary matrix of edges
        - grammatical encoding: represent mattrix via 2D grammars that are evolved, proved to be too indirect

    - simmulated growth of network of connections: not scalable, attepts for evo. robotics
    - Celuar encoding: GP is a program how to grow a network via ops: add neuron, split, swap synapse, ...

NEAT
- K. Stanley 2002 = neuroevolution of augmenting topologies
- NN is represented as list of edges (each remembers its vertices, global edge id)

- crossover edges only with the same ID
    - crossover of edges with the same evolutionary origin, does not mess the overall structure

- similarity measure on vectors of edgeIDs: how much do two networks differ
    - niching: similar networks are of the same species: fitness relative to species
    -> solves problem that structure changes are required for explor. but disrupt fitness
    -> protects newly created topologies

-> HYPERNEAT

Memetic algs:
- meme: and idea, thought, that survives in human society ~ bio evolution without DNA
- using memetic concepts for meta-heuristics in EA

- using (local) cultural search within the EA evolution 
- Hill climbing, Simmulated annealing, gradient search, ...

- How to handle result:
    - lamarckism: keep the result of the local search -> local search changes genotype
    - baldwinism: use only fitness of the locally searched better individual ~ it's potential -> doesn't change genotype

Dynamic fitness landscapes:
- representation of fitness dependence on genotype, ...
- theoretical tool to explore EAs, illustration of relationship between genotype/fitness

Changing fitness:
- the environment doesn't have to be static -> fitness for certain genotypes can change
- simple solution: restart the whole GA <> not ideal ~ a lot of offline time

Classical GA with dynamic fitness:
- as GA converges we're losing diversity
- with dynamic fitness we want generality and the possibility to change

- GA strategies are generally better than ES for dynamic f.l.

Modifications of EA for dynamic f.l:
- diploid (two sets of chromozones) representation can an work as long-term memory
    - most EAs work with haploid representation

- f.t. detection based methods: if average fitness in population goes down (-> possible f.l change)
    - hypermutation: increase mutation rate
    - random migration: add new random individuals to population 

- comma ES is more prone to landscape changes, use plus strategies instead
    - choosing from old+new and not only new

- decrease selection pressure
- crowding, niching (see above)
- sub-populations: run multiple EAs parallel
- dynamic species: using tag-bits for different species, mating only within species
    - explicit niching~ish

- fitness changes rapidly -> no solution might help (no free lunch theorem)

Biologically accurate EA:
- even small selection pressure leads to vanishing diversity of population
- introduction of species / non-random mating might save diversity

Goals:
- better reaction to changes in fitness
- paralllel exploration of search space

Niching:
- creating parallel sub-populations, exploring & converging in different areas
- recombination is limited to similar genomes only
- fitness is relative for the niche, tournaments within certain niche

- problems:
    - computing similarity, sensitive to parameter setting
    - treshold on similarity can be static / dynamic (meta evolution?)

Non-random mating:
- general idea that mating is allowed only for close individuals
- necessary to define topology for individuals

- island: sub-populations in parallel
- niching: some sort of distance
- species: defined by an explicit info in genome

Co-evolution
- evolving multiple species at the same time
- hard to assess individual's fitness if it depends on others in population / other species


- e.g.:
    - evolution for multiple strategies (coop / adversarial)
    - sorting networks and evil number sequences ~ analogic to GAN idea 

Several search algos:

- taboo search + local search:
    - remembers a taboo set -> uses it to filter next possible locations
    
    - short-time memory: list of recent previously visited locations, cyclic buffer
    - mid-term memory: rules guiding search towards a space with good chance of finding sol.
    - long-term memory: diversification rules pointing to new non-explored areas

- scatter search: focus on diversity of individuals 
    - generate population, select reference set R(0) as subset of P(0)

    - generate new candidate solutions P(t) by crossover from R(t-1)
    - apply local changes to P(t)
    - Update R(t) by some (diverse) members of P(t)

    - Updating R(t) from P(t): combination of fitness & diversity within R

- differential evolution:
    - mutation is shift based on other individuals
    - performs mutation only if the newly created member is better

    - handles deformed search-space better -> mutations move population in the direction of s.s
    


