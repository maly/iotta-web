---
id: 222
title: Základy číslicové techniky III
date: 2017-02-19T13:33:50+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=222
permalink: /zaklady-cislicove-techniky-iii/
image: http://iotta.cz/wp-content/uploads/sites/17/2017/02/3_14.jpg
categories:
  - DIY
  - Hardware
tags:
  - Číslicová technika
---
Jak je z jiskřičky pionýr a z pionýra svazák, tak je z hradla sčítačka, ze sčítačky ALU, a z ALU celý procesor... Pokračujeme ve spanilé jízdě světem číslicové techniky.

<!--more-->

[Minule](https://iotta.cz/zaklady-cislicove-techniky-i/) jste dostali domácí úkol: vytvořit hradlo XOR ze čtyř hradel NAND. Myslím, že to nebylo tak těžké:

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":false,<br /> "devices":[<br /> {"type":"LED","id":"dev0","x":368,"y":168,"label":"LED"},<br /> {"type":"DC","id":"dev1","x":32,"y":144,"label":"DC"},<br /> {"type":"Toggle","id":"dev2","x":88,"y":144,"label":"Toggle","state":{"on":false}},<br /> {"type":"Toggle","id":"dev3","x":88,"y":200,"label":"Toggle","state":{"on":false}},<br /> {"type":"DC","id":"dev4","x":32,"y":200,"label":"DC"},<br /> {"type":"NAND","id":"dev5","x":304,"y":168,"label":"NAND"},<br /> {"type":"NAND","id":"dev6","x":168,"y":168,"label":"NAND"},<br /> {"type":"NAND","id":"dev7","x":232,"y":192,"label":"NAND"},<br /> {"type":"NAND","id":"dev8","x":232,"y":152,"label":"NAND"}<br /> ],<br /> "connectors":[<br /> {"from":"dev0.in0","to":"dev5.out0"},<br /> {"from":"dev2.in0","to":"dev1.out0"},<br /> {"from":"dev3.in0","to":"dev4.out0"},<br /> {"from":"dev5.in0","to":"dev8.out0"},<br /> {"from":"dev5.in1","to":"dev7.out0"},<br /> {"from":"dev6.in0","to":"dev2.out0"},<br /> {"from":"dev6.in1","to":"dev3.out0"},<br /> {"from":"dev7.in0","to":"dev6.out0"},<br /> {"from":"dev7.in1","to":"dev3.out0"},<br /> {"from":"dev8.in0","to":"dev2.out0"},<br /> {"from":"dev8.in1","to":"dev6.out0"}<br /> ]<br /> }
</div>

Když si to zapíšu pomocí výrazů, jako bych programoval: A XOR B = NOT (NOT (A AND NOT (A AND B)) AND NOT (B AND NOT (A AND B))); Je to totálně nesrozumitelné, ale pojďme si to dát po krocích dohromady:

  * C = NOT (A AND B) - první hradlo NAND
  * AX = NOT (A AND C) - horní hradlo NAND
  * BX = NOT (B AND C) - dolní hradlo NAND
  * Y = NOT (AX AND BX) - poslední NAND

Pomohla by nám tabulka?

<table style="width: 200px;" border="1" cellspacing="1">
  <tr>
    <th>
      A
    </th>
    
    <th>
      B
    </th>
    
    <th>
      C
    </th>
    
    <th>
      AX
    </th>
    
    <th>
      BX
    </th>
    
    <th>
      Y
    </th>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
</table>

Jasné? Doufám, že ano, tohle je esenciální věc celé číslicové techniky, totiž skládání komponent dohromady.

## Sčítáme čísla

Vážně, pokud neumíte [dvojkovou soustavu](https://cs.wikipedia.org/wiki/Dvojkov%C3%A1_soustava), tak máte poslední příležitost se ji naučit. Protože teď si sečteme 0110b a 0100b, a vy budete ztracení.

Pamatujete se, jak se sčítají čísla pod sebou na papíře? Tak to uděláme úplně stejně:

<pre class="">0110
+0100
-----
 1010</pre>

Můžeme si to z pilnosti ověřit: 0110b je 6, 0100b jsou 4, 6 + 4 = 10, a 10 zapíšeme jako 1010b.

Jak se sčítá? Začneme zprava, od nejnižšího řádu. 0 + 0 = 0, žádné cavyky. Posuneme se o jeden řád doleva. 1 + 0 = 1. Stále jasné. Další řád, další posun doleva, a teď máme 1 + 1. Normálně by to bylo 2, ale dvojku nemáme k dispozici, protože dvojková soustava má jen 0 a 1. Výsledek je tedy "10" binárně. Co s tím? No stejně jako když sčítáme třeba 3 + 9 pod sebe: výsledek je 12. Napíšeme dvojku, tedy tu číslici, co je vpravo, a tu jedničku, co je nalevo, si přeneseme o řád výš a tam ji přičteme. Takže v našem případě: 1 + 1 (binárně) = 0, a přenášíme jedničku na další pozici. Na té sčítáme 0 + 0 + přenesenou 1, a výsledek je 1.

Algoritmus je tedy evidentní, ale ještě než půjdeme dál, tak si prosím vyzkoušejte, že ho bez problémů chápete:

[WpProQuiz 2]

Pro každou pozici tedy provádíme operaci (A + B + Carry) => (Y, Carry), kde Carry (značený též CY) je přenos z nižšího řádu, resp. přenos do vyššího řádu.

Začneme nejprve s jednodušší variantou sčítačky, tedy s takovou, která neuvažuje přenos z nižšího řádu. Říká se tomu poloviční sčítačka, _half adder_. Její tabulka vypadá takto:

<table style="width: 200px;" border="1" cellspacing="1">
  <tr>
    <th>
      A
    </th>
    
    <th>
      B
    </th>
    
    <th>
      CY
    </th>
    
    <th>
      Y
    </th>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
</table>

Schválně se podívejte na sloupce CY a Y, mělo by vás to do očí uhodit... Už víte?

No jasně! CY je přeci obyčejné A AND B, Y je A XOR B.

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":false,<br /> "devices":[<br /> {"type":"DC","id":"dev0","x":56,"y":72,"label":"DC"},<br /> {"type":"DC","id":"dev1","x":56,"y":152,"label":"DC"},<br /> {"type":"Toggle","id":"dev2","x":120,"y":72,"label":"Toggle","state":{"on":false}},<br /> {"type":"Toggle","id":"dev3","x":120,"y":152,"label":"Toggle","state":{"on":false}},<br /> {"type":"AND","id":"dev4","x":304,"y":144,"label":"AND"},<br /> {"type":"LED","id":"dev5","x":424,"y":80,"label":"LED"},<br /> {"type":"LED","id":"dev6","x":424,"y":144,"label":"LED"},<br /> {"type":"In","id":"dev7","x":176,"y":72,"label":"A"},<br /> {"type":"In","id":"dev8","x":176,"y":152,"label":"B"},<br /> {"type":"EOR","id":"dev9","x":304,"y":80,"label":"XOR"},<br /> {"type":"Out","id":"dev10","x":368,"y":80,"label":"Y"},<br /> {"type":"Out","id":"dev11","x":368,"y":144,"label":"CY"}<br /> ],<br /> "connectors":[<br /> {"from":"dev2.in0","to":"dev0.out0"},<br /> {"from":"dev3.in0","to":"dev1.out0"},<br /> {"from":"dev4.in0","to":"dev7.out0"},<br /> {"from":"dev4.in1","to":"dev8.out0"},<br /> {"from":"dev5.in0","to":"dev10.out0"},<br /> {"from":"dev6.in0","to":"dev11.out0"},<br /> {"from":"dev7.in0","to":"dev2.out0"},<br /> {"from":"dev8.in0","to":"dev3.out0"},<br /> {"from":"dev9.in0","to":"dev7.out0"},<br /> {"from":"dev9.in1","to":"dev8.out0"},<br /> {"from":"dev10.in0","to":"dev9.out0"},<br /> {"from":"dev11.in0","to":"dev4.out0"}<br /> ]<br /> }
</div>

Poloviční sčítačka je dobrá, ale my chceme ještě k výsledku přičíst přenos z nižšího řádu a stvořit tak plnou sčítačku (_full adder_). Označme si přenos z nižšího řádu jako CI (Carry In), přenos směrem výš pak logicky jako CO (Carry Out) a podívejme se, jak bude vypadat pravdivostní tabulka:

<table style="width: 200px;" border="1" cellspacing="1">
  <tr>
    <th>
      A
    </th>
    
    <th>
      B
    </th>
    
    <th>
      CI
    </th>
    
    <th>
      CO
    </th>
    
    <th>
      Y
    </th>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
</table>

Když se budeme na dvojici CO a Y dívat jako na dvoubitové číslo, zjistíte, že jeho hodnota (0 - 3) udává počet jedniček na vstupech A, B, CI. To je zajímavý poznatek, ale my ho potřebujeme převést nějak do řeči hradel. Můžeme na to jít od lesa, prohlásit, že Y je vlastně (A XOR B), a pokud je CI = 1, tak je to negované. Schválně, máme takovou funkci, která by dělala něco takového, jako že "Y = A pokud B=0, a pokud B=1, tak Y = NOT A"? Máme?

Máme, a je to, světe div se, XOR. Podívejte se znovu na její tabulku hodnot - vidíte to tam? Takže u plné sčítačky můžeme říct, že Y = (A XOR B) XOR CI

Co s CO? Tam je to trošku složitější. Pokud CI = 0, tak jde o A AND B, pokud CI = 1, tak jde o A OR B. To moc nepomáhá. Co si to takhle popsat slovy: _CO je 1, pokud je zároveň A a B nebo A a C nebo B a C. Výrazem: CO = (A AND B) OR (A AND CI) OR (B AND CI)_

> _Mimochodem, víte, jak jsem psal, že OR se taky označuje jako logický součet a AND jako logický součin? Mohli bychom přepsat výše uvedený výraz do podoby CO = A\*B + B\*CI + A*CI_

Jediný problém je, jak udělat OR se třemi vstupy. Kupodivu docela jednoduše: A + B + C = (A + B) + C.

Získáváme tak náš první komplikovanější obvod. Vypadal by nějak takhle:

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":false,<br /> "devices":[<br /> {"type":"DC","id":"dev0","x":64,"y":168,"label":"DC"},<br /> {"type":"DC","id":"dev1","x":64,"y":248,"label":"DC"},<br /> {"type":"Toggle","id":"dev2","x":128,"y":168,"label":"Toggle","state":{"on":false}},<br /> {"type":"Toggle","id":"dev3","x":128,"y":248,"label":"Toggle","state":{"on":false}},<br /> {"type":"LED","id":"dev4","x":568,"y":136,"label":"LED"},<br /> {"type":"LED","id":"dev5","x":568,"y":200,"label":"LED"},<br /> {"type":"In","id":"dev6","x":184,"y":168,"label":"A"},<br /> {"type":"In","id":"dev7","x":184,"y":248,"label":"B"},<br /> {"type":"Out","id":"dev8","x":512,"y":136,"label":"Y"},<br /> {"type":"Out","id":"dev9","x":512,"y":200,"label":"CO"},<br /> {"type":"DC","id":"dev10","x":64,"y":96,"label":"DC"},<br /> {"type":"In","id":"dev11","x":184,"y":96,"label":"CI"},<br /> {"type":"EOR","id":"dev12","x":432,"y":104,"label":"XOR"},<br /> {"type":"AND","id":"dev13","x":280,"y":120,"label":"AND"},<br /> {"type":"EOR","id":"dev14","x":280,"y":176,"label":"XOR"},<br /> {"type":"AND","id":"dev15","x":280,"y":232,"label":"AND"},<br /> {"type":"AND","id":"dev16","x":280,"y":288,"label":"AND"},<br /> {"type":"OR","id":"dev17","x":352,"y":264,"label":"OR"},<br /> {"type":"OR","id":"dev18","x":432,"y":200,"label":"OR"},<br /> {"type":"Toggle","id":"dev19","x":128,"y":96,"label":"Toggle","state":{"on":false}}<br /> ],<br /> "connectors":[<br /> {"from":"dev2.in0","to":"dev0.out0"},<br /> {"from":"dev3.in0","to":"dev1.out0"},<br /> {"from":"dev4.in0","to":"dev8.out0"},<br /> {"from":"dev5.in0","to":"dev9.out0"},<br /> {"from":"dev6.in0","to":"dev2.out0"},<br /> {"from":"dev7.in0","to":"dev3.out0"},<br /> {"from":"dev8.in0","to":"dev12.out0"},<br /> {"from":"dev9.in0","to":"dev18.out0"},<br /> {"from":"dev11.in0","to":"dev19.out0"},<br /> {"from":"dev12.in0","to":"dev11.out0"},<br /> {"from":"dev12.in1","to":"dev14.out0"},<br /> {"from":"dev13.in0","to":"dev11.out0"},<br /> {"from":"dev13.in1","to":"dev6.out0"},<br /> {"from":"dev14.in0","to":"dev6.out0"},<br /> {"from":"dev14.in1","to":"dev7.out0"},<br /> {"from":"dev15.in0","to":"dev11.out0"},<br /> {"from":"dev15.in1","to":"dev7.out0"},<br /> {"from":"dev16.in0","to":"dev6.out0"},<br /> {"from":"dev16.in1","to":"dev7.out0"},<br /> {"from":"dev17.in0","to":"dev15.out0"},<br /> {"from":"dev17.in1","to":"dev16.out0"},<br /> {"from":"dev18.in0","to":"dev13.out0"},<br /> {"from":"dev18.in1","to":"dev17.out0"},<br /> {"from":"dev19.in0","to":"dev10.out0"}<br /> ]<br /> }
</div>

### Kompozice

Když jsem proklamoval, že "z jednodušších stavíme složitější", tak si to pojďme zkusit na tomto příkladu. Popíšeme si funkci pomocí pojmu "poloviční sčítačka". Ta, jak víme, vezme dva bity na vstupu, provede jednobitový aritmetický součet a výstupem je tato hodnota a hodnota přenosu. Co když musíme sečíst tři jednobitová čísla? Šlo by udělat obdobnou operaci, jakou známe z matematiky, totiž sečíst nejprve první dvě čísla, pak k výsledku přičíst třetí číslo...? No, asi by to šlo... Papír a tužku do ruky, budeme si malovat stejnou tabulku, jen si doplníme hodnoty pro mezisoučet A+B (nazveme ho D) a jeho přenos (CD), a pro součet D+CI (nazveme ho E a jeho přenos CE).

<table style="width: 400px;" border="1" cellspacing="1">
  <tr>
    <th>
      A
    </th>
    
    <th>
      B
    </th>
    
    <th>
      CI
    </th>
    
    <th>
      A+B (D)
    </th>
    
    <th>
      CD
    </th>
    
    <th>
      D+CI (E)
    </th>
    
    <th>
      CE
    </th>
    
    <th>
      CO
    </th>
    
    <th>
      Y
    </th>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
</table>

Všimněte si, že u sestavování takovýchto obvodů je pravdivostní tabulka velice užitečná věc. S trochou cviku vidíte ve sloupci výsledků pravidelné vzorce (0001, 0111, 1110, 1000, 0110, ...) a hned víte, jaké logické funkce budou asi zapotřebí. Existují postupy a nástroje, jak z takových tabulek optimalizovat logické výrazy a minimalizovat je (většinou na tvar "součet součinů" nebo "součin součtů"), například [Karnaughova mapa](https://cs.wikipedia.org/wiki/Karnaughova_mapa), a já se k nim v nějakém dalším nepovinném dílu vrátím (existuje i [online verze Karnaughovy mapy](http://www.32x8.com/)).

Mimochodem, pojďme si dát takovou mentální odbočku: Kolik je asi různých logických funkcí pro dva jednobitové vstupy?

### Logické funkce dvou proměnných

Kolik jich asi tak může být? No helejte - dvě vstupní proměnné mohou nabývat celkem čtyř kombinací: 00, 01, 10 a 11. Každé téhle kombinaci odpovídá nějaký výstup, který můžeme zapsat jako čtyřbitové slovo - jako bychom četli poslední sloupec pravdivostní tabulky shora dolů. Pro OR to je 0111, pro NAND zase 1110... Je jasné, že tedy máme 16 možných výsledků. Pojďme si je zase zapsat do tabulky.

Připravte se, největší tabulka celého kurzu následuje:

<table border="1" cellspacing="1">
  <tr>
    <th colspan="4">
      AB
    </th>
    
    <th rowspan="2">
      Funkce
    </th>
    
    <th rowspan="2">
      Poznámka
    </th>
  </tr>
  
  <tr>
    <th>
      00
    </th>
    
    <th>
      01
    </th>
    
    <th>
      10
    </th>
    
    <th>
      11
    </th>
  </tr>
  
  <tr>
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td>
      Y = 0
    </td>
    
    <td>
      Kontradikce, <em>false</em>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td>
      Y = A AND B
    </td>
    
    <td>
      Součin (konjunkce)
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td>
      Y = NOT (A - > B)
    </td>
    
    <td>
      Nonimplikace
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td>
      Y = A
    </td>
    
    <td>
      Projekce
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td>
      Y = NOT (A <- B)
    </td>
    
    <td>
      Obrácená nonimplikace
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td>
      Y = B
    </td>
    
    <td>
      Projekce
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td>
      Y = A XOR B
    </td>
    
    <td>
      Nonekvivalence
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td>
      Y = A OR B
    </td>
    
    <td>
      Disjunkce
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td>
      Y = NOT (A OR B)
    </td>
    
    <td>
      NOR
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td>
      Y = A XNOR B
    </td>
    
    <td>
      Ekvivalence
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td>
      Y = NOT B
    </td>
    
    <td>
      Negace
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td>
      Y = A <- B
    </td>
    
    <td>
      Obrácená implikace
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td>
      Y = NOT A
    </td>
    
    <td>
      Negace
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td>
      Y = A -> B
    </td>
    
    <td>
      Implikace
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td>
      Y = NOT (A AND B)
    </td>
    
    <td>
      NAND
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td>
      Y = 1
    </td>
    
    <td>
      Tautologie, <em>true</em>
    </td>
  </tr>
</table>

Není jich víc? Co? No, asi není. Všimněte si, že v tabulce jsou naši staří známí (AND, NAND, OR, NOR, XOR a jeho negovaný bratr XNOR), jsou tam i funkce jedné proměnné (negace a projekce), jsou tam funkce, které jsou vždy 0, nebo vždy 1 (_a přátelé, taková funkce, to snad ani není funkce!_), a jsou tam ještě čtyři funkce implikace (normální, obrácená, a dvě jejich negované varianty). Funkce implikace jako jediné porušují pravidlo komutativnosti, tedy když zaměníme vývody A a B, dostaneme jinou funkci. Po pravdě řečeno nevím o tom, že by někdo "implikační" hradla vyráběl jako integrovaný obvod. Pokud už někdo potřebuje třeba implikaci (1101), tak použije například hradlo OR, a na vstup A připojí invertor. Schválně se podívejte, jestli implikace není totéž co (NOT A) OR B.

### Plná jednobitová sčítačka

Vraťme se zpátky k naší sčítačce. Jistě jste si tabulku už vyplnili, a vyšlo vám, že výstup sčítačky je roven E, a výstupní přenos CO je OR mezi oběma podpřenosy:

<table style="width: 400px;" border="1" cellspacing="1">
  <tr>
    <th>
      A
    </th>
    
    <th>
      B
    </th>
    
    <th>
      CI
    </th>
    
    <th>
      A+B (D)
    </th>
    
    <th>
      CD
    </th>
    
    <th>
      D+CI (E)
    </th>
    
    <th>
      CE
    </th>
    
    <th>
      CO
    </th>
    
    <th>
      Y
    </th>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
</table>

Můžeme tedy vzít co? No jasně, můžeme vzít dvě poloviční sčítačky a hradlo OR, a poskládat si z toho plnou sčítačku:

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":true,<br /> "toolbox":[<br /> {"type":"HalfAdder"}<br /> ],<br /> "devices":[<br /> {"type":"DC","id":"dev0","x":64,"y":168,"label":"DC"},<br /> {"type":"DC","id":"dev1","x":64,"y":248,"label":"DC"},<br /> {"type":"Toggle","id":"dev2","x":128,"y":168,"label":"Toggle","state":{"on":false}},<br /> {"type":"Toggle","id":"dev3","x":128,"y":248,"label":"Toggle","state":{"on":false}},<br /> {"type":"LED","id":"dev4","x":536,"y":136,"label":"LED"},<br /> {"type":"LED","id":"dev5","x":536,"y":200,"label":"LED"},<br /> {"type":"In","id":"dev6","x":184,"y":168,"label":"A"},<br /> {"type":"In","id":"dev7","x":184,"y":248,"label":"B"},<br /> {"type":"Out","id":"dev8","x":480,"y":136,"label":"Q"},<br /> {"type":"Out","id":"dev9","x":480,"y":200,"label":"CY"},<br /> {"type":"DC","id":"dev10","x":64,"y":96,"label":"DC"},<br /> {"type":"In","id":"dev11","x":184,"y":96,"label":"In"},<br /> {"type":"Toggle","id":"dev12","x":128,"y":96,"label":"Toggle","state":{"on":false}},<br /> {"type":"HalfAdder","id":"dev13","x":248,"y":208,"label":"HalfAdder"},<br /> {"type":"HalfAdder","id":"dev14","x":336,"y":136,"label":"HalfAdder"},<br /> {"type":"OR","id":"dev15","x":424,"y":200,"label":"OR"}<br /> ],<br /> "connectors":[<br /> {"from":"dev2.in0","to":"dev0.out0"},<br /> {"from":"dev3.in0","to":"dev1.out0"},<br /> {"from":"dev4.in0","to":"dev8.out0"},<br /> {"from":"dev5.in0","to":"dev9.out0"},<br /> {"from":"dev6.in0","to":"dev2.out0"},<br /> {"from":"dev7.in0","to":"dev3.out0"},<br /> {"from":"dev8.in0","to":"dev14.out0"},<br /> {"from":"dev9.in0","to":"dev15.out0"},<br /> {"from":"dev11.in0","to":"dev12.out0"},<br /> {"from":"dev12.in0","to":"dev10.out0"},<br /> {"from":"dev13.in0","to":"dev6.out0"},<br /> {"from":"dev13.in1","to":"dev7.out0"},<br /> {"from":"dev14.in0","to":"dev11.out0"},<br /> {"from":"dev14.in1","to":"dev13.out0"},<br /> {"from":"dev15.in0","to":"dev14.out1"},<br /> {"from":"dev15.in1","to":"dev13.out1"}<br /> ]<br /> }
</div>

Všimněte si, že teď už se nezabývám tím, jak je poloviční sčítačka udělaná uvnitř. Prostě jen namaluju obdélníček, podle konvence namaluju doleva vstupy, doprava výstupy (protože _schémata se malují většinou tak, aby hlavní tok informací šel jako psaný text: zleva doprava, shora dolů_), a napíšu k tomu, že to je Half Adder. To stačí.

Stejný postup se volí pak v případě složitějších obvodů. Budeme třeba tvořit multiplexery a dekodéry. A jakmile si je sestavíme, tak si je _zapouzdříme_, někde popíšeme jejich chování a signály, a dál pracujeme už jen se schematickým vyjádřením. Naprosto stejný princip používají třeba i jazyky pro popis elektronického zapojení pro moderní čipy FPGA, například VHDL (znáte můj [kurz VHDL](https://vhdl.cz)?) Podívejte se, jen pro zajímavost, jak [ve VHDL skládám plnou sčítačku z polovičních](https://vhdl.cz/komponenty-a-signaly/#more-36):

<pre class="lang:c decode:true ">library ieee;
use ieee.std_logic_1164.all;

entity fulladder is
 port (
 A, B, CI: in std_logic;
 Y, CO: out std_logic
 );
end entity;

architecture main of fulladder is

 component halfadder is
  port (
   A, B: in std_logic;
   Y, CY: out std_logic
  );
 end component;

signal D, CD, CE: std_logic;

begin
ADDER1: halfadder port map (A=&gt;A, B=&gt;B, Y=&gt;D, CY=&gt;CD);
ADDER2: halfadder port map (Y=&gt;Y, A=&gt;CI, CY=&gt;CE, B=&gt;D);

CO &lt;= CD or CE;

end architecture;</pre>

Na začátku říkám, jak se plná sčítačka tváří navenek (tři vstupy, dva výstupy), a pak říkám, že její architektura se skládá z komponenty halfadder (a definuju její rozhraní) a nějakých vnitřních signálů (D, CD, CE - vzpomeňte si na tabulku výše). Použiju dvě poloviční sčítačky (adder 1 a 2), připojím vývody a pak nadefinuju ještě operaci OR (ta ve VHDL patří k primitivním operacím a není pro ni potřeba dělat komponentu). Zas tak se to neliší od postupu výše, že?

Kduž se podíváme na naše nové schéma, tak si můžeme snadno spočítat, že i když si poloviční sčítačky "rozepíšeme" na původní komponenty (XOR a AND), tak nové řešení ušetří dvě hradla proti prvnímu "naivnímu" postupu. Je to tím, že v naivní verzi jsme oba výstupy zpracovávali odděleně. Ale to není jediný důvod, proč volit složitější komponenty a skládat to z nich. U prvního zapojení máme dvě hradla XOR, tři AND a dvě OR. Vzhledem k tomu, že dvouvstupové logické elementy se nejčastěji integrují po čtyřech do jednoho pouzdra, tak bychom spotřebovali tři integrované obvody na jednu plnou sčítačku. U druhého přístupu je velmi pravděpodobné, že někdo bude vyrábět už tu použitou komponentu v jednom pouzdře a ušetříme tak na propojování a komponentách. (Ve skutečnosti se vyráběly obvody 7480, 7482 a 7483, které implementovaly plnou sčítačku, včetně čtyřbitové verze).

Apropo, čtyřbitová verze: Zapojíme vlastně čtyři plné sčítačky vedle sebe. Nejnižší bit slova A (tedy A0) a nejnižší bit slova B (B0) přivedeme na první sčítačku. Její výstup vytvoří nejnižší bit výsledku (Y0), její výstupní přenos (CO) se stane vstupním přenosem pro sčítačku vyššího řádu, a tak dál...

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":true,<br /> "toolbox":[<br /> {"type":"4bit7seg"},<br /> {"type":"RotaryEncoder"},<br /> {"type":"HalfAdder"},<br /> {"type":"FullAdder"} ],<br /> "devices":[<br /> {"type":"RotaryEncoder","id":"dev0","x":64,"y":48,"label":"RotaryEncoder"},<br /> {"type":"DC","id":"dev1","x":8,"y":64,"label":"DC"},<br /> {"type":"RotaryEncoder","id":"dev2","x":64,"y":160,"label":"RotaryEncoder"},<br /> {"type":"DC","id":"dev3","x":8,"y":176,"label":"DC"},<br /> {"type":"FullAdder","id":"dev4","x":224,"y":120,"label":"FullAdder"},<br /> {"type":"FullAdder","id":"dev5","x":224,"y":48,"label":"FullAdder"},<br /> {"type":"FullAdder","id":"dev6","x":224,"y":192,"label":"FullAdder"},<br /> {"type":"FullAdder","id":"dev7","x":224,"y":264,"label":"FullAdder"},<br /> {"type":"4bit7seg","id":"dev8","x":400,"y":56,"label":"4bit7seg"},<br /> {"type":"LED","id":"dev9","x":416,"y":184,"label":"Carry"}<br /> ],<br /> "connectors":[<br /> {"from":"dev0.in0","to":"dev1.out0"},<br /> {"from":"dev2.in0","to":"dev3.out0"},<br /> {"from":"dev4.in0","to":"dev5.out1"},<br /> {"from":"dev4.in1","to":"dev0.out1"},<br /> {"from":"dev4.in2","to":"dev2.out1"},<br /> {"from":"dev5.in1","to":"dev0.out0"},<br /> {"from":"dev5.in2","to":"dev2.out0"},<br /> {"from":"dev6.in0","to":"dev4.out1"},<br /> {"from":"dev6.in1","to":"dev0.out2"},<br /> {"from":"dev6.in2","to":"dev2.out2"},<br /> {"from":"dev7.in0","to":"dev6.out1"},<br /> {"from":"dev7.in1","to":"dev0.out3"},<br /> {"from":"dev7.in2","to":"dev2.out3"},<br /> {"from":"dev8.in0","to":"dev5.out0"},<br /> {"from":"dev8.in1","to":"dev4.out0"},<br /> {"from":"dev8.in2","to":"dev6.out0"},<br /> {"from":"dev8.in3","to":"dev7.out0"},<br /> {"from":"dev9.in0","to":"dev7.out1"}<br /> ]<br /> }
</div>

[WpProQuiz 3]

## Domácí úkol

Za domácí úkol si zkuste navrhnout osmibitovou plnou sčítačku a zkuste se zamyslet nad tím, jak ji proměnit v "odčítačku".

V příštím díle kurzu se budeme věnovat posledním kombinačním obvodům - tedy multiplexerům, dekodérům, demultiplexerům a dalším nezbytnostem. Těším se na vás!

## Anketa

Prosím, najděte si minutu času a [odpovězte na pár otázek](https://docs.google.com/forms/d/e/1FAIpQLSeJTL1lzI7_5rh9Xp2Yws4PB81fTX7qBZMnFdw0DYDKaJOdlg/viewform?entry.525375248), které mohou ovlivnit další podobu kurzu. Děkuju.