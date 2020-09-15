# Mgr. státnice AI/ML 15/09/2020 - Petr Houška

## Obecné faktické informace: 
- 5 otázek z 5 okruhů; 2 povinné, 3 volitelné.
- Na začátku k vám (v náhodném pořadí) přijde postupně 5 lidí z komise a každý vám zadá otázku z jednoho okruhu + řekne pořadí kolikátá vaše otázka to je. 
- Následně k vám (by rules) po (30minut příprava + 30 minut zkoušení) slotech chodí příslušní lidé a zkouší vás danou otázku, kterou vám zadali. Občas si k nim přisedne někdo další a je co-zkoušející. Prakticky chodí jakmile se přihlásíte + oni jsou available, celé státnice jde stihnout daleko dřív než za 5 hodin. 
- Míra kterou si čtou papír vs. kolik si toho nechají ústně říct vs. kolik je debata extrémně záleží, jenom já měl případy plně papír i plně povídání.
- Výsledky jsme dostali asi 10 minut po dozkoušení nejpomalejšího člověka.

## Random notes:
- Nedostali jsme erární papíry, asi si o ně šlo říct, ale doporučuju si přinést svoje a radši víc. Dva papíry na otázku není vůbec špatný nápad.
- Při zkoušce šlo normálně chodit na toaletu, pít, jíst. Celkově to mělo opravdu příjemný vibe. Naprosto neironicky to bylo ve u všech otázek spíš příjemné povídání než stresové zkoušení.

## Obecné shrnutí: 
- Všechny zkoušení byly super příjemný. Jako velmi dobrá strategie mi přišlo umět opravdu hodně věcí hodně high level a ukázat to. A pak pár věcí (ty které člověk sám chce) rozvést trochu víc. IMHO tím šlo velmi efektivně předejít tomu, aby se zkoušející zeptal na nějaký detail, který zrovna není úplně jasný. 
  - Žádný z mých orkuhů nebyl ani omylem zkoušen do úrovně jako na zkoušce daného předmětu. 
  - Obecně mi přišlo, že úroveň (nejen) mých poznámek z mff-stuff se všemy důkazy zapamatovanými poze jako hrubá high-level idea (i.e. savich: dynamické programování + počet konfigurací; splay: jak je cca definovaný potenciál + teleskopická suma a jednička u jednorotace) je postačující na pohodlnou jedničku, a ani člověk nemusí umět všechno. Já uměl tak 90 %, odhadem. Cokoliv hlubšího se mi zdá že by bylo úplně zbytečné.
  - That being said, IMHO se tomu vyplatí high-level opravdu rozumnět, nejen to umět odpapouškovat. A tu a tam to demonstrovat skrze nějaké spojení. 

## Multiagentní systémy [R. Barták]: Aukce
- `./1_1.jpg`, `./1_2.jpg`

Hned při zadávní otázky jsem se ptal, jestli to můžu rozšířit, protože mi nepřijde, že se toho o aukcích dá tolik říct. Bylo mi řečeno, že se toho o nich dá říct spoustu (různé typy, jejich problémy, mitigace, ...), což mě trochu znervóznilo.

Zkoušení začalo otázkou do jaké širší kategorie aukce patří. Absolutně jsem netušil kam otázka míří, což jsem zkoušejícímu také přiznal. Snažil jsem mne navést, já místo toho říkal věci jako multi-agentní systémy (ano, ale trochu míň široké), utility theory (adjacent, but not really). Nakonec jsme se dostali k tomu, že v aukcích nastavujeme pravidla a zkoušející se otázkou jestli jsem odchodil AI2 (moje odpověď že jo, ale už 3 roky zpátky byla přijata bez problémů) prozradil, že chtěl termín "reverzní teorie her/mechanism design". 

Následně jsem začal popisovat různé typy aukcí viz papíry. Hned zkraje jsem přiznal, že mám hrozně špatnou paměť na jména/typy a že jsem je možná zmixoval. Zkoušející vypadal, že to chápe a že to nebude problém. Hned první Anglickou aukci jsem ovšem trefil názvem správně, z čehož jsme oba měli radost. U anglické jsme se trochu zasekli, když jsem říkal, že by teoreticky mohla fungovat i s druhým nejvyšším bidem, což zkoušející oponoval, že to pak není anglická aukce. Shodli jsme se na tom, že by to nedávalo moc smysl a nebyla to anglická aukce, ale mohlo by to existovat. 

U obálka/platí se nejvyšší bid se zkoušející ptal, jaká je optimální strategie. Já uvedl, že to na papíře, zkoušející oponoval, že ne. Já si stál za svou a snažil se rozebrat proč, s tím, že nevím kam míří. Zkoušející uvedl příklad 4 normálních lidí + superbohatého (let's say J. Bezose). Já pořád nechápal kam míří a stál za svou. Zkoušející mi napověděl tím, že Bezos by mohl vědět očekávanou utilitu ostatních lidí a bidnout secondBest + e. Na to jsem přiznal, že jsem měl přepodklad neznání utility ostatních, což byl implicitní a špatný předpoklad, který jsem mít neměl. A že s ním to je samozřejmě jinak. Následně jsem rozebral jak toho ovlivňuje ostatní typy. U anglického uvedl, že tam se zas u reálných lidí projevuje efekt soutěživosti, který vede k placení víc než subjektivní utilita + šroubování ceny + explicitní sdílení informací, atd.. Zdálo se, že zkoušející je spokojený s diskuzí.

Následně jsme se přesunuli k teorii her, high level jsem definoval dominovanou/dominující strategii, pareto (ne)optimum, vše v kontextu vězňova dilema. Pak jsem dostal otázku na obecní pastvinu, definici problému a případné řešení. Uvedl jsem paralelu s reálným světem, skrz to jsem i přivedl řešení v podobě z-explicitnění externalit (daně/povolenky/...). Dostal jsem otázku jak to nacenit. Na to jsem uvedl, že to je komplexní a multidimenzionální problém a že záleží na co přesně optimalizujeme. Zkoušející navrhl aukce, já lehce oponoval s tím, že jet čistě monetárně - pokud jsme v kontextu reálného světa - nemusí být jediný ideální přístup; znovy vypadalo, že je zkoušející spokojený s diskuzí. Nakonec byla otázka na to, jaký typ aukce by byl nejlepší pro podobné cenění veřejných zdrojů, aby nebylo jisté, že je ziská např. Bezos. Zkoušející se znovu snažil hintovat na obálkové aukce s 2. nejvyšší cenou, kde je optimální bid pravou utilitu. Skrze mé nepochopení k čemu tato otázka vede jsme se dostali k zásadní nelinearitě užitku majetku. 

I přes relativně hodně momentů, kdy jsem nevěděl co po mě zkoušející přesně chce to vypadalo, že je velmi spokojený s diskuzí a obecně výsledkem. A i za mě to bylo extrémně příjemné povídání.

## Přírodou inspirované počítání [R. Neruda]: Genetické algoritmy
- `./2_1.jpg`

Při zadávání otázky jsem se zeptal jestli mám dojít až k schématům/analýze přes markovovi řetězce. R. Neruda poznamenal, že to očividně vím a že teda nemusím :D. 

Když jsem ho pak odchytil, tak jsem víceméně jen odpřednesl, co mám na papíru. Trochu jsem si nebyl jistý, jestli se fitness proporční selekce dělá jen u enviromentální nebo i u rodičovské, zmínil jsem že nevím a implikace either-way. Oproti papíru jsem víc rozvedl mutace lamark/baldwin, u selekce trochu víc niching (vzpomenul NEAT/CoDeepNeat). Nakonec jsem high level zmínil sechémata, jak to souvisí s building blocks, a že building blocks je jen kódování-dependant assumption (headless-chicken experiment); ale že i když neplatí tak meta-mutace není k zahození. 

Nakonec jsme si chvíli obecně povídali o pokračování schémat v jiných kódování (já spíš poslouchal, upřímně :)), úskalích rigorozní analýzy přes  mark. řetězce (neupočítatelné), a já se dozvěděl o promising přístupech skrze analýzu vlastností populací z fyzikálního simulování/termodynamiky. Zkoušející odcházel velmi spokojený, já zůstával sedět také spokojený a obohacený o nové poznatky :).

## Základy složitosti a vyčíslitelnosti [A. Kučera]: NP úplné problémy
- `./3_1.jpg`, `./3_2.jpg`

Po cca 15 minutách ke mě přišel, jestli už může zkoušet. Byl jsem zrovna v půlce psaní high-level verze Lewin-Cook, ale řekl jsem že ok. Přečetl si, co jsem napsal (viz přiložené fotky). Trochu se nevyznal v pořadí mých definicící, takže jsem mu jenom řekl kde co mám, a že nadpis nepatří k první definici NP třídy. Když došel k Lewin-Cook, tak podotkl, že tohle jsem fakt psát nemusel. 

Projel mnou zmíněné NP úplné problémy, zeptal se co 2SAT. Já se snažil nějak kloudně říct, že nevíme, ale že neznáme převod a tak, že nejspíš NP úplný není. Trochu jsme se nechápali, já pak dodal, že má polynomiální algoritmus. Chtěl jsem dodat, že možná převod z 3SAT na 2SAT existovat může, ale kdyby existoval, tak P=NP. Než jsem to stihl pořádně vysvětlit, tak jsem dostal otázku jak funguje 2SAT poly alg, přiznal jsem, že přesně nevím, ale že je to víceméně graf ve kterém hledáme cestu, kde jsou propojené literály. Na to vzal pan Kučera propisku, načrtl mi jak přesně to funguje, dodal, že prý je na to i skoro lineární algoritmus skrze heuristiky atd. Zdálo se, že mi to říká jen jako zajímavost a odcházel naprosto spokojený.

## Datové struktury [V. Majerech]: Haldy (d-regulární, binomiální)
- `./4_1.jpg`, `./4_2.jpg`, `./4_3.jpg`

Zkoušející si přečetl co jsem napsal (IMHO jsem jen dodal jak by se dělal increase/decrease u d-regulární), u binomiálních/fibonaciho hald se zeptal na udržování spojového seznamu lesa, kdy to můžu dělat. Já se moc nezamyslel a řekl, že mohu projít pole a nastavit korektně pointery po konci heap-fix. Bylo mi vysvětleno, že to zabije amortizaci n*Insert na O(1) a jak to dělat správně; nezdálo se, že to bylo bráno jako velký problém. 

Pak si všiml mé poznámky, že existují varianty s garantovanou složitostí, k tomu jsem dodal, že se ale nepoužívají protože špatné konstanty. Zkoušející se začal ptát jaké přesně operace to garantuje lepší worst case. Přiznal jsem se, že upřímně netuším; buď decrease nebo i extract-min, ale že si to pamatuju jen jako poznámku pod čarou. Trochu jsem se začal bát, ale zkoušející okamžitě začal popisovat svůj paper na Haldy, které právě tohle řeší s údajně velmi dobrými konstantami. To nakonec zabralo víc času než samotné zkoušení, ale zas jsem se dozvěděl něco nového (vcelku theme mých státnic); zkoušející odcházel zřejmě spokojený.

## Representace znalostí [O. Čepek]: Formální systémy, logika 1. řádu, jazyk, axiomy, odvozovací pravidla.
- `./2_1.jpg`

Začal jsem víceméně 1:1 přeříkávat co mám na papíře, zkoušející si to u toho vcelku pečlivě pročítal. Záhy si přisedl ?P. Kučera. U funkčních/relačních symbolů jsem nad rámec papíru chtěl zmínit, že můžeme nahradit funkční symboly skrz n+1-ární relaci. Okamžitě jsem byl P. Kučerou (asi?) upozorněn, že to úplně obecně nefunguje a že tam musí být zásadní omezení na použití této substituce. Nezdálo se, že by to byl zásadní problém. Oproti papíru jsem trochu víc rozvedl že důkazy jsou čistě syntaktické a jak model do FO zavádí sémantiku. U FO vs PL jsem nevěděl, jak se jmenuje ona věta o modelu konečné podmnožiny grounded verze, vůbec to nevadilo; zdálo se, že je pozitivní, že jsem ji vůbec zmínil. Zkoušející odcházel spokojený.


## Results: 
Nevím dílčí výsledky, ale za 1. Naše skupina byla 6x 1 a 1x 2, jeden člověk nedorazil. 