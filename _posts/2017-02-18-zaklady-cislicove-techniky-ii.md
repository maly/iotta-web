---
id: 213
title: Základy číslicové techniky II
date: 2017-02-18T16:47:04+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=213
permalink: /zaklady-cislicove-techniky-ii/
image: http://iotta.cz/wp-content/uploads/sites/17/2017/02/voltage_char.png
categories:
  - DIY
  - Hardware
tags:
  - Číslicová technika
---
Tento díl představuje drobnou odbočku. Obsahuje techničtější informace pro ty, které zajímá, jak je vlastně takové hradlo nebo invertor udělané uvnitř, jak je poskládané z těch tranzistorů či čeho, jaký je rozdíl mezi TTL a CMOS, co se děje, když se něco děje, proč se řídicí signály často používají negované, proč se k obvodům zapojuje odrušovací kondenzátor&#8230; **Není to pro naprosté začátečníky**, minimálně je potřeba vědět, jak funguje tranzistor.

<!--more-->

Tranzistor je prvek, který je vlastně základním stavebním kamenem celé číslicové techniky. Na rozdíl od analogové, kde máme tranzistory rádi, protože zesilují slabé signály, tak v číslicové spíš využíváme toho, že tranzistor se za jistých podmínek sepne pro procházející proud, a to podle proudu, který prochází bází.



Tohoto principu využijeme a sestavíme si invertor:



Princip je jednoduchý: Když je na vstupu logická 0 (L), tedy vstup je spojen (více méně) se zemí, je tranzistor zavřený, a proud teče přes rezistor 640 ohmů z napájecího napětí na výstup. Jakmile na vstup přivedeme log. 1 (H), tranzistor se otevře a spojí výstup se zemí. Na výstupu bude tedy logická 0.

Hradlo NAND bude na podobném principu:



Pokud tranzistory zapojíme paralelně, namísto sériově, získáme hradlo NOR.

Takovéhle logické elementy opravdu existovaly. Říkalo se jim RTL &#8211; Resistor-Transistor Logic &#8211; a byly použité například v naváděcím počítači Apolla. Měly ale obrovskou spotřebu (podívejte se, jaké proudy tečou hradlem NAND, když jsou oba tranzistory otevřené), nebyly moc odolné proti rušení atd. Vývoj proto přinesl další technologii, a tou byla **TTL**, neboli transistor-transistor logic.



Výhodou TTL byla nižší spotřeba a větší rychlost. Nevýhodou pak to, že jste potřebovali výrazně vyšší počet tranzistorů na logickou funkci. Na výše uvedeném invertoru je to vidět &#8211; tranzistor úplně vlevo, připojený emitorem ke vstupu (Q1), slouží ke spínání centrálního tranzistoru (Q2). Pokud je vstup na nízké logické úrovni, teče proud skrz první tranzistor ven (ano, teče proud ze vstupu&#8230;) Při napájecím napětí 5V a omezovacím odporu R1 o velikosti 100k jde o necelých 50 uA. Centrální tranzistor Q2 je tak uzavřen, a proud teče přes rezistor R2 do báze tranzistoru Q4. Ten je tak otevřen, a na výstup je přes R4 a D1 přivedeno napájecí napětí. Q3 je uzavřen díky odporu R3 v bázi.

Pokud na vstup přivedeme logickou 1 (H), dostane se toto napětí na bázi Q2, který se otevře. Proud nyní putuje přes R2 a Q2 do báze Q3. Q4 je tedy zavřený, Q3 otevřený, a výstup je propojen přes něj se zemí.

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/02/a27_f1.png" rel="lightbox"><img loading="lazy" class="aligncenter wp-image-217 size-full" src="http://iotta.cz/wp-content/uploads/sites/17/2017/02/a27_f1.png" width="500" height="245" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/02/a27_f1.png 500w, https://iotta.cz/wp-content/uploads/sites/17/2017/02/a27_f1-300x147.png 300w" sizes="(max-width: 500px) 100vw, 500px" /></a>

Toto řešení výstupu pomocí dvou tranzistorů, rezistoru a diody se nazývá též _totem_. Připomíná Darlingtonovo zapojení, které dovoluje spínat velké proudy. Díky tomu lze výstup zatížit poměrně velkou zátěží. U standardní řady TTL lze na jeden výstup hradla připojit až deset vstupů.

### Blokovací kondenzátory

Nevýhodou totemu je, že spoléháme na nekonečnou rychlost přepínání tranzistoru Q2. V ideálním případě je tento tranzistor buď plně otevřen (a Q4 je tedy zavřený, Q3 otevřený), nebo plně zavřen (Q4 otevře, Q3 zavře). Jenže reálné obvody vykazují pomalejší přechod, daný převážně fyzikálními vlastnostmi. Na krátký okamžik se tak při přepínání stane, že budou otevřeny oba tranzistory v totemu, jak Q3, tak Q4, a poteče přes ně proud, omezený jen rezistorem R4. V praxi se to projeví rušivými pulsy, kdy je zvýšený odběr z napájení. Proto se připojuje k napájení obvodů TTL paralelně kondenzátor o kapacitě ~100nF, který tyto špičky pokryje. Zároveň z toho vyplývá, že _čím větší je přepínací frekvence, tím vyšší je odběr obvodu_.

Při napájecím napětí +5V jsou definované logické úrovně tak, že 0 až 0.8V na vstupu je logická 0, 2V až 5V je logická 1. To &#8222;někde mezi tím&#8220; je hazardní oblast, v níž se nedá předvídat, jak se bude součástka chovat, navíc může docházet k výše popsanému &#8222;polootevření&#8220;, proto je lepší se takovým napětím vyhnout. Hradla TTL mají na výstupu 0 až 0.4V pro logickou 0, 2.4V až 5V pro logickou 1, což tedy nechává ještě dostatečnou rezervu pro ztráty a rušení.

### Negované signály

Možná jste si říkali, proč jsou některé signály v číslicové technice negované. Typicky třeba signál RESET a signály řízení sběrnice bývají navržené tak, že jsou aktivní v log. 0, a v klidovém stavu jsou na log. 1. Důvodem je rušení. Představte si, že se ve vedení indukuje rušivé napětí, a tak místo 5V tam je vlivem rušení třeba o 1.5V méně, tedy 3.5V. Pokud je signál navržený jako invertovaný, nevadí to, stále se vejdeme do dovoleného pásma (2V-5V). Pokud bychom ale měli signál v klidu na log. 0, tak by 1.5V rušivého napětí znamenalo, že se signál dostane mimo bezpečnou oblast (0V-0.8V), a obvod takový stav může vyhodnotit jako aktivní impuls. Zkrátka díky tomu, že logická 1 má mnohem větší pracovní pásmo, je výhodnější u signálů, které mají být po většinu doby v klidu a sepnout jen někdy, použít invertovanou hodnotu a nadefinovat klidový stav jako 1.

## MOS, CMOS

Až do této chvíle jsme si popisovali obvody, nazývané jako &#8222;bipolární&#8220;. To proto, že používají takzvané _bipolární tranzistory_ NPN. Existuje ještě druhá technologie, která má některé velmi zajímavé vlastnosti, a tou technologií je MOSFET. FET znamená Field-Effect Transistor, MOS odkazuje na jeho uspořádání: Metal-Oxid-Semiconductor. Na rozdíl od bipolárních tranzistorů, řízených protékajícím proudem, jsou tranzistory MOS řízeny napětím. V praxi to znamená, že vstupem takového obvodu teče téměř nulový proud a chová se tedy jako by měl (téměř) nekonečný odpor.



Analogicky s výše uvedeným příkladem si můžeme navrhnout invertor pomocí jednoho N-MOS tranzistoru:



N-MOS se spíná pozitivním napětím řídicí elektrody (gate) proti společné (source). Technologie P-MOS to má přesně obráceně. Technologie MOS má mnoho výhod, a jednou z nich je to, že se na stejnou plochu křemíkového čipu vejde víc logiky. Proto se obvody TTL dělaly nanejvýš ve středním stupni integrace (MSI &#8211; čítače, dekodéry, multiplexery). Vyšší stupeň integrace (LSI, VLSI), potřebný pro složitější obvody včetně mikroprocesorů, vznikaly technologií MOS. Nejprve šlo o P-MOS, ale později přišla rychlejší NMOS, bohužel s vyšším odběrem.

Pokud se v logickém prvku zkombinovaly obě tyto technologie, vznikl CMOS (Complementary MOS).



Vidíte, že díky použití dvou rozdílných tranzistorů, které jsou zapojené &#8222;proti sobě&#8220;, získáváme téměř ideální invertor, kterým v klidovém stavu neteče téměř žádný proud. Spotřeba je tedy nula nula nic. Ovšem &#8211; jen teoreticky. V praxi je vlivem technických nedokonalostí a parazitních kapacit situace spíš takováto:



Při přepínání krátkodobě vyskočí proud protékající obvodem. Na druhou stranu oproti hradlům TTL je to vysloveně nic. Obvody CMOS (řady 40xx) mají proti TTL mizivou spotřebu, mohou pracovat v širokém rozmezí napájecího napětí, ale na druhou stranu jsou pomalejší a jejich úrovně jsou nekompatibilní s TTL.

Pokud obvody CMOS napájíme 5V, je pro ně logická 0 na vstupu v rozmezí 0V až 1.3V. Logická 1 je 3.7V až 5V. Na výstupu je v log. 0 maximálně 0.2V, v log. 1 pak minimálně 3.7V. Vstupy CMOS mají hodně velké &#8222;zakázané pásmo&#8220; (1.3V až 3.7V), a střední hodnota (prahové napětí pro přepnutí) je okolo 2.5V &#8211; tedy už v oblasti, kde je TTL logická 1.

Naštěstí v dnešní době technologie pokročila dopředu., a dnešní moderní CMOS jsou mnohem rychlejší, a třeba řada HCT má napěťové úrovně kompatibilní s TTL a představuje tak reálně &#8222;to nejlepší z obou světů&#8220;.

## 5V, 3.3V atd.

Technika šla dopředu a napájecí napětí se snižovalo a snižovalo. Bohužel to vede k tomu, že musíme stále přemýšlet: bude tento obvod kompatibilní s jiným? Následující obrázek ukazuje možné kombinace různých technologií a jejich napětí.

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/02/voltage_char.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-220" src="http://iotta.cz/wp-content/uploads/sites/17/2017/02/voltage_char.png" alt="" width="743" height="403" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/02/voltage_char.png 743w, https://iotta.cz/wp-content/uploads/sites/17/2017/02/voltage_char-300x163.png 300w" sizes="(max-width: 743px) 100vw, 743px" /></a>

## Vdd, Vcc, Vss, Vee, Gnd

Všechno toto jsou zkratky pro napájecí napětí v číslicové technice. U technologie TTL se pro napájecí napětí používá označení Vcc (C &#8211; collector, česky kolektor), zem je pak analogicky Vee (Emitor). Častěji se ale označuje jako GND. U obvodů CMOS je situace analogická, ale místo kolektoru máme kladné napětí na elektrodě &#8222;drain&#8220; (Vdd) a nulové (zem) na &#8222;source&#8220; (Vss).

## Anketa

Prosím, najděte si minutu času a [odpovězte na pár otázek](https://docs.google.com/forms/d/e/1FAIpQLSeJTL1lzI7_5rh9Xp2Yws4PB81fTX7qBZMnFdw0DYDKaJOdlg/viewform?entry.525375248), které mohou ovlivnit další podobu kurzu. Děkuju.