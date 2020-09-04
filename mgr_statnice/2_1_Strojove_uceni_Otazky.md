# Strojové učení a jeho aplikace
[wiki súbor starších otázok](http://wiki.matfyz.cz/wiki/St%C3%A1tnice_-_Informatika_-_Zkazky_/_zku%C5%A1enosti)

Strojové učení; prohledávání prostoru verzí, učení s učitelem a bez učitele, pravděpodobnostní přístupy, zpětnovazební učení, teoretické aspekty strojového učení. Evoluční algoritmy; základní pojmy a teoretické poznatky, hypotéza o stavebních blocích, koevoluce, aplikace evolučních algoritmů. Strojové učení v počítačové lingvistice. Pravděpodobnostní algoritmy pro analýzu biologických sekvencí; hledávání motivů v DNA, strategie pro detekci genů a predikci struktury proteinů.

## Otázky

### 9.7.2020 - Pilat
Povedal, ze by to chcelo nejaku zabavu, tak mi dal zpetnovazobne. Este potom dodal, ze dufa, ze viem :) Spisal som tam cca to, co je v Bartakovych slidoch. Islo mu hlavne o to, aby som popisal vzorceky updatov a myslienku a potom sa ma pytal, ci tomu rozumiem.

### 5.2.2020 - Vomlelova
Reinforcement learning
* definice ulohy
* pasivni vs. aktivni agent
* ADP, TD
* SARSA, Q-learning

### 17.9.2019 - Vomlelova
Spatnovazebne ucenie
Tuto otazku som nevedel... Trochu sme sa snazili najst spravnu odpoved ale viac menej marne.

### 11.6.2019 - Mrazova + Fink
Aplikace evolučních algoritmů v ML
Rovnou jsem Finkovi říkal když mi to zadával, jestli není blbý, že mám evu jako zvlášť obor, ale prý to nevadí a otázku mi nechal. Sepsal jsem high level pár použití, jako neuroevoluce (váhy/architektura, NEAT), hyperparametry, architecture search, LCS (michigan/pitt), kombinatorická optimalizace, apod, ale ústní pak bylo spíš jenom taková high level diskuse. Řešily se věci jako fitess, kdy jakou použiju, kdy nastane problém. Nic záludného.

### 11.9.2017 - Vomlelova
Strojové učení (Vomlelová) - Učení s učitelem. Po otázce co konkrétněji by si tak představovala bylo upřesněno na rozhodovací stromy a prořezávání. Zjistil jsem, že GINI index se nepoužívá jako míra chyby, ale jako alternativa ke Gainu, tj. pro výběr atributu ke štěpení. Taky jsem zjistil, že Cost complexity prunning a CART nejsou dva různé algoritmy prořezávání, nýbrž jeden a ten samý. Nicméně i přesto jsem prošel na výbornou, Vomlelová byla nadšená už jen z toho, když spatřila vzorec pro Cost complexity prunning (a náladu jí zjevně nezkazilo ani to, že jsem to celé vlastně chápal úplně blbě).

### 8.6.2017 - Vomlelova
Teoretické aspekty ML: nejdřív se mě zeptala, co si pod tou otázkou představuju (k-fold crossval, train/test split, bias-variance) a pak doplnila svoje (overfitting, prokletí dimenzionality). Ke každému jsem něco krátce neformálně řekl nebo nakreslil a padlo pár dotazů. Např. proč se chyba nedá dekomponovat jenom na bias a variance (chybí ještě šum) nebo proč k-fold crossval může dávat špatné výsledky. Proč je nutné mít train/valid/test split, jak je to s časovo náročností třeba u LOOCV atd

### 8.6.2017 - Vomlelova
Pravděpodobnostní přístupy v ML: Musel jsem se doptat, co chtěla paní Vomlelová pod touto otázkou slyšet, na což mi bylo řečeno že chce MAP a maximum likelihood napsat vzorečky. A potom že si mám připravit něco z klasického supervised learningu, protože si myslí, že to nebude stačit. Zkoušení bylo vcelku důkladné a vydalo se směrem do hloubky pochopení látky (např. které se používá, zda MAP nebo maximum likelihood a podobné otázky), přes to jsme se ale nakonec probourali k supervised learningu, kde jsem mluvil o bias-variance trade-off, také parametrické a neparametrické metody (tady chtěla slyšet příklady z obou skupin metod). Připravil jsem si i odvození naive bayes, které zkoušející prošla bez větších komentářů - usuzuji, že to nebylo to, na co se chtěla při zkoušení zaměřit. I když zkoušející vypadala, že by nejradši ještě něco slyšela, nakonec to stačilo. (Celková známka byla 1, takže to stačilo nejspíš velmi dostatečně).

### 13.9.2017 - Mrazova
Evoluční algoritmy a věta o schématech (Mrázová)

Tak tady mě celkem překvapilo, že jsem dostal otázku z EVY, když jsem na ní měl i zaměření (5)). Ale vlastně tam byl nějakej zmatek ohledně novýho oboru a původně jsem měl ve složce ze studijního uvedený špatný okruhy, tak možná proto.
Každopádně mě tohle zkoušení trošku zklamalo, zamotali jsme se totiž do nějakých technikálií ohledně elitismu (jestli po okopírování elitních jedinců generovat jen tolik nových jedinců, aby byla populace pořád stejně velká, nebo jestli jich generovat víc a pak nějakým způsobem odstraňovat pár přebytečných). Každopádně to byly věci, který se u Nerudy vůbec neřešily a nevím, jestli mi to nesnížilo známku u triviální otázky...

