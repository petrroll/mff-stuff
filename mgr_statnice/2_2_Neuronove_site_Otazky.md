# Neuronové sítě
[wiki súbor starších otázok](http://wiki.matfyz.cz/wiki/St%C3%A1tnice_-_Informatika_-_Zkazky_/_zku%C5%A1enosti)


Neurofyziologické minimum. Modely pro učení s učitelem, algoritmus zpětného šíření, strategie pro urychlení učení, regularizační techniky a generalizace. Asociativní paměti, Hebbovské učení a hledání suboptimálních řešení, stochastické modely. Umělé neuronové sítě založené na principu učení bez učitele. Modulární, hierarchické a hybridní modely neuronových sítí. Genetické algoritmy a jejich využití při učení umělých neuronových sítí.

## Otázky

### 9.7.2020 - Pilat
Kohonenky. Pisal som o motivacii, nejaky nacrt, ako to vyzera, klobucikovu funkciu (tu sa ma pytal, ako ju vytvorit - dozvedel som sa, ze rozdielom Gausov) a cca nacrt dokazu. Este som mu nanutil zaklad Ojovho alg, ale to ho uz moc nezaujmalo.

### 5.2.2020 - Mrazova
Učení s učitelem
* učení perceptronem
* dukaz konvergence u perceptronu
* backprop
* odvozeni vah pro vystupni vrstvu
* generalizace a regularice

### 17.9.2019 - Pilat
Spatna propagacia, strategie pre urychlenie ucenia
Ukazal som ako funguje algoritmus spatnej propagacie aj s odvodenim vzorcov na upravu vah, zo strategii som spomenul vhodne zvolenie vah, uciace konstanty, momentum

### 11.6.2019 - Mrazova + Fink + dalsi
Samoorganizace, Kohonenovy mapy, Učení bez učitele
Tady trochu začala sranda, protože jsem neuronky neudělal, tak jsem znal akorát základy jak Kohonen mapy fungují (ani jsem si nevzpomenul na hierarchické, ale nevadilo), tak jsem popsal jak to funguje, jak se to trénuje, podmínky pro SGD konvergenci, a pak nějaké obecné u unsupervised (PCA, Apriori, clustering). Zkoušení bylo docela detailní, řešilo se hlavně high level jak to funguje, co když tam mám a nemám laterální spoje, co když se mi ta dečka zamotá do sebe (motýl), čemu to vadí, proč to nechceme, apod. Taky docela důraz na dimenze, kde co je jak velký a proč (žádná záludnost, jen věci typu <a,b> musí mít oboje stejně velké apod).

### 11.9.2017 - Mrazova
Perceptron (včetně důkazu konvergence učení). Mám pocit, že tedy nějak zafungovalo asociační učení a Mrázová si můj ksicht spojila s tématem perceptronového učení. Tohle bylo totiž potřetí (a naposledy :D ) za mé studium na matfyzu, co jsem u ní zodpovídal tuhle otázku (data mining, neuronky, státnice).

### 8.6.2017 - Bozovsky
Hledání suboptimálních řešení: Tady vlastně nevím, co jsem měl říct, protože hledání subopt. řešení se chceme typicky vyhnout. Takže jsem ukázal, že backprop se může zaseknout v lokálním minimu a že momentum tomu může zabránit (tady chtěl vzorečky na gradient descent s momentem, ale ne vlastní backprop natož jeho odvození). Dál jsem zmínil nějaké augmentace trénovacích dat a náhodné perturbace vah neuronů. Dál jsem zmínil problém lokálních optim u hopfielda a že se to dá řešit pomocí simulated annealing (tady byl potřeba vzoreček a vysvětlení jak funguje) -> Boltzman machine (zmínka o RBF, ale jen malá). Nakonec jsem zmínil SOM a problém mrtvého neuronu, načež se mě zeptal, jak to řešit a jak inhibiční funkce a velikost okolí ovlivňuje mrtvé neurony. Trochu jsme se v tom patlali, ale nakonec v poho.

### 8.6.2017 - Bozovsky
Backprop: Poslední mé zkoušení a jedno z posledních toho dne, zkoušející mi zadal tuhle otázku a při zkoušení jsme se bavili hlavně o principu, jak funguje backprop, proč děláme derivaci, rozdíly mezi lokální a globální chybou, proč může error růst při učení apod. Při přípravě jsem napsal error funkci, ta se sešla, a také nějaké odvození vah, ale to ho vůbec nezajímalo, šel spíše po principu a pochopení.

### 13.9.2017 - Mrazova
Asociativní paměti a její stochastické modely (Mrázová)

Nejdřív jsem si nebyl jistej, co se zadáním myslí, ale šlo (kromě AM obecně) zejména o popsání Hopfielda a jeho stochastických variant (varianta se sigmoidou, Boltzmannův stroj (to je varianta se sigmoidou s náhodnou aktivací na základě pravděpodobnosti místo využití pravděpodobnosti samotný), simulované žíhání). Prostě prakticky přesně podle ppt, případně pdf prezentace :)
