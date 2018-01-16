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
- Binárně zakodování jedinci
- Rulettová selekce (pravděpodobnost výběru fitness jedince / fitness všech)
- 1point crossover (vezme se bod, první půlka z prvního, druhá z druhého)
- bitové mutace
- nad nimi postavená teorie schémat

Evoluční programování
- 1965 Owens a Walsh
- evoluce konečných automatů
- smazán rozdíl mezi genotypem a fenotypem
- důraz na mutace, většinou bez křížení
- turnajová selekce (vyber dva (čí víc) náhodně, s nějakou pravděpodobnostní vyber lepšího, else horšího)

Evoluční strategie
- 1964
- optimalizace vektorů reálných čísel (floating point)
- základní operace je mutace (heuristikou / adaptivní)

Genetické progrmaování
- jedinci LISPovské stromy
- nejen evoluce programů, operátory přížení mutace, inicializace
- evolving HW, neuroevoluce, ...

GA
- původní návrh označujeme jako SGA (bitový jedinci, ...)

Schémata:
- řád schématu: počet pevných 0, 1
- definující délka: vzdálenost mezi první a poslední pevnou pozicí
- fitness schématu: průměrná fitness jedinců v populaci
- "Krátká nadrpůměrná s malým řádem se v populaci během GA exponenciálně množí"
  - Schéma ma pravděpodobnost vybrání svojí fitness (nadprůměrná)
  - Čím menší definující délka, tím větší pravděpodobnost přežití křížení
  - Čím menší řád tím, větší pravděpodobnost přežití mutace
- důlsedky
  - na zakódování záleží
  - na vvelikosti záleží
  - předčastná konvergence škodí
- Implicitní paralelismus
  - GA pracuje s m jedinci, implicitně vyvíjí 2^m až n2^m schémat

- GA nesamplují schémata rovnoměrně, ale "z populace". Mohou je odhadnout špatně.

- Explorace: nacházení nových oblastí: nevyužívá předchozí znalosti
- Exploitace: prohledávání aktuálních oblastí: uvíznutí v lokálních minimech

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
  - aritmetické křízení (průměr, konvexní kombinace, ...)
  - zatížená / nezatížená mutace
- permutace, stromy, matice, ... 

Selekce
- ruletová (tradiční pravděpodobnost, "n" hodů)
- SUS: jedna náhodná pozice v ruletovém kole, další posunem o 1/n
  - spravedlivější neb podíl vybraných pro celočíselné podíly vždy odpovídá
- turnajová selekce (vybere se k, nejlepší s pravděpodobností p, druhý s p*(1-p), ...)
  - možné tam, kde fitness není explicitně dána, ale je získaná simulací, např


Evoluce kooperace: 







