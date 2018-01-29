- Darwin 1859: omezené zdroje prostředí, základem je reprodukce
- Mendel 1856: gen jako jednotka dědičnosti, dva páry jen jeden se přenese na potomka
  - genotyp (geny) x fenotyp (vnější znaky)

No free lunch theorem
- neexistuje žádný nejlepší algoritmus, vše dobré jen jak generický search
- vyplatí se vytvářet doménově specifické varianty EA

Generic EA:
- mating selekce
- rekombinace a mutace -> offspring
- enviromentální selekce nové generace

Genetické algoritmy
- Holand 1975
- Binárně zakodovaní jedinci
- Rulettová selekce (pravděpodobnost výběru fitness jedince / fitness všech)
- 1point crossover (vezme se bod, první půlka z prvního, druhá z druhého)
- bitové mutace
- nad nimi postavená teorie schémat

Evoluční programování
- 1965 Owens a Walsh
- evoluce konečných automatů
- smazán rozdíl mezi genotypem a fenotypem
- důraz na mutace, většinou bez křížení
- turnajová selekce (vyber dva (čí víc) náhodně, s nějakou pravděpodobností vyber lepšího, else horšího)

Evoluční strategie
- 1964
- optimalizace vektorů reálných čísel (floating point)
- základní operace je mutace (heuristikou / adaptivní)

Genetické progrmaování
- jedinci LISPovské stromy
- nejen evoluce programů, operátory křížení mutace, inicializace
- evolving HW, neuroevoluce, ...

GA
- původní návrh označujeme jako SGA (bitový jedinci, ...)

Schémata:
- řád schématu: počet pevných 0, 1
- definující délka: vzdálenost mezi první a poslední pevnou pozicí
- fitness schématu: průměrná fitness jedinců v populaci
- "Krátká nadrpůměrná s malým řádem se v populaci během GA exponenciálně množí"
  - Schéma ma pravděpodobnost vybrání úměrné svojí fitness (nadprůměrná)
  - Čím menší definující délka, tím větší pravděpodobnost přežití křížení
  - Čím menší řád, tím větší pravděpodobnost přežití mutace
- důsledky
  - na zakódování záleží
  - na velikosti záleží
  - předčastná konvergence škodí
- Implicitní paralelismus
  - GA pracuje s m jedinci, implicitně vyvíjí 2^m až n2^m schémat

- GA nesamplují schémata rovnoměrně, ale "z populace". Mohou je odhadnout špatně.

- Explorace: nacházení nových oblastí: nevyužívá předchozí znalosti
- Exploatace: prohledávání aktuálních oblastí: uvíznutí v lokálních minimech

Problémy GA: 
- pokud populace začne konvergovat přestanou být schémata samplována stejnoměrně
  - je-li 111**** jeho jedinci převáží, tj. ***00000 jsou samplované vždy se začátkem 111
- pro velký rozptyl bude GA konvergovat jen na těch částech prostoru, kde je velká hodnota

Kódování:
- Binární: holand, teoretické výsledky, binární délky 100 lepší než desítkové 30 neb víc schémat
  - dnes víme, že schémata nejsou to nejdůležitější
  - leckdy nepřirozené a neefektivní
- víceznakové abecedy
- celočíselné
  - jednobodové, vícebodové křížení
  - mutace: zatížená (posun od aktuální hodnoty), nezatížená (celý rozsah)
  - uniformní křížení (u každé položky s pravděpodobností vezmeme z rodiče)
- floating point
  - aritmetické křížení (průměr, konvexní kombinace, ...)
  - zatížená / nezatížená mutace
- permutace, stromy, matice, ... 

Selekce
- ruletová (tradiční pravděpodobnost, "n" hodů)
- SUS: jedna náhodná pozice v ruletovém kole, další posunem o 1/n
  - spravedlivější neb podíl vybraných pro celočíselné podíly vždy odpovídá
- turnajová selekce (vybere se k, nejlepší s pravděpodobností p, druhý s p*(1-p), ...)
  - možné tam, kde fitness není explicitně dána, ale je získaná simulací, např


Evoluce kooperace: 
- darwinismus je inherentně kompetetivní, jak se v přírodě může objevovat kooperace?
- Teorie evoluce altruismu
  - skupinová selekce
  - příbuzenská selekce
  - Sobecký gen: snaží se přežít gen, ne jedinec
  - Reciproční altruismus: pokud je výhodný pro oba, tak proč ne (iterované věžnovo dilema)
- Nashovo ekvilibrium: ani jednomu z hráčů se nevyplatí zlepšit strategii
- Paretovo optimum: neexistuje strategie, která by byla pro oba hráče lepší
- Pro iterované
  - TFT většinou velmi dobré
  - I evolučně se naučí TFT
  - v prostředích s šumem dobrá Win-stay, loose-shift

- Důležité vlastnosti
  - nezrazuj první
  - oplácej zradu, ale dokaž prominout
  - musí být relativně jednoduchá
- neexistuje jedna nejlepší strategie


Evoluční strategie:
- 60. léta, optimalizace reálných funkcí mnoha parametrů
- jedinec
  - genetické parametry: ovlivňují chování
  - strategické parametry: ovlivňují evoluci
- nový jedinec jen, je-li lepší
- dnes "nejlepší" CMA-ES (Correlation matrix adaptation-ES)

- parametry: M počet jedinců, L počet vznikajících potomků, R počet "rodičů"
  - (M+L): M jedinců do nové je vvybráno z M+L starých i nových
  - (M, L): M nových je vybráno jen z L nových jedinců: lepší proti uvíznutí v lok. minimu

- ES cyklus:
  - Náhodně inicializuj populaci M jedinců
  - Ohodnoť jedince pomocí fitness
  - Vyber R rodičů, zkřiž, mutuj, ohodnoť nové, vyber M nových

- ES Mutace: 
  - jedna společná odchylka pro všechny
  - každý svojí odchylku
  - nemutuje se jen podle os dimenzí, ale jsou korelované mutace 

ES Mutace:
- 1/5 pravidlo: pokud je podíl lepších dětí > 1/5, tak se zvětší síla mutace zvětší, jinak zmenší
  - drží úspěenost mutace okolo 20 %
  - pokud je moc úspěšných, tak hledám už moc lokálně

ES křížení: 
- lokální, globální, diskrétní, aritmetické (průměr)


Differenciální evoluce: 
- mutace: posun podle ostatních
- křížení: uniformní s pojistkou
- selekce: porovnání & případně nahrazení lepším potomkem

- mutace: vezmeme další 3 jedince, vypočítáme z nich difference, ten přičteme k mutovanému
- křížení: uniformní, parametr c určuje pravděpodobnost změny

Particle Swarm optimization: 
- populační pohledávací algoritmus, 1995 Kenedy
- inspirovaný hejny hmyzu/ryb
- jedinec vektor reálných čísel: částice, žádné křížení ani mutace jak ji známe

- init všechny částice
- spočítej fitness všech, pokud nějaká nejlepší, nastav její best jako gBest, pro každou nastav pBest
- spočítej pro každou částici její rychlost, aktualizuj její pozici
- repeat 2.

- rovnice rychlosti: v += c1 * rand() * (pbest - present) + c2 * rand() * (gbest - present)
- postupně konvergují k globálnímu maximu + ke svému vlastnímu maximu
- částice mají paměť, globální sdílení jedné informace

Evoluční strojové účení: 
- učení pravidel na základě předkládaných dat
- data mining, expertní systémy, učení agentů, robotů, ...

- Michiganský přístup: pravidlo je jedinec
- Pittsburg: jedinec je množina pravidel

Michigan: 
- 80 léta
- jedinec GA pravidlo, celá popoulace jako řídící systém
- levá strana příznak nastal / nenastal / don't care
- pravá strana: klasifikace
- pravidla mají váhu (úspěšnost), určuje fitness v evoluci
- evoluce nemusí být v generacích

Michigan LCS: 
- evoluce jen občas a jen na části populace
- problém reaktivnosti: pravidla se mohou chainovat, problém jak distribuovat odměnu
- obecně jen některá pravidla vedou k akci, která je dobrá / špatná
- pravidla musí dát část své síly, aby byly zahrnuty v cestě k řešení, následně odměněné jen ty zahrnuté

Zero LCS
- žádné vnitřní zprávy
- pravidla jednoduché bitmapy IF THEN
- cover operátor: pokud žádné pravidlo, vygeneruje se náhodné s náhodným výstupem
  - náhodné wildcardy
- odměna jen pravidlům která odpovídají situaci:
  - odpovídá výstup: zvíší se síla
  - neodpovídá výstup: sníží se síla
  - všem se trochu sníží síla
  - těm, co minule odpověděla správně, se rozdělí odměna ze snížená všech
- nevýhody ZCS: 
  - nemá nutkání vyvíjet kompletní pravidlový systém
  - pravidla na začátku řetězu jsou málo odměňovaná a nepřežijí
  - pravidle vedoucí k akcím s malou odměnou nemusejí přežívat

XCS: 
- oddělení fitness od předpokládaného výdělku pravidla
- fitness čistě na přenosti pravidla

- prunning: snaha o co nejgeneričtější pravidla
- niching: jedinci soupeří jen v rámci kategorií (např. podle pravé strany, ...)


Pitt:
- jedinci množiny pravidel
- genetické operátory komplikovanější (na celých množinách), ...
- většinou bohatá reprezentace domén (výčtové typy, intervaly, ..)

- operátory
  - jedinec: výměna pravidel, kopírování pravidel, generalizace, ...
  - na úrovni komplexů: zahrnutí negativního příkladu, rozdělen íkomplexu na 1 selektoru, speicalizace
  - operace na selektorech

Vícekriteriální optimo: 
- místo jedné fitness máme vektor funkcí
- jedinec slabě dominuje jiného, když je lepší/stejný pro všechny fitness funkce
  - jedinci jsou neporovnatelní, pokud se navzájem nedomuníjí
- paretova fronta: množina jedinců, které nikdo nedominuje

- agregovaná fitness: jaké koeficienty vážené sumy pro jednotlivé fitness
- VEGA: utřídím jedince podle každé z účelových fcí, podle každé vyberu N/n jedinců
  - ty křížím a mutuju
  - těžko se udržuje diverzita populace
  - má tendenci kovnergovat k extrémům v jednotlivých fi
- NSGA (non doominated sorting GA): rozdělený podle dominovaných front, beru nejlepší frontu, pak další, ...
  - každý jedinec má niching factor: v rámci fronty jak daleko je od ostatních (čím dál tím lépe)
  - fitness: fitness nejhoršího z předchozí fronty / niching factor
- NSGAII. 
  - řeší absenci elitismu a parametru dshare
  - niching nahrazen crowding distance (součet vzdálenosti k sousedům)

- hypervolume: 
  - hypervolume populace které dominuje pro porovnání jednotlivých populací (tj. algoritmů, prakticky)
  - hypervolume jedince: když část sdílí, tak dostane jen tu podílovou část
- decomposition: rozdělíme prostor na radiální úseky, v každém weigted sum

Kombinatorická optimalizace: 
- NP těžké úlohy
  - batoh: jednoduché kódování, problematická fitness (nelegální ale užitečné stavy)
  - TSP: jednoduchá fitness, těžké operátory & kódování

TSP
- operátory závislé na reprezentaci
- reprezentace sousednsoti: město "j" je na pozici "i" vede-li cesta z i do j
  - každá cesta jen jednu reprezentaci
  - některé reprezentace nelegální cesta
  - klasické křížení nefunguje, schémata naopak ano
- reprezentace bufferem: umožňuje normální 1-bodové křížení
  - buffer a reprezentace, reprezentace říká jaké je další město v bufferu, z bufferu se po použití mažou
  - křížení lze, ale neodpovídá moc realitě
- reprezentace cestou: permutací
  - nefunguje klasické křížení
  - PMX (partially mapped crossover): swap middle points, keep what's not in conflict, use partial mapping to fill the rest
  - OX (order crossover): snaží se zachovat relativní pořadí
  - CX (cyclic crossover): snaží se zachovat absolutní pozici města v cestě
- ER: snaha zachovat hrany z rodičů (předchozí tak max 60 %)
  - každé město má seznam hran
  - začnu někde, vybírám města s méně hranami, případně náhodně
  - stavím cestu
  - vylepšení ER2: Hrany co jsou dvakrát mají prioritu
   
- důležitá i inicializace
  - začni náhodným, pokračuj nejbližším
  - nejbližší město c mimo, najdi k-j, že minimalizuje rozdíl k-c-j a k-j, přivlož do té cesty k-c-j

- mutace Tsp
  - inverze
  - vkládání města do cesty, posun podcesty, záměna 2 měst, heuristiky 2-opt, ...

- další přístupy: 
  - maticová reprezentace
  - kombinace s lokálními heuristikami

Rozvrhování:
- kódování maticí přímočaré
- mutace zamíchej, křížení vyměň učitele předmětů
- hard omezující podmínky, ..

Job Scheduling: 
- zakódování je kritické
- přímá reprezentace lepší než jen permutace pořadí (dekodér pak musí řešit volbu plánů)

Genetické programování: 
- John Koza
- Programy jako syntaktické / sémantické stromy
- terminály proměnné a konstanty
- křížení výměna podstromů, mutace generování podstromu, ...





