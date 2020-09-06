#  Přírodou inspirované počítání
- [Genetické algoritmy, genetické a evoluční programování](#genetické-algoritmy-genetické-a-evoluční-programování)
- [Teorie schémat, pravděpodobnostní modely jednoduchého genetického algoritmu](#teorie-schémat-pravděpodobnostní-modely-jednoduchého-genetického-algoritmu)
- [Evoluční strategie, diferenciální evoluce, koevoluce, otevřená evoluce](#evoluční-strategie-diferenciální-evoluce-koevoluce-otevřená-evoluce)
- [Rojové optimalizační algoritmy](#rojové-optimalizační-algoritmy)
- [Memetické algoritmy, hill climbing, simulované žíhání](#memetické-algoritmy-hill-climbing-simulované-žíhání)
- [Aplikace evolučních algoritmů (evoluce expertních systému, neuroevoluce, řešení kombinatorických úloh, vícekriteriální optimalizace)](#aplikace-evolučních-algoritmů-evoluce-expertních-systému-neuroevoluce-řešení-kombinatorických-úloh-vícekriteriální-optimalizace)

Zdroje:
[EVA1](http://ktiml.mff.cuni.cz/~neruda/eva1-16cz.pdf); [EVA2](http://ktiml.mff.cuni.cz/~neruda/eva2-13.pdf); [Evolucni robotika](https://ksvi.mff.cuni.cz/~mraz/EvoRob/); [Výcuc na štátnice](https://drive.google.com/drive/folders/10q-akLIsi_54HAsD3vnMDM8uzVy3Zf2l) :: p77+;

---
# Genetické algoritmy, genetické a evoluční programování
- evoluční algoritmy
    - generické označení pro výpočetní metody inspirované evolucí
    - **stochastické prohledávací algoritmy**, metaalgoritmus (spíš technika, než algoritmus), rodina algoritmů
    - delení (dnes mix, těžko zařadit pod jedno či druhé)
        - GA: Genetické algoritmy - původně integer based, parametry evoluce externí 
        - ES: Evoluční strategie - self adapting (učí se hyperparametry), původně real-valued
        - EP: Evoluční programování - generování programů 
- základní struktura
```
vytvoř náhodnou populaci, ohodnoť
dokud není splněna ukončovací podmínka (fitness, čas)
    křížení, mutace -> noví jedinci 
    ohodnocení nových jedinců
    selekce nové populuce z původní a nových jedinců na základě fitness
```

## GA (Simple GA, Holland)
- binární řetězce
- operace
    - **ruletová selekce**, pouze rodičovská selekce (rodiče nové populace)
        - může být přeškálovaná; podle pořadí a ne podle hodnoty fitness
        - p(i) = f(i) / Σ f(j)
    - 1 bodové **křížení**
    - bitové **mutace**
    - původně inverze (nefunkční)
- alternativy
    - Stochastic universal sampling (SUS)
        - náhodne vyber prvý bod, všetky ďalšie sú presne 1/n ďaleko
        - lépe garantuje poměry
    - turnajová selekce: n vybratých rodičov, výherca postupuje

## GP (Genetické programovaní = generovanie programov)
- původně na **syntaktických stromech**
    - vrcholy (uzly) = operace; listy = premenné a konštanty
    - křížení: výměna podstromu
    - mutace: mutace konstant, vymena uzlov rovnakej arity, permutace podstromu, zamena neterminalu za terminal
    - **ADF** (automaticky definované funkce): dvojúrovňové, místo low-level nodes používám komplexnější ADF podstromy (předdefinované, co-evolved, ...)
        - evolve subprograms that can be composed 
        - adds modularity
        - program has main() and one or more ADF subtrees
        - evolution doesn't mix main and ADF subtrees
- problémy: 
    - **bloat**: třeba nějak omezit velikost stromů: část fitness, chytré operátory, ...
    - konstanty: local search, ...
- linearni GP (bytecode)
    - môžu vniknúť nevalidné programy
    - jednoduchšie operátory i reprezentácia
    - rýchlejšia emulácia
- grafové GP
    - evoluce aj cyklických grafov
    - dnešní neuroevoluce (zejména architektur) je *in a way* pokračování 


## EP (Evoluční programování)
- snaha vytvoriť umelú inteligenciu, agent representovanán konečným automatem, prostředí sekvence symbolů 
- operace
    - populace: konečné automaty se vstupy
    - fitness: úspěšnost predikce výstupu 
    - turnajová selekce 
    - různé mutace (nové stavy, změna stavu, přechodu, ...)
    - **žádné křížení** 
- vs GA:
    - teoreticky víc universální, žádné omezení na kódování jedince 
    - velká škála mutací (od malých po velké), žádné křížení ~ není křížení jen metamutace? headless chicken (95)
        - křížení funguje u problémů, kde jsou explicitní building blocky, jinak moc ne; respektive ne líp než metamutace
- vs ES: 
    - EP turnajová selekce naproti deterministickému odstranění nejhorších

**Genotyp**: soubor vlastností geneticky udržovaných v populaci (genetická informácia)  
**Fenotyp**: konkrétní projev genotypu v jedinci závislý na konkrétním prostředí (charakteristiky jedinca)

---
# Teorie schémat, pravděpodobnostní modely jednoduchého genetického algoritmu
Schéma: 
- analýza subpopulací v GA
- 0, 1, *: libovolný symbol 
- stats
    - schéma s __r__ __*__: **2^r** možných jedinců 
    - jedinec délky **m** representován: **2^m** schématy (každá pozice může být pevný bod nebo *)
    - existuje **3^m** schémat délky **m** 
    - v populaci délky **m** o velikosti **n** je **2^m** až **n*2^m** schémat
- def
    - **řád schématu o(S)**: počet pevných pozice
    - **definující délka d(S)**: vzdálenost mezi první a poslední pevnou pozicí 
    - **fitness F(S)**: průměrná fitness odpovedajucich jedinců _v populaci_ 

## Věta o schématech

**Krátká** schémata s **nadprůměrnou** fitness a **malým** řádem se v populaci během GA **exponenciálně** množí.  
**P(t)** populace v čase t, **n** velikost populace, **m** délka jedinců  
**C(S, t)**: počet jedinců schéma S v populaci  

odhadujeme **P(S, t + 1)**  
- selekce (ruleta):
    - pravdepodobnosť vybrania schéma S
        - p_s(S) = F(S) / Σ_{u∈P(t)} F(u)
    - a teda C(S, t+1) = C(S, t) * n * p_s(S) (**n** výberov)
    - ekviva. **C(S, t+1) = C(S,t) F(S)/F_avg(t)**
    - schéma je nadpriemerné o **e%**, tj. F(S, t) = F_avg(t) + e * F_avg(t), pro t=0,...
    - potom C(S, t+1) = C(S, t) * (1 + e)
    - a teda C(S, t+1) = C(S,0) * (1+e)^t	
    - -> počet nadpriemerných schémat rastie geometrickou radou (v populaci, po selekci) 
    
- křížení (one-point, **p_c** pravdepodobnosť kríženia):
    - schema prežije kríženie, ak jeden z rodičov je danej schémy a aspoň jeden z potomkov
    - pravděpodobnost, že schéma nepřežije křížení
        - p_death(S) = d(S)/(m-1) -- krížim medzi pevnými pozíciami
    - p_surv(S) >= _1 - p_c * ( d(S) / (m-1) )_
        - nerovnost: bod křížení se může trefit dovnitř schématu a obě části stále odpovídají
- mutace (**p_m** prav. mutace): 
    - všetky pevné body musia ostať
    - p_surv = (1-p_m)^o(S)
    - pre p_m << 1 platí: p_surv ~= 1 - pm * o(S) 

C(S, t+1) >= **C(S, t) * F(S)/F_avg(t)** * (1 - _p_c * (d(S)/(m-1))_ - p_m*o(S))

## Hypotéza o stavebních blocích
- GA hledá suboptimální řešení problému rekombinací krátkých, nadprůměrných, s malých řádem schémat (building blocks)
- důskedky VoS 
    - GA pracuje s **n** jedinci, ale současně implicitně s **2^m** až **n*2^m** schématy 
    - Holland tvrdí, že počet schémat, s ktorými GA súčasne pracuje je až n^3
    - automaticky řeší **explorace vs expoitace**: skrze exponenc. lepší alokaci nadějným schématům odpovídá analytickému řešení 
    - problém odhadu fitness schémat v populaci
        - např. F(111*) = 2, F(0*) = 1, F(x) = 0, tak F(1*)=1/2 a F(0*)=+, jenže GA odhadne F(1*) ~ 2, protože 111* v populaci převáží 
        - kolaterální konvergence -> jakmile začneme konvergovat, tak jsou všechny samply schémat biased a nejsou samplované rovnoměrně 

## Pravděpodobnostní modely
- snahy o analytické popsání GA, alespoń SGA  
- **nejjednodušší JJGA**  
    - inicializuj náhodně populaci binárních řětězců x délky l 
    - dokud nenajdeš dost dobré x
        - dokud nenaplníš novou populaci
            - vyber selekcí 2 jedince, zkříž je z s p_c, jendoho potomka zahoď 
            - mutuj každý bit nového řetězce s p_m 
            - vlož do populace 
    - každého jedince jde brát jako binárně zapsané číslo 

- **analýza** 
    - def
        - p_i(t) = kolik procent populace zabírá číslo i 
        - s_i(t) = pravděpodobnost selekce řetězce i 
        - matice F, kde F(i, i) = f(i) | else 0, f(i): fitness jedince     
    - vzťah s(t) a p(t) -- proporciálna selekcia
        - s(t) = F * p(t) / Σ_{j=0..2^l-1} F(j, j) * p_j(t)
    - chceme definovat matici G, která realizuje krok JJGA: p(t+1) = G * p(t)
        - G = F ° M // zloženie matice na fitness a na mutace-křížení 

    - **G = F**
        - žiadne genetické operátory
        - E[p(t+1)] = s(t)
        - díky s(t+1) ~ F * p(t + 1) plati E[s(t+1)] ~= F * s(t)
        - problém konečných populací, čím větší, tím víc přesné
    - **G = M**
        - M zahrňuje aj kríženie aj mutáciu
        - r(i, j, k) = pravdepodobnosť, že z **i** a **j** vytvoríme **k**, známa, ide ju analyticky spočítať 
        - E[p_k(t+1)] = Σ_i Σ_j s_i(t) * s_j(t) * r(i,j,k)  
    
    - neznáme obecné fixed points (nemění distribuci stavů) 
        - pevné body F sú populácie s rovnakou fitness
        - pevný bod M je rovnomerné zastúpenie jedincov

- **konečné populace** 
    - populaci lze definovat jako stav **markovovského řětězce** (pravděpodobnost přechodu jen na předchozím stavu)
    - stochastický proces v diskrétnim čase
    - jeden stav: konkrétní populace
    - možných populací je mnoho, matice přechodu je velká, nemá zmysel 
    - asymptotické výsledky
    - dokázala sa korespondende JJGA a konečnej populácie n -> inf

---
# Evoluční strategie, diferenciální evoluce, koevoluce, otevřená evoluce
- původně primárně pro optimalizaci reálných funkcí 
- jako první přišla s **self-adaptation**, součást jedince jsou evoluční parametry 
- nový jedinec je akceptovaný, len ak je lepší
- notace
    - **M**: počet jedinců 
    - **L**: počet nových potomků
    - **R**: počet rodičů každého nového jedince 
    - **(M+L)**: M jedinců do nové populace z M+L nových i starých
    - **(M, L)**: M nových jedinců z L nových potomků (ukazuje se robustnější proti lok. extrémům)

    - jedinec tvaru: C(i) = [G_n(i), S_k(i)] | pre k = 1, nebo n, nebo 2n
        - genetické (ovplyvnujú chovanie) a strategické parametry (ovplyvňujú evolúcia)
        - k=1: společná odchylka pro všechny parametry 
        - k=n: nekorelované mutace gemetricky se mutuje po elipse rovnoběžné s osami, každý parameter ma svoju odchylku
        - k=2n: korelovaná mutace, odpovedá mutovaniu z n-rozmerného normálneho rozdelenia

**Populační cyklus**
- n=0, randomly init P(n) of size M 
- evaluate fitness in P(n)
- while not good enough: 
    - repeat l times: 
        - choose R parents 
        - crossover, mutate, eva new indiv 
    - choose new population (by the type of OS), n++

**Selekce Enviromentální**
- nový jedinec akceptován jen když je lepší 

**Mutace**
- genotyp nahrazen přičtením náhodného norm. rozd. čísla s patřičnou odchylkou (prípadne rotáciou)
- odchylka změněná tak, aby mutace měla úspěšnost on avg. 20% 

**Křížení** 
- uniformní (avg) / diskrétní (hodnotu jednoho vybraného rodiče)
- možné aj s viacerými rodičmi

## CMA-ES: correlation matrix adaptation ES 
- mean vybírání je **vážený průměr úspěšných jedinců** z minulé generace (větší váha lepším)
- udržuje se korelační matice pomocí iterativního EM, efektivně PCA se zachováním všech dimenzí
- dynamický stepsize podle úspěšnosti 

## Difirenciální evuluce
- každý jedinec projde mutací, křížením, selekcí
- pre jedinca **i** zvolme 3 ďalších a, b, c
- **donor**: v = a + F(b-c), F z [0,2]
- křížení: uniformní s pojistkou 
    - křížení původního jedince s donorem vznikne **u**
    - ve výsledku zajištěn alespoň jeden prvek z donora, zbytek náhodně s určitou pravděpodobností
- selekce: porovnání **u** a **i**, vezmeme lepšieho 

## Koevoluce
- více druhů se vyvíjí nezávisle na sobě 
- těžko postihnout fitness jednotlivých jedinců pokud závisí na dalším druhu 
- příklady: 
    - sorting networks vs. evil number sequences
    - hráči tic-tac-toe 
    - roboty
- sledováni fitness
    - změna jednoho hráče vede ke změně fitness landscape ostatních 
    - CIAO (Current Invidisual vs Ancestral Opponents)
        - 2D matica
        - najlepčí jedinec z generácie súperi so všetkými najlepšími z predošlých generácii
        - ideálne uhlopriečka (dva trojuholníky)
            - najlepší z novej generácie vždy porazí najlepšich z predošlých
    - Mistrovství
        - po skončení evolúcie, najleší z danej zo všetkými najlepšími
        - graf počtu víťazstiev
- zacyklení strategií:
    - může dojít k zacyklení
    - pamatovat si n-posledních, vyhodnocovat proti sobě,..

## Otevřená evoluce
- evoluce bez konce, neexistuje ukončovací podmínka 
- fitness funkce je dynamická, může být do jisté míry i kódování jedince 
- velké změny: nic nebude fungovat (no free lunch theorem)
- různé typy změn: 
    - malé změny
    - velké morfologické změny: vznikají nové extrémy, ...
    - cyklické změny 
    - katastrofické změny
- řešení: 
    - udržovat si nehomogenní populaci, nejen nejlepšího
        - niching: fitness je relativní species, species identifikované skrze podobnost, ... 
        - dynamické druhy: evoluce jen uvnitř, explicitní niching v podstatě
- bootstrap problem: 
    - na začátku všechna řešení nulová fitness -> komplexní problém
    - začnu lehčím úkolem -> postupně měním na komplexnější a komplexnější
    - hľadať bielu stenu -> hľadať biely štvorec -> hľadať biely trojuholník

---
# Rojové optimalizační algoritmy
- optimalizační metody inspirované přírodou, např. hejny ptáků, ryb, včel, mravců
- homogénne, jednoduché jedince vo veľkej populácii

## Particle swarm optimization
- **výměna informací** jen od nějlepších částic k ostatním 
- částice mají **omezenou paměť** (nejlepší pozici)
- population = swarm, candidate solutions = particles
- particle move based on its best position and based on the swarm best position
- finding new best position (hopefully) moves whole swarm
- každá částice má rychlostní vektor
    - aktualizuje se podle nejlepšího umístnění částice p v historii a podle nejlepší částice obecně
    - na začiatku uniformne volená
    - môže byť rôzna pre každú dimenziu

```
init each particle 
while not done: 
    gBest = max(p.pBest for each p in Particles)
    foreach p in Particles: 
        p.v = p.v + 
        + c1 * rand(0, 1) * (gBest - p.Pos) // pohyb na základe najlepšieho výsledku celej populácie
        + c2 * rand(0, 1) * (p.Best - p.Pos) // pohyb na základe najlepšieho výsledku danej častice
        update position p.pos = p.pos + lerning_rate * p.v
    p.pBest = max_pos{f.pBest, p.pos}
```


## Swarm inteligence (SI)
- sebeorganizace a dělba práce 
- PSO, ant colony optimization, ... 
- metaphor based alghorithms: inspirované reálnou skutečností (možno ale useless)
- robotika: záchrana ľudí, explorácia miestnostní, atď

### Ant colony optimization: 
- inspirované chováním mravenců při hledání potravy -> hledání cesty v grafu skrze feromony 
- p_ij(t) = t_ij(t)^a * n_ij^b / (Σ_{n∈J_k} t_in(t)^a * n_in^b)
    - p_ij(t): pravděpododobnost přechodu z i, do j v čase t 
    - Jk: uzly kam mravenec může aktuálně jít 
    - t_ij: množství nevypařeného fermononu 
    - n_ij: viditelnost mezi i a j 

    - umístění feromonů: Q/Lk: Q: konstanta, Lk: cena cesty, časem se vypařují t_ij(t+1) = (1-p)*t_ij(t) + Σ_{k=1..m} delta t_k_ij(t)
        - p: parametr vypařování, suma: přidání feromonů pokud byla hrana na cestě nějakého mravence k 
    
### Feromónová robotika
```
0. u: hodnoty všetkých uzlov sú nulové
1. s: startovací pozice
2. s': sousední uzel s minimální hodnotou u(s)
3. u(s) = u(s) + 1 // pokrytie grafu v exponenciálnom čase
    - varianty
        - u(s) = u(s') + 1 // poly čas pokrytí 
        - if u(s) < u(s') then u(s) = u(s) + 1 // poly
        - a ďalšie
4. přesuň se na s'
5. repeat 2
```

---
# Memetické algoritmy, hill climbing, simulované žíhání
- memetické algoritmy: obohacení EA o lokální prohledávání uvnitř cyklu 
- inspirováno meme: ideje/myšlenky/... šířící se ve společnosti 
- dva přístupy: 
    - **Lamarkismus**: skrze lokální hledání lepší jedinec je navrácený do populace, změna genotypu skrze fenotyp
        - není korektní w.r.t to darwinismus, biologická motivace
    - **Baldwinismus**: z lepšího jedince vezmu jen fitness pro přežití jeho nezlepšeného genotypu, hledání potenciálu

## Hill climbing 
- nejjednodušší forma prohledávání prostoru 
- vygeneruji náhodný bod v okolí, jdu na něj pokud je to zlepšení w.r.t to fitness funkce 
- varianty
    - **stochastic** HC: vyberu směr ruletou podle toho jak jsou strmé 
    - **first choice** první lepší směr vs **steepest ascent** (gradient descent)
    - random restart: náhodné restarty 
    
## Simulated annealing 
- lokální prohledávání kombinující HC a náhodnú procházku
- alg. náhodně zvolí stav a vydá se do něj pokud 
    - je **lepší** než aktuální stav 
    - je **horší** než aktuální stav, ale jen s pravděpodobností úměrnou od toho jak je špatný a aktuální **teploty**
        - výšší teplota -> vyšší šance jít do špatného stavu 
        - teplota se postupně snižuje, když se zaseknu -> zvýšit 
        - exp^( (f(next) - f(current)) / T) < rand(0,1)
            - T je teplota (na základe schedule podľa času t)
            - reprezentuje to probability function nad f(next), f(current), T

---
# Aplikace evolučních algoritmů (evoluce expertních systému, neuroevoluce, řešení kombinatorických úloh, vícekriteriální optimalizace)

## Expertní systémy
- forma pravidel if: then rules (založené na formálnej logike)
- **learning classifier systems**: expertní systémy učené pomocou evoluce
- **michiganský přístup** (jedinec je pravidlo), **pittsburský přístup** (jedinec je množina pravidel)
- většinou klasifikační úlohy, väčšinou nominálne (kategorické) príznaky 
- instance mají pevnou strukturu príznakov (nominální, diskrétní, spojité)

### Pittsburský přístup
- nejbližší klasickému GA
- jedinec je **množina pravidel** řešící problém, různě velká (problém variabilních jedinců)
- problém více matchujících pravidel: voting, priorita, ... 
- operátory jsou komplikované, na rôznych úrovniach
- Fitness: vezme se první matchující pravidlo, klasifikuje -> např. podíl správně klasifikovaných 
- příklad: **systém GABIL** 
    - nominální atributy, binární klasifikace
    - pravidla (komplexy) ve formátu predikát -> třída; predikát v CNF 
    - 1100|0010|1001|1 : první atribut 1. nebo 2. hodnota, druhý 3. hodnota, třetí 1. nebo 4. pak patří do třídy 1
    - snadné genetické operace 
        - na úrovni množin (na úrovni jedinca): výmena pravidiel generalizace, smazanie, specializace pravidla, ...
        - na úrovni komplexov: rozdelenie na selektory, úprava selektoru (specializace, generaliza)
        - na selektoroch: mutace 0-1, rozšírenie 0->1, zúženie 1->0

### Michiganský přístup 
- jedinec v populaci je **pravidlo**
- IF-THEN; 
    - vľavo: príznak nastal (1), nenastal (0), i don't care (žolík)
    - vpravo: kód akce či klasifikace kategorie
    - pravidlá majú váhu (úspešnosť), tá určuje fitness
- jde proti klasické evoluci, kde celá populace konverguje k jednomu řešení (nechceme)
    - šlo by řešit skrze: Iterative rule learning: iterativně spouštíme GA
        - najdeme pravidlo, uložíme pravidlo, 
        - smažeme z trénování všechny dobře klasifikované jedince
        - spustíme GA znovu a najdeme další pravidlo, ...
- na klasifikaci se podílí celá populace 
    - evoluce není po generacích, ale jen občas a po částech populace 
    - fáze: 
        - objevování nových pravidel (GA)
        - učení odměna/penalizace jedinců podle toho, jak si vedou při klasifikaci 
    - LCS
        - původně měl systém podporu pro posílání zpráv (správy součástí pravidel, fronta, doplnené receptory na čítanie správ a znaky na posielanie)
        - "pamäť"
    - klasifikace a odměny skrze kredity za které soutěžili o řešení, těžkopádný systém 

**Zero classifier system**
- pravidlá sú IF THEN (bitové mapy)
- **cover operator** - pokud žádné pravidlo neodpovídá: vytvoří se nové pravidlo ad hoc pro daný vstup a pridajú sa náhodne žolíky
- odmeny/úprava síl pravidiel
    - pravidla, která neodpovídají dané situaci, tak nic
    - pravidla, která **odpovídají** vstupu, ale mají **jiný výstup**: síla se zmenší násobením konstantou 0<T<1 
    - všem pravidlům se zmenší síla o maloučást B 
    - tahle síla se rozdělí rovnoměrně mezi pravidla, která **minule** odpověděla správně, zmenšená o faktor 0<G<1 
    - nakonec, odpověď od systému se zmenší o B a rozdělí rovnoměrně mezi pravidla, která teď odpověděla správně
- nevýhody
    - negeneruje kompletný pravidlový systém, pokrývajúci všetky prípady
    - pravidlá na začiatku môžu predčasne umierať
    - pravidlá s nízkou odmenou môžu umierať, hoci sú neskôr dôležité
- **XCS** 
    - cieľ: oddeliť fitness od predpokladaného výdelku
    - fitness na základe presnosti pravidla (nie množstve)

## Neuroevoluce
- evoluce neuronových sítí 
- tradičně dnes: pevná architektura, váhy skrze backprop. 
- evoluce lze použít and/or: 
    - architektura 
    - váhy 
    - různé kombinace jednoho/druhého/obojího s jinými přístupy
    - v obou případech jde v podstatě o reinforcement learning (učení bez učitele)
### Učení vah 
- v podstatě učení floating point GA optimalizace funkce 
- mnoho parametrů, mnoho funkčně ekvivalentních jedinců (napr. symetricky priradených váh)
- jiné formy optimalizace (backprop) fungují téměř vždy rychleji/lépe 
    - možná kombinace: lokální search skrze backprop, evoluce
    - nie vždy je možný backprop (záleží na funkcii)
    - backprop je silne závislý na inicializácii -- môžeme použiť evolúciu a len potom dohľadávať 

### Učení struktury
- prostor architektur je obtížně diferencovatelný: gradientní metody moc nefungují 
- různé možnosti kódování: 
    - přímé kódování: binární matice synapsí / podbloků 
    - gramatické kódování: skrze gramatiky 
    - celuární kódování: genetické programování -> automat vytvoří síť (přidej neuron, uber, rozděl, ...)
- operace 
    - růst
    - prořezávání (než se nezhorší fitness)
    - dekompozice (podsítě, moduly, ...)

#### SANE 
- evoluce částečné sítě 
- struktura pevná, **jedna skrytá vrstva**, jedinci v evoluci neurony
- ohodnocení: vybere se pevný počet neuronů, postaví se síť, vyhodnotí se, garance výběru neuronů
- fitness neuronů je průměrná sítí kterých se účastnil

#### ESP
- podobný princip, každá lokace neuronu má vlastní populaci
- větší tlak na diverzitu, redundanci, ... 

#### NEAT 
- evoluce topologie společně s vahami 
- smysluplné křížení pomocí historických značek 
- ochrana inovací pomocí druhů (súťaženie len vrámci druhu) 
- operace: držím samostatný list vrcholov a hrán 
    - mutace (napr.)
        - pridaj vrchol na hrane
        - disable/enable hranu
        - pridaj hranu
    - kríženie
        - každý neurón/spoj má svoju genetickú značku
        - ak je uzol len v jednom rodiči, vlož
        - ak v oboch, tak náhodne vyber
    - spíš růst sítí než zmenšování 

#### HYPERNEAT:
- vyvýjíme síť, která postaví naší síť, pomocná síť CPPN (Compositional Pattern Producing Network)
    - různé nelinearity, nejen sigmoid, ... 
    - vstup: 2D souřadnice dvou neuronů; výstup váha 
- umožňuje explicitně zachytit geometrické závislosti problému
    - dobré pro malou robotiku, ... 
    - neškáluje na velké sítě 

#### CO/DEEP/NEAT 
- evoluce stavebních bloků (několik málo vrstev) a blueprintů pro stavby velkých bloků 
- spojené s backprop optimalizací vah bloků, evoluce toho jaké váhy se mají sharovat, ... 
- modularita

## Řešení kombinatorických úloh 

### Batoh
- batoh: kapacita CMAX, snaha maximální cenu aby váha < CMAX 
    - kódování: jedinci délky n: vezmem i-ty predmed (ano-ne) 
    - klasické operátory křížení, mutace, ... 
    - fitness: problém -> dva členy - vicekriterialni
        - **max** súčet cen & **min** zaplnenosť
    - problém: mnoho jedinců není validní řešení (predmety sa nezmestia) 
        - změna kódování na: 1 znamená dej do batohu, pokud se nepřeplní -> všechno jsou přípustná řešení 

### TSP
- fitness: délka cesty (na úplnon grafe)
- kódování: různé problémy 
    - **sousednost**: město j je na pozici i vede-li cesta z i do j 
        - nefunguje křížení, fungují schémata (*3* značí všechny cesty s hranou 2->3)
    - **buffer**: pořadí města v bufferu, postupně se mažou buffer: 123456 a repre 1121 kóduje 1-2-4-3
        - lze použít jednobodové křížení 
    - **permutace** 
        - nefunguje křížení, lze použít speciální operátory 
        - **PMX: partially mapped crossover**: 
            - vybereme náhodný podřetězec (od a po b u oboch rodičov) AXXAA, BYYBB
            - tieto dva podreťazce tvoria mapovanie XX na YY
            - BXXBB, akurát na A použijeme to mapovanie z XX na YY
            - pozor, to mapovanie môže mať viac krokov (hľadáme také, kde to není v XX)
        - **OX: order crossover**
            - beze změny zkopírujeme náhodný interval z rodiče1 
            - vezmeme nepoužité hodnoty z rodiče2 a počínaje pravým koncem je dokopírujeme do potomka 
        - **CX: cyclic crossover**
            - identifikujeme cykly mezi dvěma rodiči, do nového jedince kopírujeme striedavo po těchto cyklech
            - napr. cyklus dĺžky 1 zostane na tej pozícii v oboch
        - **ER: edge recombination** 
            - pro každé město seznam hran které do/z něj vedou a jsou puožity v nějakém z rodičů 
            - začnu náhodným městem, připojím další, vždy vybírám město s nejméně zbývajícími hranami (odeberu)
            - jen málokdy selže ~1 pct 
            - vylepšení: přednost dvojnásobným hranám (v obou rodičích, snaha zachovať viac z rodičov)
- úspěšnost hodně závisí na inicializaci 
    - nejbližší sousedi: hladová inicializace 
    - vkládání hran: na začátku náhodná hrana, přidej k cestě T nejbližší město mimo cestu T. Najdi hranu k-j v T, že minimalizuje rozdíl mezi k-c-j a k-j. Nahraď je. 
- mutace
    - inverze !
    - vkládání města do cesty 
    - posun podcesty 
    - záměna 2 měst 
    - záměna podcest 
    - Lokální zlepšení 2-opt, 3-opt, ...
    - inicializace skrz aprox. algoritmy, ... 

### Rozvrhování 
- matice: řádky učitelé, sloupce předměty 
- zamýchej předměty, samýchej učitele, vyměň lepší řádky jedinců 
- nutno respektovat hard constrains v operátorech

### Plánování
- kódování je (zas) kritické 
    - permutace: plán je permutace pořadí výrobků (nutné dekódování do plánu, těžko designovat operátory ~ lze špatně použít TSP operátory)
    - přímá reprezentace: jedinec jako plán (speciální operátory)

## Vícekriteriární optimalizace
- potřeba optimalizovat víc fitness funkcí najednou 
- **x slabě dominuje řešení y** pokud je ve všech kritériích f_i(x) >= f_i(y)
- **x silně dominuje y**: slabě dominuje a v aspoň jednom ostro j: f_j(x) > f_j(y)
- řešení jsou **nesrovnatelná**: ani jedno nedemoninuje druhé 
- **pareto optimální fronta**: fronta řešení, které nejsou dominována jiným jedincem  
- řešení: 
    - agregace fitness do jednoho skaláru
        - vážená suma všech fitness - problém vybrať správne váhy

### VEGA (Vector evaluated GA)
- populaci N jedinců utřídím podle každé z n fitness funkcí 
- pro každé **i** vyberu **N/n** nejlepších jedinců dle **f_i**, na ty aplikuju křížení, mutaci, ... 
- obtížné udržení diverzity, snadná konvergence k extrémům v jednotlivých f_i 

### NSGA
- fitness se počítá podle dominance, diverzitu zaručuje explicitní niching 
- populace rozdělená na fronty F1, F2, .... 
- pro každého jedince i ve frontě Fk se spočte niching faktor sh(i) 
    - sh(i) = sum_j€Fk sh(i, j) | sh(i, j) = 1-(d(i, j)/d_share)^2 pro d(i, j) < dshare, else 0
    - vyjadřuje jak blízko je ostatním jedincům v populaci, 1: hodně blízkých sousedů, 0 samotný 
- F1 mají dummy fitness dělenou niching faktorem, F2 dummy fitness menší než je fitness nejhoršího z F2 dělenou jejich niching factorem, ... 

### NSGA II. 
- místo nutnosti dělit dshare crowding distance: součet přes všechna kritéria of vzdálenost nejbližšího horšího a nejbližšího lepšího jedince (v každém kritériu)
- namísto explicitního počítání fitness turnej: prvně podle fronty (menší lepší), druhak podle crowding distance (větší lepší)
- explicitní elitismus: do nové generace se postupně kopírují fronty dokud se již nevejde celá, pak podle turnajového systému výše 

### Porovnání algoritmů
- **IGD: Inverted generational distance**
    - průměrnou vzdálenost každého řešení na skutečné pareto opt. frontě k nejbližšímu řešení ve výsledné frontě 
    - různé možnosti jak získat refereční pointy "skutečné pareto opt. fronty"
        - např. všechny nedominované řešení ze všech runů, uniformní sampling z téhož, ... 
- **Hyperobjem**
    - objem prostoru dominovaného danou množinou
    - pro 2D plocha "nad" body (omezené referenčním bodem společném pro všechny pro zabránění nekonečnosti) 
