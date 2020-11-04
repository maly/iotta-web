---
id: 293
title: Základy číslicové techniky VI
date: 2017-03-11T14:16:43+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=293
permalink: /zaklady-cislicove-techniky-vi/
categories:
  - DIY
  - Hardware
tags:
  - Číslicová technika
---
Teorii kombinačních obvodů máme za sebou, ale hradla ještě neopustíme, kdepak. Tentokrát zjistíme, co se stane, když stvoříme perpetuum mobile. No dobře, přeháním, ale jen trošku...

<!--more-->

## Astabilní klopný obvod

Pamatujete se na staré elektrické domovní zvonky? Tam byl elektromagnet a kovová kotva, která břinkala do toho kovového zvonivého objektu. Jakmile se pustil proud, elektromagnet přitáhnul kotvu - ale tím se zároveň přerušil obvod, takže elektromagnet pustil, kotva se vrátila zpět, ale tím opět sepnula obvod, takže elektromagnet přitáhnul...

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/bell-2-anim.gif" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-294" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/bell-2-anim.gif" alt="" width="250" height="290" /></a>

Výsledek tedy byl ten, že s určitou frekvencí, danou mechanickými vlastnostmi té kotvy, především její setrvačností, střídalo zařízení dva stavy.

A teď myšlenkový pokus, ano? Máme invertor. Víme, že když má na vstupu 0, má na výstupu 1. Když má na vstupu 1, má na výstupu 0. Zkrátka je vždycky v opozici, asi jako ten elektromagnet s kotvou u zvonku... Co kdybychom ho donutili, aby byl v opozici se sebou samotným?! Vytvořili bychom paradox? Zakázaný stav? Nestvořili bychom časoprostorovou trhlinu? Nebo by to fungovalo jako ten zvonek?

No, schválně si to předstvte. Na vstupu je 0. Tedy na výstupu je 1. Tu přivedeme na vstup, takže na výstupu bude 0. Tu přivedeme na vstup, takže na výstupu bude 1. Tu přivedeme na vstup, takže...  Takže se to celé buď ustálí v nějakém stavu "ani ryba, ani rak, logická hodnota 0.5", nebo se to rozkmitá, že?

Bé je správně! Invertor má totiž taky určité dynamické vlastnosti, stejně jako ta kotva, a nepřepíná úplně hned, chvilku mu to trvá. Jakou chvilku? No, u toho starého dobrého TTL invertoru 7400 to bylo okolo 10 ns. Řada 74ALS má okolo 4 ns, řada 74S okolo 3 ns, "rychlá" řada Fast (74F) přepne za 2.5 ns. Nízkoodběrová řada 74L měla zpoždění 33 ns. Ve skutečnosti to bylo ještě složitější, protože zpoždění se lišilo podle toho, jestli se přepíná z 0 do 1, nebo z 1 do 0, ale to můžeme v našem případě zanedbat.

> Většinou nevadí, když do zapojení dáte místo obvodu 74LS00 třeba obvod 74ALS00 nebo 74L00. Nějak to fungovat bude. Ale u složitějších (a rychlých) obvodů může pomalejší kus způsobovat velmi výrazné změny funkce (většinou směrem k horšímu, jak jinak). Někdy to platí i obráceně - že zapojení, které stavíte, buď schválně, nebo v důsledku "race condition", bude fungovat pouze s pomalejší řadou, protože autorovi zpoždění někde k něčemu pomohlo.

Pokud je zpoždění 10 nanosekund, bude frekvence, na které to celé kmitá, rovna 1/t, tedy 100 MHz. Což je hodně moc. Já vím, že s 2.4GHz rádiem a třígigovými procesory v notebooku vám to nepřipadá, ale v číslicové technice, kde se budeme pohybovat my, je vrcholem tak těch 24 MHz pro procesor 8052, ale ještě častěji spíš 16 MHz v Arduinu. Staré procesory běží na 2 MHz, popřípadě 4 MHz. Takže pokud se něco rozkmitá na 100 MHz, věští to problémy.

### Násobky a zlomky

Nanosekundy, megahertzy... Je jasno? Jo? Fakt? No dobře, ale pro jistotu:

Násobky jsou po tisících. Tisíckrát základní jednotka má předponu kilo- (k) - třeba kilometr. Tisíckrát víc je mega- (M), tisíckrát víc giga- (G), a tisíckrát giga je tera- (T). Gigahertzy (GHz) jsou tedy miliardy hertzů (1000 x 1000 x 1000 = miliarda). _Pozor na to, že v angličtině slovo miliarda nemají, tisíc milionů (giga) označují jako billion, zatímco pro nás je bilion tisíc miliard (tera)._

Zlomky jednotek jsou taky po tisících. Tisícina je mili- (m), tisícila z tisíciny je mikro- (µ), tisícina mikra je nano- (n), a z něj tisícina (tedy vlastně bilióntina) je piko- (p)

S čím se v elektronice setkáte? Zkusím to shrnout v tabulce:

<table>
  <tr>
    <th>
      Veličina
    </th>
    
    <th>
      Jednotka
    </th>
    
    <th>
      S čím se setkáte
    </th>
  </tr>
  
  <tr>
    <td>
      Napětí
    </td>
    
    <td>
      Volt
    </td>
    
    <td>
      Volt (V), milivolt (mV)
    </td>
  </tr>
  
  <tr>
    <td>
      Proud
    </td>
    
    <td>
      Ampér
    </td>
    
    <td>
      Ampér (A), miliampér (mA), mikroampér (µA)
    </td>
  </tr>
  
  <tr>
    <td>
      Odpor
    </td>
    
    <td>
      Ohm
    </td>
    
    <td>
      Ohm (Ω), kiloohm (kΩ), megaohm (MΩ)
    </td>
  </tr>
  
  <tr>
    <td>
       Kapacita
    </td>
    
    <td>
      Farad
    </td>
    
    <td>
      Mikrofarad (µF), nanofarad (nF), pikofarad (pF)
    </td>
  </tr>
  
  <tr>
    <td>
       Frekvence
    </td>
    
    <td>
      Hertz
    </td>
    
    <td>
      Hertz (Hz), kilohertz (kHz), megahertz (MHz), gigahertz (GHz)
    </td>
  </tr>
</table>

A pokud jste to ze školy zapomněli, tak Ohm se čte [óm], Hertz pak [herc], a Ampér, ačkoli se celým jménem jmenuje [André Marie Ampère](https://cs.wikipedia.org/wiki/Andr%C3%A9-Marie_Amp%C3%A8re), je mužského rodu - tedy nikoli "ta ampéra". Váš jistič má hodnotu šestnáct ampérů, nikoli šestnáct ampér.



### Co je to to H a L?

Dosud jsem označoval logické úrovně číslicí - 1 a 0. 1 je aktivní, 0 je neaktivní. Někdy se pro značení úrovní používají písmena H a L. H, jako že "high", je ta "vyšší úroveň", tedy logická 1, L je "low", tedy logická 0. V běžném použití lze obě značení volně zaměnit. Svůj význam mají například ve VHDL, kde se jimi značí "tvrdost úrovně", ale to nás, nebojte, až do konce kurzu nebude trápit.

### Astabi-cože?

Stvořili jsme astabilní klopný obvod. **Klopné obvody** jsou obvody, které definovaným způsobem mohou měnit svůj stav v čase. Rozlišujeme tři základní druhy. Klopný obvod _astabilní_ nemá žádný pevný, stabilní stav. Střídá na výstupu 1 a 0 a ignoruje přitom okolí. Druhý typ obvodů je _monostabilní_. Má jeden stálý stav (třeba log. 0), ale na základě nějakého vnějšího vlivu může přejít do druhého stavu (třeba log. 1), z něhož se ale po čase vrátí zpět do toho stálého. No a konečně třetí typ jsou _bistabilní_ klopné obvody. Ty mohou být ve dvou různých stavech, a jakmile v jednom z nich jsou, jsou v něm neomezeně dlouho - až dokud okolí nezpůsobí jejich přepnutí.

Bistabilní obvod bych přirovnal třeba k vypínači od lustru: je buď zapnutý, nebo vypnutý, a setrvává v tom stavu až do doby, než ho vnější vliv nepřiměje stav změnit. Monostabilní obvod v téže analogii funguje jako schodišťové osvětlení: stisknutím rozsvítíte, a po čase samo zhasne.

Astabilní obvody (oscilátory) se používají v číslicové technice často, a to ke generování časových pulsů o nějaké frekvenci. Příklad: chcete si postavit digitální hodiny. Začnete tedy tím, že si postavíte astabilní klopný obvod, který bude generovat pulzy s frekvencí 1 Hz a víte, že co puls, to sekunda. Ve skutečnosti ale použijete obvod, který bude kmitat na frekvenci vyšší (třeba 32.768 kHz) a několikerým dělením získáte kýžený 1 Hz. Proč? Protože pro 32.768 kHz seženete přesnou součástku, zvanou "krystal", která se o frekvenci postará.

Ještě častěji se potkáte s bistabilními obvody. Důvodem je to, že si pamatují svůj stav, což se hodí pro nejrůznější účely, od prostého zapamatování hodnoty po konstrukci složitějších obvodů, jako jsou děličky frekvencí, čítače, posuvné registry a spousta dalších zábavných komponent.

## Bistabilní klopný obvod

Můžeme v našich experimentech se zaváděním zpětné vazby pokračovat. S dvouvstupovým hradlem, kdy do jednoho vstupu zapojíme výstup hradla, a na druhý budeme přivádět log. 1. Zkuste si to, například v simulátoru, anebo si na to přijďte vlastní hlavou. (Součástku smažete tím, že ji přesunete zpět doleva.)

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":true,<br /> "toolbox":[<br /> {"type":"DC"},<br /> {"type":"LED"},<br /> {"type":"PushOff"},<br /> {"type":"PushOn"},<br /> {"type":"Toggle"},<br /> {"type":"AND"},<br /> {"type":"NAND"},<br /> {"type":"OR"},<br /> {"type":"NOR"},<br /> {"type":"EOR"},<br /> {"type":"ENOR"}<br /> ],<br /> "devices":[<br /> {"type":"NOR","id":"dev0","x":216,"y":24,"label":"NOR"},<br /> {"type":"DC","id":"dev1","x":64,"y":16,"label":"DC"},<br /> {"type":"LED","id":"dev2","x":320,"y":24,"label":"Q"},<br /> {"type":"Toggle","id":"dev3","x":128,"y":16,"label":"Input","state":{"on":true}},<br /> {"type":"NAND","id":"dev4","x":216,"y":80,"label":"NAND"},<br /> {"type":"OR","id":"dev5","x":216,"y":136,"label":"OR"},<br /> {"type":"AND","id":"dev6","x":216,"y":184,"label":"AND"}<br /> ],<br /> "connectors":[<br /> {"from":"dev0.in0","to":"dev3.out0"},<br /> {"from":"dev0.in1","to":"dev0.out0"},<br /> {"from":"dev2.in0","to":"dev0.out0"},<br /> {"from":"dev3.in0","to":"dev1.out0"}<br /> ]<br /> }
</div>

[WpProQuiz 6]

Všimněte si, že u obvodů se zpětnou vazbou musíme už brát v úvahu něco, čemu se říká _předchozí stav_. Zatímco dosud jsme se setkávali s obvody, kde se změna neuvažovala, respektive bralo se, že je okamžítá a vždy dopředná, v obvodech se zpětnou vazbou může mít obvod cosi jako "vnitřní stav". To kombinační obvody nemají. Kombinační obvod sám o sobě nemá žádný vlastní stav, a jeho výstup je vždy závislý pouze na stavu vstupů v daném okamžiku. Hezky to uvidíme na následujícím obvodu.

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":true,<br /> "toolbox":[<br /> {"type":"DC"},<br /> {"type":"LED"},<br /> {"type":"PushOff"},<br /> {"type":"PushOn"},<br /> {"type":"Toggle"},<br /> {"type":"AND"},<br /> {"type":"NAND"},<br /> {"type":"OR"},<br /> {"type":"NOR"},<br /> {"type":"EOR"},<br /> {"type":"ENOR"}<br /> ],<br /> "devices":[<br /> {"type":"DC","id":"dev0","x":64,"y":16,"label":"DC"},<br /> {"type":"LED","id":"dev1","x":320,"y":24,"label":"Q"},<br /> {"type":"NOR","id":"dev2","x":232,"y":24,"label":"NOR1"},<br /> {"type":"NOR","id":"dev3","x":232,"y":104,"label":"NOR2"},<br /> {"type":"PushOn","id":"dev4","x":144,"y":16,"label":"Reset"},<br /> {"type":"PushOn","id":"dev5","x":144,"y":112,"label":"Set"},<br /> {"type":"DC","id":"dev6","x":64,"y":112,"label":"DC"},<br /> {"type":"LED","id":"dev7","x":320,"y":104,"label":"Q_"}<br /> ],<br /> "connectors":[<br /> {"from":"dev1.in0","to":"dev2.out0"},<br /> {"from":"dev2.in0","to":"dev4.out0"},<br /> {"from":"dev2.in1","to":"dev3.out0"},<br /> {"from":"dev3.in0","to":"dev2.out0"},<br /> {"from":"dev3.in1","to":"dev5.out0"},<br /> {"from":"dev4.in0","to":"dev0.out0"},<br /> {"from":"dev5.in0","to":"dev6.out0"},<br /> {"from":"dev7.in0","to":"dev3.out0"}<br /> ]<br /> }
</div>

Zkusme si tak nějak z hlavy zjistit stav na výstupu hradla NOR1. Dejme tomu, že vstup, označený Reset, je v log. 0. Co bude na výstupu z hradla? To záleží na hodnotě druhého vstupu, a ten je připojený na výstup hradla NOR2. Jeho hodnota záleží na tom, co je na vstupu Set (dejme tomu, že i tam je 0) a na tom, co je na druhém vstupu... který je připojený na výstup NOR1... který... a jsme v nekonečné smyčce. Ale dejme tomu (_dejme tomu!_), že na výstupu NOR2 je logická 0. Na výstupu NOR1 bude tedy 1 (0 NOR 0 = 1). Tato jednička je přivedena na vstup NOR2. Společně s nulou na vstupu Set dají dohromady nulu na výstupu NOR2 (1 NOR 0 = 0). Obvod je takto stabilní. Q je 1, not Q (označil jsme ho Q_) je 0, oba vstupy jsou nulové.

Co se stane, když na vstup Reset přivedu 1? Na vstupech NOR1 teď bude 1 a 0, výsledek bude tedy 0 a Q se změní na 0. Tato nula se ale přenese i na vstup hradla NOR2, kde spolu s nulou od vstupu Set vytvoří jedničku na výstupu NOR2 (0 NOR 0 = 1). Na výstupu Q_ bude tedy 1. Tato jednička se přenese na NOR1, s jedničkou ze vstupu RESET vytvoří zase nulu, takže se na výstupu nic nemění, a tento stav bude trvat po celou dobu, co bude na vstupu Reset logická 1.

A co se stane, když se Reset teď vrátí do logické 0? Ve světě kombinačních obvodů by platilo, že pro určitou kombinaci vstupních hodnot (Reset = Set = 0) je vždy jen jedna platná kombinace hodnot výstupních. Se zpětnou vazbou to tak úplně neplatí. Chvilku se nad tím zamyslete, třeba si i namalujte, kde jsou ty jedničky a nuly... Už to vidíte?

**Obvod zůstává ve stavu, v jakém byl předtím!**

Zkuste si chování takového obvodu prozkoumat. Co se stane, když přivedete 1 na Set? Co když budou na obou vstupech nuly? Co když budou na obou vstupech jedničky?

<table>
  <tr>
    <th>
      Reset
    </th>
    
    <th>
      Set
    </th>
    
    <th>
      Q
    </th>
    
    <th>
      Q_
    </th>
  </tr>
  
  <tr>
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
      Q<sub>n-1</sub>
    </td>
    
    <td>
      Q_<sub>n-1</sub>
    </td>
  </tr>
  
  <tr>
    <td>
    </td>
    
    <td>
      1
    </td>
    
    <td>
      1
    </td>
    
    <td>
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
    </td>
    
    <td>
    </td>
    
    <td>
      1
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      1
    </td>
    
    <td>
      0(X)
    </td>
    
    <td>
      0(X)
    </td>
  </tr>
</table>

Symbolem "Q<sub>n-1</sub>" označujeme "stav předtím". Pokud je reset i set roven 0, udržuje obvod předchozí stav. Vstup Set nastaví jedničku na výstupu Q, vstup Reset nastaví na výstupu 0. Výstup Q_ je "negovaný Q". Co se stane, když aktivujeme oba vstupy, Set i Reset? Obvod dostaneme do takzvaného "zakázaného stavu", též _hazardního_. Proč hazardní? Představte si, že ze stavu "Set = Reset = 1" přejdou oba vstupy naráz do stavu 0. Co se stane s výstupy? Správně, začnou kmitat. V reálu ale kmitat nezačnou, v reálu se vzhledem k různým nedokonalostem a asymetriím překlopí jeden výstup do 1 a druhý do 0. Který? No, to je právě to, co nelze předvídat, proto "hazardní stav". Konstruktéři se proto snaží důmyslnými zapojeními předejít tomu, aby se na vstupech takového klopného obvodu objevily zakázané hodnoty.

Příště si ukážeme, jak se zakázaných kombinací zbavit, taky si ukážeme, jak říct klopnému obvodu, kdy se smí přepínat, představíme si jednoduchou děličku kmitočtu a taky si řekneme, jaký je rozdíl mezi synchronním a asynchronním...

_Těším se na vás u dalšího pokračování._