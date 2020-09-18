- [Objektové koncepty moderních jazyků.](#objektové-koncepty-moderních-jazyků)
  - [TLDR](#tldr)
- [Koncepty jazyků bez tříd](#koncepty-jazyků-bez-tříd)
- [Skriptovací jazyky, prototype-based jazyky](#skriptovací-jazyky-prototype-based-jazyky)
- [Domain Specific Languages](#domain-specific-languages)
  - [TLDR](#tldr-1)
- [Generické programování a metaprogramování, generika a šablony, politiky, traits, typová dedukce, reflexe](#generické-programování-a-metaprogramování-generika-a-šablony-politiky-traits-typová-dedukce-reflexe)
  - [TLDR](#tldr-2)
- [Odkazy na objekty a jejich životnost.](#odkazy-na-objekty-a-jejich-životnost)
  - [TLDR](#tldr-3)
- [Moderní konstrukce programovacích jazyků.](#moderní-konstrukce-programovacích-jazyků)
- [Pokročilé aspekty imperativních jazyků.](#pokročilé-aspekty-imperativních-jazyků)
  - [TLDR aspects](#tldr-aspects)
  - [Aspect Oriented programming](#aspect-oriented-programming)
- [Výjimky a bezpečné programování v prostředí s výjimkami](#výjimky-a-bezpečné-programování-v-prostředí-s-výjimkami)
  - [TLDR](#tldr-4)
- [Implementace objektových vlastností, běhová podpora, volací konvence, garbage collection.](#implementace-objektových-vlastností-běhová-podpora-volací-konvence-garbage-collection)
- [Vliv moderních konstrukcí na výkonnost kódu.](#vliv-moderních-konstrukcí-na-výkonnost-kódu)
- [Paralelní programování, Amdahlův zákon, synchronizační primitiva, task stealing](#paralelní-programování-amdahlův-zákon-synchronizační-primitiva-task-stealing)
- [Functional programming](#functional-programming)
- [Návrhové vzory a jejich využití.](#návrhové-vzory-a-jejich-využití)
- [Principy tvorby kvalitního kódu, doporučené postupy.](#principy-tvorby-kvalitního-kódu-doporučené-postupy)
- [Refaktorizace](#refaktorizace)
- [Testování funkčnosti, hledání chyb, monitorování programů.](#testování-funkčnosti-hledání-chyb-monitorování-programů)

Resources:
[NPRG059 Praktikum z pokročilého objektového programování](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NPRG059)
[NPRG014 Koncepty moderních programovacích jazyků](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NPRG014)
[Navrhove vzory](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NPRG024)
[Doporucene postupy v programovani](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NPRG043)

## Objektové koncepty moderních jazyků.

### TLDR
- Object
  - Classless/classfull
- Goal
  - Bind behavior to function
- Value/reference type
- OOP
  - Polymorphism
  - Dynamic Dispatch
  - Encapsulation
- Strongly/weakly typed
- Traits
- Reflexion
- Anonymous objects
- Functional
- Attributes/Aspects
- Invokation by message sending (smaltalk)

**Klasické**

- V některých jazycích je všechno objekt (RUBY), v jiných existují primitivní datové typy
  - value type vs reference type (obecny koncept)
  - Na primitivnich nejdou volat metody
  - boxing-unboxing
- Access modifiers (zapouzdreni)
- Dynamic dispatch 
  - pokud podedim klasu, tak nevim kterou metodu zavolat (polymorfismus)
  - neplest s multiple dispatch - to je na overridy

- Objekt je state+behavior
  - https://eev.ee/blog/2013/03/03/the-controller-pattern-is-awful-and-other-oo-heresy/
  - state bind to behavior by the parameter `self` or `this`
- Cíl: Easy to implement highly granular programs
- Strongly/weakly typované
- Classful Třídy se tvoří instanciací a inicializací
- Classless ... viz prototybe based

Moderní
- Dogenerovani kodu v compile time
  - serializace pri startu se nacte pomoci reflexe typ jen jednou
  - 
- Funkcionalni smer
  - First class citizen metody
- Anonymni objekty
- anotace(atributy) 
- reflexe
- změnit delegáta metody
- traity 
  - does not introduce hiearachy
  - overidable abstract classes
  - traits extends traits 
  - metoda traitu muze volat metodu implementatora 
- defaultni metody interfacu
  - zavedlo se kvuli zpetne kompatibilite
- aspect oriented
- immutable classy (copy on write)
- reactive programming

[1](https://en.wikipedia.org/wiki/Object-oriented_programming)
[2](https://groovy-lang.org/objectorientation.html)

## Koncepty jazyků bez tříd
and 
## Skriptovací jazyky, prototype-based jazyky
and
## Domain Specific Languages

### TLDR 
- object is bag of variables and functions
  - created new or cloned
- has a prototype
  - differential inheritacne
- script
  - api cally
  - read eval write
  - glue languages
- dls
  - domain specific
  - internal external

Object is a set of slots (IO) or a table (JS)
It responds to messages
Object has list of prototypes (IO) or one prototype (JS)

In JS, when using `new` the function is considered to be consturctor
```js
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

const car1 = new Car('Eagle', 'Talon TSi', 1993);
```
or
```js
var o = {a: 1};
```
or 
```js
var o = {a: 1};
```

Differential inheritance 
- Object contains only difference to its prototype
Základem je prototyp
- Objekty jsou jeho kopie
  - prototyp ví jak se naklonovat
  - Inheritence is done by reusing
- more general than objects
- does not require definition in advance (defined on runtime)

Upon invocation
- paramters
- sender
- target
- message
- and self

In IO - while, if and other control flow commands are also methods

- Multiple dispatch + flexible delegation mechanism
- self or this is could be bind to the "current prototype" and furthermore it could trace all other chained prototypes or parents
  - e.g. the delegate could be different to the one "holding" the method

- In Groovy, there are `owner`, `this` and `delegate` keywords

- if a method is called on a child object (which usually has only pointer to the method which resides on the prototype) the delegation chain created upon creation is followed

[1](https://en.wikipedia.org/wiki/Prototype-based_programming)
[2](https://groovy-lang.org/closures.html#closure-owner)

script langs 
- sometimes reffers to high-level dynamic GPL
- DSL
- Made for READ-EVAL-PRINT purpose
- Declarative programming style
- Interpreted
- primitiva jazkyka jsou casto api cally (bash)
- interpreted
  - platform independent
  - dynamic typing
  - less reliable (no compile time type checks)
  - easy to steal
  - less secure
- Usually abstract (declarative)
- easier to embedd into something else (OS, Web Browser)
- DSL or GPL
- glue languages (cmd running script doing something with two programs, python using multiple multi-lang programs)...

[DSL](http://docs.groovy-lang.org/docs/latest/html/documentation/core-domain-specific-languages.html)
- Not general purpose
- SQL, regex, css, html
- Internal DSL - hosted in languages (using fluent api)
  - written in particular style of host language
  - string format, regexps
- external 
  - (markups) latex, markdown -> requires parser in order to exchange data with some other tool
  - modelling tools
  - awk
- The value
  - business readable (rather than writable)
  - can be modified by domain experts (sql by databasists, css by graphics)
  - limited scope makes them easier to learn
  - focused on one thing which it does well
- Cons
  - usually open source
  - lack of documentation is common
  - using DSL can get out of hand -> fallback to General Purpose Language (GPL)
  - less adopted than GPL -> harder to do -> more expensive to pay for experts
- used from commandline or from hosted code or makefile
- In GPL languages, there is typically some support
- in groovy
  - various datatype builders (DOM, HTML, json, ANT, cli)
  - custom type checking, compilation
  - delegateTo (Closure has concrete type)
  - ExpandoMetaclass

## Generické programování a metaprogramování, generika a šablony, politiky, traits, typová dedukce, reflexe

### TLDR
- Generics and templates
  - type specified later
  - cpp and c# are different
- traits
  - chovani co se dokopiruje do classy, neni interface (ma implementaci), muze volat property base classy pres interface
- type inference
  - in compile time
  - based on type hierarchy
  - duck typing
- reflexe
  - sebe pozorujici se kod


- Metaprogramming
  - Programs can treat other programs as their data
  - reading, generating, analyzing
  - allows user to move stuff from run time to compile time
  - in .net we expose internals of IL compiler
  - code that write more code
- In groovy - powerfull AST changes, a lot of attribtues
- Runtime
  - POJO - Java objekt
  - POGO - Groovy objekt
  - Interceptor
  - base class with overridable methods (invokeMethod, getProperty)
  - Interceptor says that all calls are intercepted
  - Category enables extension methods
- Compile time
  - AST - abstract syntax tree (does not contain parentheses and such)
  - Modify AST to generate bytecode
  - Local (annotation) or global
    - ToString (generate toString producing human readable object representation)
    - hashAndEqual (generates getHashCode and equals methods)
    - ToupleConstructor - reduces boilerplate code
    - Immutable ...
    - Logging
    - Script transformation stuff (@Field)
- Generika
  - "types to be specified later" objects
  - kovariance, kontravariance, invariance 
  - in cpp templates will fail on compile error (max(c_1,c_2) where c_i is complex number)
  - generics are runtime, can't call operators (+)
- Type inference - Strongly typed only
  - heuristics and walking the type tree
  - duck typing
    - Func<> nebo action<>
    - objekty co maji stejne metody
- Reflection 
  - ability to process own structures
  - helps serialization
  - code raping - create instances of stuff that should not be instantiated
  - read private properites
  - Java - neni v metadatech (type erasure)
- traits
  - late binding
  - Trait dodava chovani do te tridy
- reflexe 

Covariance
- `out`
- You can use instance of `IEnumerable<Derived>` to a variable of `IEnumerable<Base>`

Contravariance
- `in`
- You can assign `Action<Base>` to `Action<Derived>`

Invariance
- You use only the type specified

## Odkazy na objekty a jejich životnost.

### TLDR

- managed/unmanaged
- garbage collertor
  - process clearning dead references
    - tracing
      - reachable form a root
      - mark all and sweep unmarked
    - reference counting
  - generations of objects

- depends on language
  - managed (GC collected) vs unmanaged
- unmanaged - cpp, either malloc, new or unique<>, shared ptr classes
- class is a blueprint
- block of memory on heap is allocated
- reference is on stack 
- monitored by GC - background running cleaner
- destroyed after not used
- three generations (managed and allocated by GC)
  - gen0 (more frequently removed) initial for small objects
  - gen1, gen2 (hosts all that survived through previous cleanups)
  - [LOH](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/large-object-heap) 
    - large objects (85KiB) immediately allocated here
    - cleared when gen2 is cleaned
    - is not compacted by GC, only by user or when GC can't get memory from OS
    - allocation here costs a lot of resources
      - allocation/cleaning
- v managovanych jazycich
  - used to release unmanaged resources (connection, opened files etc.)
- pros - reduces bugs
  - dangling pointer
  - double free memory
  - memory leak
- cons 
  - resource demanding
  - unpreddictable stalls (freeing and shifting memory)
- strategies 
  - tracing - finds object that are not tracable from some root object
  - reference counting - object has a number of referencing objects
    - gets deleted when the number is zero
    - cons
      - circular pointers
- after cleanup - the memory may get compacted
  - compacted - when all object are moved to new area, previous block is deleted  
  - noncompacted - one by one object is marked deleted
- heap fragmentation
  - LOH is not compacted


## Moderní konstrukce programovacích jazyků. 

- Traity
- Attributy
- Extension methody
- async operations
- multiplatformni (pc, mobily ...)
- multiparadigm (scriptovaci i OOP - python)
- DSL (vlastni operatory a support pro custom logiku - matice a Infrastuct)
- Goal:
  - reduce boilerplate code - > nesrat se zbytecnym kode
  - make it easier to add stuff and not brake the current one
- Type inference 
- 
## Pokročilé aspekty imperativních jazyků. 

### TLDR aspects
  - Cross cutting concern
    - concern is a encapsulated behavior
    - advice is applied to point cuts by weaving it into the code

https://en.wikipedia.org/wiki/Imperative_programming
- Specify how program works, not what should be one
- Control flows
  - usually fixed order
  - manaull loops - you can loop as much as you want to
  - conditional branching 
  - changes states manually
- ... just what ever you want
- pros whathever you want... good for control freaks
- cons tedious and contain log of work
- vektorizace instrukci (kontrola nad tim co se da vektorizovat)
- thready

Execution model - specifies the behavior of the elements of the language
- example
  - map reduce
  - c statements
  - Posix thread
- static choices are made here, dynamic in runtime system

context switchin v ramci procesu je rychlejsi nez mimo nej
preemtive
processy maji oddeleny adresni prostor

### Aspect Oriented programming

- Addind additional behavior with custom code (advice ) without changing actual code
- Allows easily adding code that is not business logic
- Concern - encapsulate functionality
- Some of the concerns cut across the application
  - Logging
  - Security
  - Transactions
- They are captured in separate module called aspecn
- Applied to join-points
  - Method execution
  - Field reference
- Incluced in the code through weaving
  - The code of the aspect is put into the source code in compilation time (metaprogramming) or upon class loading
  - 

## Výjimky a bezpečné programování v prostředí s výjimkami
  
  ### TLDR
  
  - Disrupts normal flow of the program
  - triggers exception handler
  - if unhandled-applicatoin stop
  - try catch finally (usually)
  - multiple catch blocks + filters
  - Unchecked - catch (Exc)
  - Checked (java)
    - rutime vs checked  
    - Forced exceptioon handling (nemuzes na ni zapomenout)
      - bud trycatch nebo ji das do signatury
      - invalid op je exception
    - the ones checked at compile time
    - Breaking client code(versioning)
  - stack trace - retrhowing and wrapping
 
## Implementace objektových vlastností, běhová podpora, volací konvence, garbage collection. 

Objekt ma hlavicku
- typ
- Tabulka virtualnich metod (dispatch)
  - used for dispatch
- policko pro lock

new object
- upon new in classful
- upon clone in classless
- size of the class is detemnided
- memory allocated
- superclass intit code called 
- the class init code
- Make sure the constructor is exception free
- Method binding
  - Early 
    - On complie time
    - faster
  - Late 
    - in runtime
    - slower - requires some lookups

[Runtime system](https://en.wikipedia.org/wiki/Runtime_system)
- execution model
  - it gets a program and runs it
  - 
- cooperates with OS
  - handles memory
- this is what is responsible for dynamic type checking
- passes parameters from one method to another
- threading
Behova podpora
- ETW events
  - GC started
  - Method invoked
- debugging

[Volaci konvence](https://en.wikipedia.org/wiki/Calling_convention)
- kde se nachazi apramametry (stack, registry)
- poradi parametry
- how to deliver return value (stack, registry, heap)
- caller or callee, who is cleaning after function
- ex
  - x86
    - 4 params are in registry, stack used for passing other arguments (low number of registers)
    - return value is a pointer in the regsitry
  - dulezite vede pro interoperabilitu jazyku (c a c#)
- Messaging
  - Nekter jazyky nemaji procedure/function cally, nepredavaji si parametry pomocí stacku, ale komuniují pomocí posílání messages objektům a očekávají od nich, že je zpracují
  - receiver, selector, arguments `2 raisedTo: 4` (smalltalk)

Garbage Collection [here](#odkazy-na-objekty-a-jejich-životnost)

## Vliv moderních konstrukcí na výkonnost kódu. 

and 

## Paralelní programování, Amdahlův zákon, synchronizační primitiva, task stealing

MONITOR a pulsy
Task stealing
- multiple double ended queues
- fork-join 

Ahmdal law
- paralelizace ovlivnuje jenom paralelizovatelny kod
- speedup = $\frac{1}{1-p}$

MAP reduce

- amdhal - umera poctu jader ke zlepseni vykonu proporcionalne k mnozstiv paralelizovatelne casti prace
- TOHLE neni v karolince viz `~/Downloads/or_k1819.pdf`
- locky, semaphores, mutex, rw locks


- zabiji vykon
  - alokace (stack aloc, ne na heap)
  - kontext switching (task a async, jeden thread a neswitchuje lowlevel)
  - thread raping (idle, async await)
- lock free, wait free
- treba type inference je narocnejsi pro kompilator, ale kod to neovlivni

- CPU paralelism
- OS paralelism - ten ma svoje thready... mapuje je na CPU. resi mapovani na CPU ... thread scheduling
- Na urovni c# - THREAD - wrapper nad tim fyzickym.
- tasks
  - shared object sucks
  - paralelism is not hard - multithreading is
  - DSL for pipelines 

## Functional programming

- function composition
- declarative - tree of functions rather then sequence
- pure functions - does not have state -> exclude OOP
- functions are first class citizens
- based on lambda calculus (which is turing complete)
- higher order functions - functions that return functions or can take them as parameters
- currying (only some parameters are provided)
- recursion
- strict vs nonstrict evaluation (lazy/ not lazy)
  - Lazy languages - graph reduction
- much nicer / slower performencewise
- can be done in non-functional programming (through DSL)
- Closures
  - can contain free variables from outside of the body
- in groovy - fluent api / extension methods
- delegate - setting owner of the closure from outside (good for building DSL)
- .memoize() cache results


## Návrhové vzory a jejich využití. 

- Creational
  - abstract factory
  - factory method
  - builder
  - prototype
  - singleton
- Structural
  - adapter
  - bridge
  - composite
  - decorator (moca or the special bikes)
  - fasade
  - flyweight
  - proxy
- Behavioral
  - chain or responsibility
  - command
  - interpreter
  - iterator
  - mediator
  - memento(undo) - used so two classes does not communicate
  - observer (weather station)
  - state
  - strategy (the duck from the materials, each aspect is represented by different class)
  - template method
  - visitor
    - accept(visitor) => visitor.visit(this)



## Principy tvorby kvalitního kódu, doporučené postupy. 

API
- users vs programmers
- defines border/interface/contract among components
- should be perfectly documented
  - can be documented via unit test (input,output)
- as simple as possible while having all common functionality (hard problem)
- Hard to change
  - backwards compatibility or a separate version
  - should never break (hence the contract)


Class design
- SOLID
  - Single responsiobility principle
    - Class should do one thing expressed in its name
    - Doing two things is a sign of a bad design
  - Open for extension, closed for modification
    - When you want to add something, do it by extending, never modify it.
    - Dependency injection
    - Inheritance, traits ...
  - Substitution principle
    - Subtypes must be substitutable for their base types
  - Interface segregation 
    - users should not be forced to implement interface they dont use
    - avoid fat interfaces
  - Dependency inversion 
    - high level modules should not depend on low level ones
- Favor composition over inheritance
  - "`is a` vs `has a`
  - inheritance is misused to reduce code -> can be done through traits
- Use factory methods over constructors
  - Factory mehtods can be named (constructor can't)
  - Nobody reads/maintains documentation
- SPI is used to extend and implement while API is used to call stuff
- Fail fast methods
  - check input arguments
  - check return values
  - offensive programming
- Use idioms of platform and the language
- skryt implementacni detaily
- it is easy to change something which does not break assumption
- defensive programming
  - garbage in - nothing out

Documentation
- Should contain what does it do
  - Concepts
  - Goals
  - Api
- How should not desribe code
  - The code is documented in the code
- Generated 

## Refaktorizace
- Factoring (decomposition)
- Restructualization of existing code
  - Improve design while preserving functionality
  - Improve readability
  - Reduce complexity
  - Should be done to optimize stuff 
    - to avoid doing while prototyping - premature optimization
- May be motivated by static code analysis or when we need to add new feature
- Simple in strongly typed languages. Realy hard in weakly or not typed.
- IDE offers multiple tools
  - Changing names
  - Extracting methods
  - Find duplicate code
- Challenges
  - Intra application dependencies
  - Improperly executed refactoring and renaming may break API

Goals
- Coherence - Things that are related should be together (concern)
- Avoid values with opaque semantics (-1 = infinity in waiting loop)
- Function is a named piece of code (reusable)
- Avoid changing function parameters or reusing variables
- ...

## Testování funkčnosti, hledání chyb, monitorování programů.
- always automatic, simple execution (oneliner), not much config (preferably none)
- unit, component, integration, system (in its final form with final config)
  - unit (bottom up)
    - mocks, contracts, all possible execution paths and their results (limited to the unit)
- regress, performence, usability
- blackbox
  - TTD
  - specification based
- whitebox
  - created after the subject code was written
- good testing - good design
- good tests - documentation (input - output assertion)
- code coverage
- test is also code

Static code analysis
- Input is the code, not runing program (hence static)
- Locates code smells
- Some tool focuses only on local pieces, some on the whole context
  - Unit Level
    - Linq multiple iteration
    - Invert if to reduce nesting
  - Technology Level
    - On android, there may be different problems than on desktops (permissions)
  - System, Business ...
  - Undecidable problem
- Model checking

Code review, pair programming
- ...

Monitoring
- Development monitoring
  - Manage programmers work
  - Manage bugs
- Debuging tools
  - Breakpoints
  - Inspect values of variables
- Event Logging
  - Appinsights
  - Exceptions
- Profiling
  - Resource monitoring