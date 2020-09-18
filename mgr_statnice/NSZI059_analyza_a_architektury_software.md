- [Procesy vývoje SW a jejich fáze.](#procesy-vývoje-sw-a-jejich-fáze)
- [Podnikové procesy a jejich modelování pomocí BPMN.](#podnikové-procesy-a-jejich-modelování-pomocí-bpmn)
- [UML a jeho využití pro analýzu a návrh struktury a chování SW.](#uml-a-jeho-využití-pro-analýzu-a-návrh-struktury-a-chování-sw)
- [Formální základy jazyka UML.](#formální-základy-jazyka-uml)
- [OCL jako specifikační jazyk a formální základy dle specifikace.](#ocl-jako-specifikační-jazyk-a-formální-základy-dle-specifikace)
  - [TLDR](#tldr)
- [Testování SW, dopadová a změnová analýza.](#testování-sw-dopadová-a-změnová-analýza)
- [Plánování SW projektů, odhad nákladů, úrovně řízení projektů.](#plánování-sw-projektů-odhad-nákladů-úrovně-řízení-projektů)
- [Právní aspekty SW, hlavní zákony důležité pro IT projekty.](#právní-aspekty-sw-hlavní-zákony-důležité-pro-it-projekty)
  - [TLDR](#tldr-1)
- [Typy pohledů na SW architekturu.](#typy-pohledů-na-sw-architekturu)
- [Modelování a dokumentace SW architektury.](#modelování-a-dokumentace-sw-architektury)
- [Klasifikace atributů kvality SW architektury, jejich popis pomocí scénářů a taktik.](#klasifikace-atributů-kvality-sw-architektury-jejich-popis-pomocí-scénářů-a-taktik)
- [Servisně orientované architektury.](#servisně-orientované-architektury)
- [Algebraické metody, vícedruhové algebry, iniciální modely.](#algebraické-metody-vícedruhové-algebry-iniciální-modely)
  - [TLDR](#tldr-2)
- [Formální základy RDF a jazyka OWL, deskripční logika.](#formální-základy-rdf-a-jazyka-owl-deskripční-logika)

Resources:

[Architektura SW systémů](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NSWI130)
[nebo](https://drive.google.com/drive/folders/16-yIBa0NhFG4iUXnMsJqFNczub8N8hC5)


[Pokročilé aspekty softwarového inženýrství](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NSWI026)

[Formální základy softwarového inženýrství](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NTIN043)


## Procesy vývoje SW a jejich fáze. 

Začátek
- zakázka poptaná přímo
  - získáváme povědomí o tom co user vlastně chce (iterativní proces)
- výběrové řízení
  - zpracování studie proveditelnosti
- vlastní nápad
  - push - tlačímě něco zákazníkům
    - nadefinujeme o co se jedná a to pak prodáváme (nemusí to ještě existovat)
  - dělame něco pro co si někdo přijde
    - MVP a jdem na trh

Vývoj
- Vodopád
  - máme specifikaci - analyzujeme jí
  - navrheme řešení a alternativy 
  - po přijetí implementujeme
  - testujeme 
  - dokumentujeme 
  - nasadíme 
- Agile 
  - ...


## Podnikové procesy a jejich modelování pomocí BPMN. 

- TLDR
  - Fancy Activity diagram
  - Events (kolecka), Activities (obdelnicky), Gateways(diamonds)
  - Sequence flow(spojena cara), Message flow (carkovana)

Procesní modelování (modelování organizace):
-	Obyčejně: use case diagramy → aktéři a jejich potřeby → scénáře:
  -	Scénář = sekvence kroků, kde máme hlavní, příp. i alternativní flow (cesty).

BPMN (Business Process Modelling Notation) 2.0:
- přímo pro modelování byznys procesů, které mohou zahrnovat některé scénáře.


## UML a jeho využití pro analýzu a návrh struktury a chování SW. 
and
## Formální základy jazyka UML.
and
## OCL jako specifikační jazyk a formální základy dle specifikace. 

### TLDR

- UML
  - Requirement
    - Whats and whys - not hows
  - Actual architecturea and behaviro
    - used in the documentation

[Uml for modelling requirements (Necasky)](https://drive.google.com/drive/folders/1VSYpdEJW2GyFYcM8wxsxdBWROd0kw6Ct)
[OCL and UML formal basics (Necasky)](https://drive.google.com/drive/folders/1k4tC8ZxH0cUokO_m1sEpsFzcJZcvVJVu)


Diagrams 
- Requirements
- Business
- For architecture

Requirements
- User - User x should do y
- System - more detailed system-centric
  - [See Quality](#klasifikace-atributů-kvality-sw-architektury-jejich-popis-pomocí-scénářů-a-taktik)
  - Function
  - Domain
    - Train breaking control system should take into account
      - weather
      - weight
      - ...
    - Difficult for non specialist -> must be documented well

Documentation
- Should be complete
- Should not have contradiction (consistent)
- Is a set of WHATs and WHYs - not HOWs
- Every reader use it for different purposes
  - Managers - planning
  - Engineers - design
  - Test engineers ...

UML (Unified Modeling Language):
- Modelovací jazyk a notace pro navrh SW
  - Strukturální: např. class, component, object, package či deployment diagramy.
  - Behaviorální: např. activity, use case, sequence diagramy.
- Unified
- Used to capture requirements,intent, logic or the design and architecture, business process (BPLM may be more suitable)


Use Case Diagram
- Desribes scenarios (actors and use case they perform)
- Documents all possible interactions with the software
- Use case (bubble)
  - specifies set of behaviors 
  - yields observable result
  - `<<extend>>` - optional extending behavior
    - ON registraion - `<<extend>>` get registration help
  - `<<include>>` - used to denote common behavior
    - remove record `<<include>>` search for record
- This diagram should be accompanied with
  - Starting/End state
  - Possible fuckups
  - And a description of intended use

Activity
- Behavior expressed as Graph
- Nodes
  - Action
  - Object
  - Initial/Final
  - Fork/Join
  - Decision

State Machine
- Good for testing
- State represent a situation in which the system can be
  - Does not change on its own, only by an event
- Transition - directed edge
  - Represents an event
  - Requires a trigger

Sequence 
- Interaction modeled through exchange of messages among multiple actors and systems
- Lifeline
- Arrows are the messages (exchange)

Class Diagram
- Class - kind of object, not instance
  - Attributes
    - Name, DataType, Access Modifier
  - Operations
- Relations/Associations
  - Association classes (a class attached to the relations)
    - Worked around with normal class
  - Types
    - Specialization - arrow with empty (white) head
  -	Aggregation (empty diamond on owner) - the thing is a part but can live on its own
  -	Composition (filled diamond on owner) - something is part of something else 
- Stereotypes

Propety of Class diagram
- Can be both, an attribute or an association end
- Owned by the class
- Used by instances
- Example
  - Person has name, surname, email
- Association can be also specialized

Object diagramy (specialni varianta class diagramu). 
- Modelování konkrétních instancí - bez datových typů, složitých asociací
- Stereotypy:
- Přidání sémantiky, rozšíření diagramu o další standardní koncepty (interface, enum). V podstatě speciální označení objektů nebo asociací.

Component diagram:
- logical structure
- component - black box
- relations
- may ne nested

OCL 
- Extends UML
  - Initial values
  - pre-conditions
  - post-conditions
  - invariants
    - Author of a paper must not be reviewer
    - Book name must be unique
- Strongly typed, functional
- Without OCL, constraints were expressed via note with ``<<invariant>>`` stereotype

Initial Values
```
contextDocument::status
init:DocStatus::New
```

Derive
- some value should always be equal to some expression
```
contextProject::teamSize
derive:self.worker->size()
```

Pre codition/post condition
- Operation won't start if precondition is not true
- Operation not executet properly if postcondition does not hold

```
context Project::publishDocument(d:Document)
body: ...code
pre: self.output->includes(d) and d.status= DocStatus::Finished
post: d.status= DocStatus::Published
```

Invariants
- True before and after operation execution
```
context Project
inv: self.startDate->isBefore(endDate)
```

Linq-like syntax for asserting collections
```
contextPerson
inv: self.authoredDoc->forAll(d|self.project.output->includes(d))
```

## Testování SW, dopadová a změnová analýza. 

- Odhaleni chyb => prevence chyb v produkci
- Coding - unit test
- Funkčnost - integracni a system
- Požadavky - User acceptance
- Business case - verifikace
- Aspekty - vykon, funkce, bezpecnost, stress, regrese ...
- funguje to co ma a nefunguje to co nema
- blackbox vs whitebox
- zacit prilis brzo = testovat nedodelany sw, pozde zas malo coasu
- Reportovat vysledky
- automatizace a opakovatelnost
- testy dokumentujeme... 
- pomahaji to bootovani lidi a slouzi jako api dokumentace
- pokryvame requirementy

Dopadova analyza
- nasledky preruseni cinnosti
- penale 
- nastvany zakaznik

Jak resime chyby
- pager a 24/7 ops
- next business day
...

## Plánování SW projektů, odhad nákladů, úrovně řízení projektů. 

Planovani 

zaklada se na pozadavcich
- specifikace
- urcuje rozsah
- podle ni se validuje
- meli by na ni spolupracovat stakeholderi
- obrazky (BPML)
- profily uzivatelu
- slouzi k tomu, abychom zakaznikovi vysvetlili proc neco neni

odhady
- zakomponovat historii a zkusenost (vyzaduje mereni v soucasnem projektu)
- jasne se vymezit, co budeme delat
- risk faktor
- v zavislosti co po nas chteji (ops, poc, gui, tooling) a co vi (co nebudeme zjistovat my behem vyvoje)
- 


## Právní aspekty SW, hlavní zákony důležité pro IT projekty. 

### TLDR
- Autorske pravo
- Patenty
- Open source - non-profit is not opensource
- License
  - BSD
  - GNU3
  - Appach - list all changes
  - MIT
- You are buying the right to use
- You can resell used software

- autorske pravo (literarni a jine umelecke dilo, vedecke dilo, programy)
  - vznika vytvorenim toho dila
  - vyjímka z jedinecnosti (programy a databaze)
    - neni  
      - napad pouze zpracovani
- prumyslova (patenty, ochrane znamky atd...)
  - nutno registrovat
  - platna v určitých zemích

- dilo a hmotny nosic
- virtualizace

- autorske pravo - osobnostni
  - byt uveden
  - bude/nebude zverejneno
  - nedotknutelnost
  - vyjimka
    - citace
    - kopie pro osobni uziti (netyka se SW)
    - svoboda panoramatu
    - kopie orignalu je v pohode

- dekompilace a reverse egineering lze pouze interoperability

smlouva o dilo

sw neni patentovatelny, jen pokud je soucasti sirsiho reseni

- open source
  - copyright owner allows users to get it for free, change and redistribute
  - collaborative and community building
- the licences that allows only non profit distribution are not considered open source
- apache 2.0 and GPL v3 are compatible
  
- copyleft - derived works inherit the license
- MIT is the most permisive
- Apache
  - requires you to list all the changes
  - apache makes you give up patent right
- BSD 
  - also permisive

When you buy software, the ownership is still not yours (EULA)
- prohibits sw engineering
- only one user
- prohibits benchmarking (?)
- 
you can resell used software

## Typy pohledů na SW architekturu. 

Několik vrstev (odspoda)
- SW
- Businees
- Informacni
- Enterprise (vsechny dohromady)

- SW architekutra
  - Viz [Kvalitativni atributy](#klasifikace-atributů-kvality-sw-architektury-jejich-popis-pomocí-scénářů-a-taktik)
  - Design je realizace funkcnich pozadavku
  - Architektura spise zahrnuje ty nefunkcni (funkcni jsou totiz "implementacni detail")
- Business process
  - processy a aktivity, které má system splňovat
- Information system technologs
  - kombinace SW i HW
- Informacni
  - DWH + tooling + processy ... vsechno
- Enterprise - modelovani chodu cele firmz


[UML slouzi k dokumentaci](#uml-a-jeho-využití-pro-analýzu-a-návrh-struktury-a-chování-sw)

## Modelování a dokumentace SW architektury. 

[Use UML](#uml-a-jeho-využití-pro-analýzu-a-návrh-struktury-a-chování-sw)

- metodicka 
- procesni
  - plany, odhady, pozadavky
- produktova
  - uzivatelska
  - systemova
- musi se udrzovat
- hodne obrazyku
- negenerovat z kodu
- konfiguracni rizeni
- verzovani
- kompletni


## Klasifikace atributů kvality SW architektury, jejich popis pomocí scénářů a taktik. 

Definují systémové vlastnosti a omezení
- Typically more important than functional
- Typically affect overall architecture
- Influences other quality attribtues
- To fulfill quality attribute, various functional needs to be implemented

Classification
- Product
  - Usability
  - Testability
  - Performance
  - Scaling 
  - Security
  - Availability
  - Traceability
- Organizational
  - Environment
  - Operations
  - Development
- External
  - Legislation
  - Regulations
  - Ethics


Pitfalls
- Hard to tackle
  - It's hard to express them precisely
  - Imprecise requirements are hard to verify

Example: 
- 27/4 availability (product), 
- users should authentice via card (organizational)
- Staff should be ok after 4hrs of training. (easily verifiable)

Vlastnosti
- Závisí na architektuře
- Jejich vztah k funkčním požadavkům
- Zanedbávané
  - systems are usually redesigned due to not fulfilling quality attributes
    - e.g. slow, hard to maintain or extend ...
- Kvalitativní atributy se neustále ovlivňují
- Musí se ně myslet od začátku

Quality assurance
- uzivatel na neco klikne
  - jak rychle to vyskoci (performance)
  - jak casto to failne (availability)
  - jak snadno se to pouziva (usability)
  - ...

Usability
- easy to learn interface
- UX design rather than architecture
- but redo operation is

Modifiability
- extend or improve usually
- cost and risk
- prevent ripple efect (should be prevented by good design)

Performance 
- how fast the system can respond (or how much data it can process)
- measure = response timestamp - request timestamp
  - latency
  - throughput

Scalability
- handle tasks and keep other quality attributes while scaling up
- scalability cube

Security
...

Interoperability

- Orchestration
- do not breake api

Testability

Avaialbility
- reliability + ability to recover
- fault detection, prevention, tolerance
- Scenario
  - source of stimulus: people, HW
  - stimulus: fault
  - artifact: component or the system
  - response: mask or repair
  - measure: uptime/downtime
- Hearbeaty, monitoring, redundance, rollbacks

## Servisně orientované architektury. 

- sidetrack Microservices
- System decomposed into functional components called services
- Each service is responsible for some part of the business
  - Their combination supports whole business process
- Contract
  - Based on interface
  - SLA, documentation, policies (security... )
- 1 Standardize
  - make it easy to be understand
  - naming conventions and such
- 2 Loose-coupling
  - independent services
  - do not depend on implementation of other services
  - HW, SW dependence
  - Napriklad propertky a jejich hodnoty
- 3 Abstraction
  - publish only the necessary information for consumers
- 4 Reusablity
  - some other programs may use this service... make it general
  - there should not be other service that does the same
- 5 Autonomous
  - it may develop on its own
- 6 Stateless
  - to improve scalablity
- 7 Discoverability
  - consumers may require to consume metadata
- 8 Composability
  - I may be part of other services

## Algebraické metody, vícedruhové algebry, iniciální modely. 

### TLDR
- Used to validate critical software
  - Detect issues
  - Model behavior
- Based on many sorted algabra
  - A = (D-datatypes,F-functions on those datatypes)
- Maude
  - Reasoning tool
  - Rewriting/reducing and checking
    - reducing is done using equations
    - rewriting by rules
  - Substitution
  - Use [example](https://en.wikipedia.org/wiki/Maude_system) with naturan numbers
    - 


Formal verification
- the act of proving or disproving the correctness of intended algorithms with respect to a given formal specification

We have a specification and a model of the software (both expressed mathemtaically) which is exhausitively checked (fully automatic)

Priklad: vytah neotevre dvere kdyz je v pohybu

Temporal logic - rules and symbolism for expressing proposition terms in terms of time  (diagramky jak se meni hodnota promenych)

Used 

Validation - did we do the right thing?
Verification - Did we do what we wanted to do?

Algebra = <Datatypes, Functions>
- Correctness validation

Math techniques supported by math
Good for validation of critical systems
Modeling behavior
Detect issues
Transform into code

Algebraic methods
- based on algerbra
  - carrier set and functions (sorts and datatypes)

Many-sorted algebra
- The set is not homogenous

Things are checked by rewriting
- like haskell
- some are irreducible
- Maude

Used primarily by HW companies (expensive mistakes)
Used by Cisco to build large networks

## Formální základy RDF a jazyka OWL, deskripční logika.

[W3](https://www.w3.org/TR/rdf11-primer/)
[Vojtas](https://www.ksi.mff.cuni.cz/~vojtas/vyuka/y5v/NSWI108/NSWI108LectureSlides.html)

TLDR
- Help machine understand DATA
- Knowledge graph
- Dedicated language and standardization

Web
- Too much information with too little structure
  - Too many types of data
  - Queried only by few keywords or "example"

Semantic web
- Standardized description of information
- We capture the meaning of information by specifying relations and iteractions
  - This is implicity inferred 
  - We don't do that explicitely
- Methods for obtaining this information
  - Deduction: DC is a capital; Every captial is a city; DC is a city

Ontologie 
- Taxonomy
  - classes of things organized into hierarchy
  - Every A is B
  - family relationships 
    - (father - son) -> all fathers are also sons
    - Every publication has an author
- Partonomy
  - A is a part of B
  - classes of things organized into hierarchy of **part of** hierarchy
  - Hand is a part of body
- Defines objects and their relations
- priklad: dva vedecke obory maji svoji jinou ontologii. Aby si rozumeli, tak je zavedeno mapovani mezi pojmy a explicitni vyjimky

RDF
- how do you encode piece of information 
  - "The book FOST is published by CRC Press"
  - We can use XML, but merging it sucks
- Subject - Verb - Object
  - subject The Book FOST (e.g. URI of this book)
  - verb (or the property) isPublishedBy (e.g. Uri where this is defined)
  - object CRC Press (e.g. CRS press uri)
- Data Model
  - Relationships between objects
- Easily machiner readable
- Focused on semantics
- Oriented multigraph
- Allows for reasoning (multiple edges concatenated)

Triplets 
- Literals
- Blank Nodes 
  - used to model n-ary relationships
    - ingredients - what and how many
  - lists




OWL - jazyk pro praci s webovymi ontologiemi
- Postaven na zaklade RDF
- Pridava dalsi vrstvu, nad kterou je mozne delat chytrejsi dotazy

Deskripcni logika - (individual, concept, role)
- Langage for knowlege representation
  - less expressive than First order logic (constant, unary predicate, binary predicate)
- Zaklad pro OWL (individual, class, property)

Linked Open Data
- Connect data from various sources and make them machine readable using URIs ,RDF and HTTP



Challenges
- Vague computer does not understand what "tall" or "young" means
- Vast one thing is reffered to by many terms
- Uncertain for example, when I go to doctor and say what is wrong. I may say too many symptoms that does not fit to one illness and the doctor need to decide what the problem is by dropping some of them
- Inconsistency - there may be contradiction
- Deceit



- SparQL
```
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
SELECT ?name 
       ?email
WHERE
  {
    ?person  a          foaf:Person .
    ?person  foaf:name  ?name .
    ?person  foaf:mbox  ?email .
  }
```