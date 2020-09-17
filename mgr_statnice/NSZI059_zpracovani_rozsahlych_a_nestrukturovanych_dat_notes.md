- [Distribuce s horizontální fragmentací, implementace NoSQL databází, CAP teorém.](#distribuce-s-horizontální-fragmentací-implementace-nosql-databází-cap-teorém)
- [Big Data management - distribuce, škálování, replikace, transakce](#big-data-management---distribuce-škálování-replikace-transakce)
- [MapReduce](#mapreduce)
- [Úložiště typu klíč hodnota](#úložiště-typu-klíč-hodnota)
- [Sloupcová úložiště](#sloupcová-úložiště)
- [Dokumentová úložiště](#dokumentová-úložiště)
- [Techniky vizualizace dat](#techniky-vizualizace-dat)
- [Modely pro fulltextové dotazování - vektorový, booleovský](#modely-pro-fulltextové-dotazování---vektorový-booleovský)
  - [TLDR](#tldr)
- [Podobnostní dotazy v multimediálních databázích, metrické indexační metody.](#podobnostní-dotazy-v-multimediálních-databázích-metrické-indexační-metody)
  - [TLDR](#tldr-1)
- [RDF(S) modely splňování, deskripční a dynamická logika, webovské dotazovací jazyky, model sémantizace webu.](#rdfs-modely-splňování-deskripční-a-dynamická-logika-webovské-dotazovací-jazyky-model-sémantizace-webu)
- [Formální základy RDF a jazyka OWL, deskripční logika.](#formální-základy-rdf-a-jazyka-owl-deskripční-logika)

Resources: 
- [databazove koncepty](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NDBI040)
- [vyhledavani na webu](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NDBI034)
- [techniky vizualizace dat](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NDBI042)
- [semantizace webu](http://is.cuni.cz/studium/garantlink.php?glogin=false&gmodul=predmety&gscript=redir.php&redir=predmet&kod=NSWI108)


## Distribuce s horizontální fragmentací, implementace NoSQL databází, CAP teorém. 
and
## Big Data management - distribuce, škálování, replikace, transakce

Big Data rechnologies - typically for write once, read many (reports and stuff)

|| Horizontal | Vertical |
|:---|:---|:---|
|Scaling|Ad hoc| proactive|
|Limit|limited by available commodity machines| limited to the best machine available|

Network falacies (reliable network, no latency  )

(rict neco o big data, ktera jsou motivaaci pro NOSQl)
Big data 4v
- Velocity
- Variety
- Veracity (reliablity)
- Volume

Replication and Sharding
- can be combined
- Shards are machines that hold a given subset of data (parition??)
  - different data placed on different machines
  - Idea: related data are accessed together
  - user is connected to the shard that is trying to read from
  - due to the potential of being unavailable, it's combined with replication
- Replicas are machines with the same data 
  - master slaves
    - Appointed master or changing
    - limited by one machines (typically writes)
  - peer to peer
    - all nodes serve read/write requests
    - and then communicate the changes among others
  - peer to peer replicas have write write conflict (majority should agree on the result)
  - typically 3 replicas, one on the rack, other on the node and the third one somewhere else

Transactions 
- Logical group of operations
- NoSql have lightweight support 
  - only one row or document
  - or multiple but with no rollback

Implementace nosql databazi
- Nemaji moc transakce (nebo jen limitovane-besteffort, mongo nepodporuje uplny rollback)
- read write conflicts
  - optimistic vs pesimistic
  - quorum
- KV storage
- document db
- column family
- graph DB
- map reduce

CAP theory

- Consistency - After an update, all readers read some data
- Availability - All requests are always answered
- Parition tolerance - System continues to work even after network is paritioned

BASE (rict neco o acid)
- Basically available
- soft state
- eventually consistend

Relacni model 
– tabulky, vztahy, klice, constrainty, 
  - pomale

Big data
- large scale tera bytes
  - 7 mld lidi, (predpokladejme)kazdej ma mobil, notebook a dalsi. Vsechno to generuje data
  - variety - na kazde dato se hodi jiny typ (text, video, obrazek atd)
  - velocity - rychle generovana data se potrebuji take rychle zpracovavat
  - veracity - neda se na ne moc spolehnout
  
NOSQL
- non relational
- distributed and scalable
- often schema free, thus flexible
- supports big data
- cloud based (could be on premises but who wants that except banks, telco and goverment)
Not updated much, more like write once, read many times... or replace 

General stuff
- Distributed - different objects stored at different places 
- Replicated - one object stored at multiple places
- Give up ACID
- Strong consistency -> eventuall
- Vertical scaling 
  - does not scale well
  - exponentionall costs
- horizontal
  - commodity hardware
  - loosen up some assumptions
    - errors is a rule rather than exception
    - network partitioning sometimes happen
    - ...
- CAP
  - Consistency
    - After update, all clients can see it immediatelly 
  - Availability
    - All requests are always answered 
  - Parition tolerance
    - System works even if the network splits
  - Only two of them can be chosen (in reality, there is a tradeoff)
  - ACID - DB is on a single machine
  - BASE
    - basically available - partitionaing does not affect the system
    - soft state - changes occur all the time
    - eventually consistent - the system will be consistent at some point in the future
  - Replication, sharding or both
    - Sharding
      - Each node has its own access methods
      - how to ensure that similar data are accessed together
      - Still requires some replication
    - Replication
      - Master slave - master writes, slaves reads (reading scales, writing does not)
      - Peer2peer
        - write write conflicts - solved by write quorum


## MapReduce

- Massive paralalelization
- Used by all other NOSQL technologies
- MAP - task is broken down to sub problems (key,pair)
- Reduce - processes values with the same keys
- many nodes
- onlu custom logic is specified by programmes the framework does everything
  - task distribution among all the nodes and stuf
- phases: input - split - map - shuffle - \[combine\] -reduce - output
- fault tolerant 
  - all jobs of the failed worker are given to different worker
  - master is either ensured to not fail (its failure means the task failuer) or restarted as well
- stragglers - backup execution
- disc oriented mechanism
- slowly replaced by different techniques (cloud based stuf)

Supported by Hadoop (Spark)

Spark
- Batch oriented data analysis framework
- User write script (python or java) which, from defined source, extract data, transform and load it somewhere
- RDD (resilient distributed collection)
  - readonly distributed collection of data
  - can do unstructured data
  - low level
- DataFrame
  - more abstract
  - only structured
  - something like a table
  - from v2 its api  is the same to Dataset
- Dataset
  - typesafe RDD
  - but can do unstructured
- user writes query that is broken down into multiple tasks

Hadoop
- HDFS Distributed file system
  - fault tolerant (replicated)
    - usually 3 replicas (1st at the same rack, 2nd somewhere else)
  - scalable
  - works best with write once-read many times and batch processing
  - file is a block 64MiB
  - name node - filesystem manager
  - data notes - actual hold files (send heart beat to namenode)
  - edit logs
- YARN Resource manager and allocator
- Map reduce
- runs on comodity HW 
- Cassandra, 
- Hbase(also column family)
- Hive warehouses
- Zoo keeper
- Zeppelin


## Úložiště typu klíč hodnota

- use for simple data
  - no relations, no quering etc

Riak 
- map reduce and fulltext search on top of usual mime types (data must be indexed)
- keys are paritioned (buckets)
- allows adding relationships 
  - done by links
  - example: author an a song and stuff
- BASE transactions
- uses write qorum
- data distributed using consistent hashing (each node is responsible for interval of hashes)
- gossip protocol
- value is marked with vector clock
- customizable conflict resolution logic

Redis
- In memory
- keys are binary
- publish/subscribe to channels
- string, list, set, ordered set, hash 
- atomic commands
- supports transactions for multiple commands
  - command is not executed but queued for execution
  - starts with EXEC
  - error before exec discards the whole sequence
  - errors after exec is not handled
    - no rollbacks
    - because its faster and errors are programers problem
- master slave
- non blocking replication

## Sloupcová úložiště

- Columns of related data 
- motivation - a record with firstname,surname, birth, gender has two column families (firstname,surname) and (birthday,gender). When you want to find all males born in 1950, you access only one column family.
- A row has many columns (may differ to columns of other rows)
- More like group of data frequently accessed together (e.g. profiles, event log, blogs)
- row oriented, column oriented formats
- Don't use when
  - ACID  required
  - aggregations used frequently

Cassandra
- uses CQL(like sql with no JOINs)
- Column - key-value
- Row - bunch of columns attached to key
- Columns are not modeled in advance
- no foreign keys
- supercolumnss
- datatypes do not haveto be specified
- group by primary key (may be composed)
- persistence layer like LDG
- write request passed to multiple nodes
  - number of responses required to assume successful write is configurable
- repair read - multiple replicas return the same row, the ones with out-dated one will get it replaced
- tombstone-like deletion
- compaction is performed once in a while
  - tombstones removal
  - ss table removal
  - compaction has two different strategies (simple-threshold triggered and complext extensive hierarchical SSTables)
- Peer to peer with coordinator

## Dokumentová úložiště

- Json, xml
- expected to be similar, but does not have to be
- key-value stores with examinable value part
  
mongo
- db - collection (structure of  documents)
- stored as BSON (max 16 MiB, chunked otherwise)
- referencess vs embedded documents
- master slave
- sometimes, the nearest(shortest ping time) replica isi selected
- replicas can have primary and secondary ones that changes
- transactions for multidocument
  - atomicity all or nothing even for multidocument
  - makes invisible changes, make then visible after successful commit
- single doc update is atomic
- 


## Techniky vizualizace dat

- Cil: 
  - Jasne komunikovat obsah data pomoci grafickych nastroju
  - zvyraznit uzitecne informace
  - odhalit strukturu dat
  - data storytelling
- Pruzkum dat - vysvetleni 
- grafy (prumery, mediany, korelace ...)
- zrak tvori velku cast (70%)
- sacadicky pohyb oci 
- snadno se orientujeme pomoci ruznych barev, intenzita
  - mame malou pamet
  - idealne potrebujeme vsechno videt naraz
  - take vnimame delku sirku... 3d pozici nebo i smer
- !! Reprezentace filmu pomoci donutu 
- take muzeme lidi naschval mast (pomoci velikosti bublinek reprezenotvat linearni data a ne plosna )
  - `>-<` vs `<->`, nebo bevel and emboss ... nebo okoli (sedive ctverecky)
  - x/y axis manipulation - usekneme cast grafu a ukazeme jen rozdil e.g. 
```
------ -> ----
---       - 
```

- Kvantitativni data - meritelna
- kategoricka - jdou popsal slovy (ENUM)

Nastroje
- tabulky a grafy
- bodiky
  - scatter plot
  - lines (pozor na prerusovane cary)
- bars
- boxes
- pies
- fillpattern is harder to distinguies
- timeseries - pozor na cteni z leva do prava
- razeni ma smysl (GDP per Country)
- mapy
- objects close together (or enclosed) are perceived as group 
- column vs row oriented
- body spojene carami (matou protoze vypadaji spojte)
- Site
  - fyzicke, comunita, biologie
  - estetika (overlap, link size, link crossing, area, alignment)
  - tree
  - spring (mechanical) based vykreslovani (pomaly a spatne pracuje s klastry)
- PCA:Spocitame covariance matrix
```
Var(X), Cov(X,y)
Con(x,y), cov(y)
```
- vyhodime vlastni vektory s nejnizsimi vlastnimi cisly a na ostatni projektujeme hodnoty z dane mnoziny-
- MDS - PCA s jinou metrikou (maximalizace corelace je stejna jako minimalizace vzdalenosti - v euklidovskem prostoru)

## Modely pro fulltextové dotazování - vektorový, booleovský

### TLDR
- Preprocessing
  - stemming
  - stop words
- Boolean
  - document is a vector of words
  - query - t1 && t2 or (not t3)
  - inverted index
    - union of documents
  - only binary results
- Vector
  - bag of words
  - query has also weights
  - matrix of weights
    - based on tfidf ... metrics of relative occurence related to the document
  - inner product with the matric and we get list of occurence


Preprocessing
  - remove unwanted characters and markups (punctuations, whitespaces, html tags)
  - break into terms 
  - stemming
  - remove stop words (is, a, the, ....)
- collection of documents
- all terms of the collection consist in the vocabluraly
- query (set of few keywords)
- sparse matrix and inverted index

Boolean mode
- each document is set of terms
- we have query Q = (W_1 or ...) and  (W_i)
- then for each clause of this query, we obtain `true|false`
- and then we pick the documents for which the query is `true`
- implement via invereted index
- pros
  - easy for simple queries
  - clean formalism
  - reasonably efficient
- cons
  - limited by or,end, not operators
  - number of documents
  - no output ranking
  - no relevance feedback
  
Vector mode
- input bag of words
- query consists of terms and respective weights (Q = nature (.5); forest(.2); mountain(.3)) (no boolean)
- similarity based on frequency of queried words in the document
- impl for d_i and t_j
  - calculate frequency_ij  = frequency of term i in doc j
  - normalize it tf_ij = frequency_ij/max_j(frequency_ij)
  (terms present in many documents are less significant)
  - df_i = number of documents containing term i
  - idf_i = log_2(n/df_i) (n is number of documents)
  - weighting scheme is tf_ij*idf_i
-example
  - n = 10000
  - d = `<mountain(3), forest(2), nature(1)>`
  - frequencies mountain(50), forest(1300), nature(250)
  - calc 
    - mountain `tf = 3/3, idf = log_2(10000/50)  tfidf = 7.6`
    - forest `tf = 2/3, idf = log_2(10000/13000) tfidf = 2.0`
    - nature = `tf 1/3, idf = log_2(10000/250)   tfidf = 1.8`
  - similarity
    - inner product (d_j, q)
    - cosine similarity
  - Latent semanting indexing
    - Using SVD it reveals semantically simmilar topics
- pros
  - simple
  - nonbinary weights
  - degree of similarity
- cons (in its simplicity)
  - long documents have poor similiarity
  - doesnot work for substrings
  - does not follow order


## Podobnostní dotazy v multimediálních databázích, metrické indexační metody. 

### TLDR
- Similarity
  - feature extraction(ann, skull timeseries )
  - hand made 
  - metric/non metric
    - nonmetric
      - expert driven
    - metric
      - kun, kentaur, clovek - trojuhelnikova nerovnost
  - high/low level
    - highlevel
      - based on smart features
      - kolo
    - lowlevel
      - aggregation on query times
      - slower but more flexible
      - skull and time series -> distance based on time series
- Methods
  - Ball region
    - 100% recall
    - number of results is not known in advance
  - kNN 
    - ball but with growing radius
    - size depends on number of results we want
    - results may not vary - should be somehow distinct
    - could mix different methods of retrieval
  

- semantic gap - the difference between descriptions of the same object (linquistic description and image description) -> difference between how human and computer understand somehing


- Metrics
  - Vector similarity
    - Cosine
  - Histogram similarities
    - Quadratic form
    - EMD - piles of dirt (expencive)
  - string similarities
    - DNA
  - sequence similarities
    - similar to string (the skull time series)
  - motivation - fast similarity search
    - uses pivots
    - based on metric postulates
    - design
      - pivot tables
      - tree based 
      - hashed
      - index free 
    - AESA
      - every point is a pivot
      - precomputes distances of all points
      - we don't need to compute all the distances when the query is calculated
      - https://tavianator.com/2016/aesa.html
    - LAESA
      - only k points are pivots
    - gh -tree
      - recursively partition database (like range query in some trees from datovky)
    - d index - hashing
    - index free
      - cached values in matrix

## RDF(S) modely splňování, deskripční a dynamická logika, webovské dotazovací jazyky, model sémantizace webu.
and 
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