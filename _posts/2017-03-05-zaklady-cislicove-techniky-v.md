---
id: 282
title: Základy číslicové techniky V
date: 2017-03-05T16:34:02+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=282
permalink: /zaklady-cislicove-techniky-v/
image: http://iotta.cz/wp-content/uploads/sites/17/2017/03/800px-74181aluschematic.png
categories:
  - DIY
  - Hardware
tags:
  - Číslicová technika
---
Od teorie zase na chvíli zpátky k základním stavebním kamenům číslicové techniky. Zbývá nám probrat poslední kombinační obvody, než se vrhneme na sekvenční...

<!--more-->

Stručná rekapitulace: Ukázali jsme si základní stavební kámen číslicových obvodů, totiž **hradlo**. Hradlo je součástka, která _realizuje_ logickou funkci. Základní logické funkce jsou AND, OR a NOT. Jejich vhodnou kombinací lze získat všechny logické funkce. V praxi se používá hradlo NAND (popřípadě NOR), z nějž lze poskládat všechny ostatní: spojením vývodů, popřípadě připojením jednoho na log. 1 (u NOR na log. 0) získáme invertor, připojením invertoru na výstup NAND získáme AND, připojením invertorů na oba vstupy NAND získáme OR, spojením čtyř NAND získáme XOR...

Proč _hradlo_? Hradlo je pojem z železnice. Do jeho přesné definice se pouštět nebudu, to by mě případní železničáři mezi vámi utloukli. Zůstanu u jednoduchého lidového popisu: Hradlo je _ten semafór_, který říká v zásadě buď "volno", nebo "stůj". Obvod s funkcí AND vlastně může hrát roli takového "hradla" pro logický signál. Představme si zapojení:

<div class="simcir">
  {<br /> "width":700,<br /> "height":300,<br /> "showToolbox":false,<br /> "toolbox":[],<br /> "devices":[<br /> {"type":"AND","id":"dev0","x":152,"y":136,"label":"AND"},<br /> {"type":"OSC","id":"dev1","x":16,"y":144,"label":"OSC"},<br /> {"type":"LED","id":"dev2","x":280,"y":136,"label":"LED"},<br /> {"type":"PushOn","id":"dev3","x":88,"y":48,"label":"PushOn"},<br /> {"type":"DC","id":"dev4","x":16,"y":48,"label":"DC"}<br /> ],<br /> "connectors":[<br /> {"from":"dev0.in0","to":"dev3.out0"},<br /> {"from":"dev0.in1","to":"dev1.out0"},<br /> {"from":"dev2.in0","to":"dev0.out0"},<br /> {"from":"dev3.in0","to":"dev4.out0"}<br /> ]<br /> }
</div>

Obvodem prochází signál - pulsy z oscilátoru vlevo do LED vpravo. Pokud je tlačítko puštěné, je na jednom vstupu hradla AND logická 0, a výsledkem je tedy vždy log. 0, bez ohledu na to, jaký je signál. Pokud tlačítko stiskneme, dostane se na řídicí vstup logická 1, a na výstupu bude hodnota, odpovídající hodnotě na druhém vstupu. Schválně, podívejte se na pravdivostní tabulku, že to tak je. No a tahle schopnost připomíná právě ten železniční _semafór_: signál buď prochází, nebo neprochází (ale na rozdíl od vlaků se před hradlem nehromadí...) Hradlo OR funguje podobně, jen "signál zastaven" je log. 1, a spouští se logickou nulou. Proto hradlo.

Ukázali jsme si, jak lze spojovat základní hradla do složitějších celků - příkladem byla sčítačka jednobitových čísel, kterou jsme si rozšířili o práci s přenosem. Čtyři sčítačky vedle sebe nám stvořily čtyřbitovou sčítačku. Dvě čtyřbitové vedle sebe stvoří osmibitovou...

## Vícevstupová hradla

Ještě jsme si neřekli, i když si to mnozí z vás asi odvodili, že existují i vícevstupová hradla - v praxi se používají třívstupová, čtyřvstupová a osmivstupová, ale při syntéze obvodů v logických polích si klidně nadefinujeme třináctivstupové AND. Funkce je podobná těm dvouvstupovým, jen si slovo "oba" nahraďte slovy "všechny" (AND). Tedy: **Výsledkem AND je 1, pokud všechny vstupy jsou 1, jinak 0. OR je 1, pokud alespoň jeden vstup je 1, jinak 0.**

Schválně, co udělá třívstupový XOR? Už jsme na to trošku narazili u sčítačky... Třívstupový XOR je v log. 1 tehdy, pokud je na vstupu právě jedna logická 1, nebo pokud tam jsou tři. V logické 0 je, pokud na vstupu nejsou žádné jedničky, nebo pokud tam jsou dvě. Obvod XOR tak vlastně počítá **lichou paritu**.

Lichá parita (Parity Odd) nějakého vícebitového signálu je jednobitový údaj, který je log. 1 tehdy a jen tehdy, pokud je počet logických 1 v signálu lichý. Paritní bit se používá pro jednoduchou kontrolu správnosti dat: pokud je jeden bit poškozen (má opačnou hodnotu), parita nesedí. Na více bitů bohužel jednoduchá parita nesedí. A ano, je i sudá parita (Parity Even), která má přesně opačnou funkci.

## Zjednodušování logických výrazů

Minule u sčítačky jsme si ukazovali práci s logickými výrazy a jejich optimalizaci. Na logické výrazy můžeme s trochou dobré vůle nahlížet podobně jako na aritmetické - i zde existují například operace sčítání a násobení, i tyto operace mají neutrální prvek 0, resp. 1, i tyto operace jsou komutativní (lze prohodit operandy) a asociativní, i zde lze krátit a dělat podobné operace. Nejčastější situace, kdy je potřeba pracovat s těmito funkcemi, je tehdy, když máte daný "black box", tedy nějaký obvod s určitým počtem vstupů a výstupů, k tomu dostanete pravdivostní tabulku, tedy seznam požadovaných hodnot při určitých vstupních kombinacích, a vy máte navrhnout z hradel ekvivalentní zapojení.

V číslicové technice platí, že libovolný výraz, zadaný pomocí tabulky hodnot, můžeme převést na součet součinů (popřípadě součin součtů), a to nad vstupními signály a jejich negacemi. Tedy třeba _(A AND B) OR (NOT B AND C) OR (C AND NOT D AND E)... _V praxi se k takovému zjednodušení používají různé metody - [Karnaughova mapa](https://en.wikipedia.org/wiki/Karnaugh_map), [Quine-McCluskey algoritmus](https://en.wikipedia.org/wiki/Quine%E2%80%93McCluskey_algorithm)... Jednou se k nim dostaneme, slibuju. Nejpozději ve chvíli, kdy bude řeč o logických polích. Do té doby se nebojte, budu vybírat takové výrazy, kde nebude zjednodušení potřeba.

## AND-OR-INVERT

Přiznám se, že když jsem se seznamoval s číslicovou technikou, tak jsem moc nerozuměl smyslu tohoto obvodu. V katalogu TESLA jich bylo několik (mezi 7450 a 7470), a mně vrtalo hlavou, k čemu takový nesmysl může být. Vám ušetřím tápání: AND-OR je ten výše zmíněný "součet součinů", takže tyto obvody slouží právě ke skládání složitých logických funkcí.

No a druhá věc, která je na AND-OR hezká, je, že mohou přímo sloužit k vytvoření obvodu, který se jmenuje

## Multiplexor

V anglické literatuře se tentýž obvod jmenuje **multiplexer**, a česky jsem slyšel oba tvary. Ačkoli se většinou přimlouvám za jednotnout terminologii, tak zrovna u tohoto slova používám oba tvary. _Hanba mi!_

Multiplexor je "digitálně řízený přepínač". V základní podobě má několik signálových vstupů (třeba 2, označme si je A a B), jeden výstup (Q) a jeden řídicí vstup (S), který říká, který ze vstupů má být připojen na výstup. U našeho dvouvstupového multiplexeru stačí jeden řídicí bit. Funkce je pak v závislosti na hodnotě S následující:

<table style="width: 300px;">
  <tr>
    <th>
      S
    </th>
    
    <th>
      Q
    </th>
  </tr>
  
  <tr>
    <td>
    </td>
    
    <td style="text-align: right;">
      hodnota ze vstupu A
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td style="text-align: right;">
      hodnota ze vstupu B
    </td>
  </tr>
</table>

Jasné? Pokud ne, tak si to celé ještě rozepíšeme:

<table style="width: 200px;" border="1" cellspacing="1">
  <tr>
    <th>
      S
    </th>
    
    <th>
      A
    </th>
    
    <th>
      B
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
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <b>1</b>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <b>1</b>
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
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <b>1</b>
    </td>
    
    <td style="text-align: center;">
      <strong>1</strong>
    </td>
    
    <td style="text-align: center;">
      <strong></strong>
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <b>1</b>
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
  </tr>
</table>

Všimněte si, že funkce odpovídá slovnímu popisu výše: pokud je S=0, je na výstupu Y hodnota, která je na vstupu A, a vstup B se ignoruje. Pokud je S=1, je na výstupu hodnota ze vstupu B bez ohledu na vstup A.

Můžeme navrhnout různá zapojení, ale nejjednodušší získáme, když si uvědomíme to, co jsme si řekli výše o hradlech: vstup S bude povolovat buď signál A, nebo signál B. Signál B bude povolovat, pokud bude roven 1. Takže "povolený signál B" bude vlastně "B AND S". Signál A je povolen v nule, takže "povolený signál A" bude "A AND NOT S". No a protože víme, že "zakázaný signál" je 0, a nula je zároveň neutrální hodnota pro OR, můžeme výsledek zapsat jako "povolené A OR povolené B" - po dosazení tedy "(A AND NOT S) OR (B AND S)".

Schéma libo-li?

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":false,<br /> "toolbox":[<br /> ],<br /> "devices":[<br /> {"type":"DC","id":"dev0","x":16,"y":48,"label":"DC"},<br /> {"type":"DC","id":"dev1","x":16,"y":104,"label":"DC"},<br /> {"type":"Toggle","id":"dev2","x":80,"y":48,"label":"Signál A","state":{"on":false}},<br /> {"type":"Toggle","id":"dev3","x":80,"y":104,"label":"Signál B","state":{"on":false}},<br /> {"type":"Toggle","id":"dev4","x":80,"y":192,"label":"S","state":{"on":false}},<br /> {"type":"DC","id":"dev5","x":16,"y":192,"label":"DC"},<br /> {"type":"LED","id":"dev6","x":376,"y":72,"label":"LED"},<br /> {"type":"NOT","id":"dev7","x":176,"y":192,"label":"NOT"},<br /> {"type":"AND","id":"dev8","x":176,"y":104,"label":"AND"},<br /> {"type":"AND","id":"dev9","x":256,"y":48,"label":"AND"},<br /> {"type":"OR","id":"dev10","x":320,"y":72,"label":"OR"}<br /> ],<br /> "connectors":[<br /> {"from":"dev2.in0","to":"dev0.out0"},<br /> {"from":"dev3.in0","to":"dev1.out0"},<br /> {"from":"dev4.in0","to":"dev5.out0"},<br /> {"from":"dev6.in0","to":"dev10.out0"},<br /> {"from":"dev7.in0","to":"dev4.out0"},<br /> {"from":"dev8.in0","to":"dev3.out0"},<br /> {"from":"dev8.in1","to":"dev4.out0"},<br /> {"from":"dev9.in0","to":"dev2.out0"},<br /> {"from":"dev9.in1","to":"dev7.out0"},<br /> {"from":"dev10.in0","to":"dev9.out0"},<br /> {"from":"dev10.in1","to":"dev8.out0"}<br /> ]<br /> }
</div>

Pomocí hradla AND-OR jsme si stvořili multiplexor. Jeho funkce spočívá v propojení vybraného vstupu s výstupem. Vstup se vybírá pomocí řídicího vstupu S. V naší nejjednodušší podobě má řídicí vstup S jeden bit, proto může vybrat jeden ze dvou vstupů. Pokud použijeme dva řídicí bity S1 a S2, můžeme vybrat jeden ze čtyř vstupů. Při třech řídicích bitech jeden z osmi vstupů. V praxi jsou nejčastěji používány právě tyto tři multiplexory, totiž se dvěma, čtyřmi a osmi vstupy.

Zkusme si cvičně vytvořit soustavu rovnic pro čtyřvstupový multiplexor:

<pre class="">Y = (A AND NOT S1 AND NOT S2) OR 
    (B AND S1 AND NOT S2) OR
    (C AND NOT S1 AND S2) OR
    (D AND S1 AND S2)</pre>

Na každém řádku je zapsaný logický součin pro jeden vstup. NOT S1 AND NOT S2 je splněno pro S1=S2=0, S1 AND NOT S2 je splněno pro S1=1, S2=0 a tak dále. Takto vlastně "hradlujeme" čtyři vstupy, a nakonec je prostě sloučíme do jednoho.

### Proč slučujeme přes OR?

Ano, správná otázka. To bychom nemohli ty vývody prostě jen tak spojit dohromady drátem? No, mohli. Taky bychom mohli zkratovat baterii hřebíkem, ale neděláme to. Důvod je, že by to nefungovalo tak, jak si představujeme. U obvodů TTL totiž platí, že proud může téci z výstupu i do výstupu. A pokud bychom spojili "jen tak drátem" vývody dvou hradel, a na jednom by byla výstupní hodnota 0, na druhém 1, tekl by proud z jednoho výstupu do výstupu druhého hradla a jen by se darmo pálila elektřina. Pokud chceme nějak "sloučit" výstupy hradel, musíme vždy použít logickou funkci (pravděpodobně OR). Existují výjimky, ke kterým se dostanu za chvilku.

## Dekodér (demultiplexor) "1-z-N"

Demultiplexor je součástka s opačnou funkcí, než má multiplexor. Má jeden signálový vstup, několik výstupů, a řídicí vstup, který říká, na jaký výstup bude vstup připojen. Ostatní výstupy budou v logické nule, případně ve třetím stavu (za chvíli si o něm něco povíme).

Speciálním případem demultiplexoru je dekodér - ten nemá signálový vstup, jen výstupy a řídicí vstupy. Podle toho, jaká kombinace je na řídicích vstupech, je na zvoleném výstupu jednička, na ostatních nula (nebo obráceně).

Dekodéry se používají nejčastěji 1-ze-4, 1-z-8, 1-z-10 a 1-z-16. Mají tedy dva, tři nebo čtyři řídicí vstupy a odpovídající počet výstupů. Logická funkce je podobná té u multiplexoru:

<pre class="">Y1 = NOT S1 AND NOT S2
Y2 = S1 AND NOT S2
Y3 = NOT S1 AND S2
Y4 = S1 AND S2</pre>

<table style="width: 200px;" border="1" cellspacing="1">
  <tr>
    <th>
      S1
    </th>
    
    <th>
      S2
    </th>
    
    <th>
      Y1
    </th>
    
    <th>
      Y2
    </th>
    
    <th>
      Y3
    </th>
    
    <th>
      Y4
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
    
    <td style="text-align: center;">
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
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
</table>

### Vícebitové varianty

Jak multiplexory, tak demultiplexory se používají nejen jako jednobitové, ale i jako vícebitové - fyzicky to je pak několik (de)multiplexorů se spojenými řídicími vstupy. Hovoříme pak například o čtyřkanálovém čtyřbitovém multiplexoru (tedy 2 řídicí vstupy, 4 datové vstupy, každý po 4 bitech, 1 výstup po 4 bitech), nebo dvoukanálovém (1 řídicí bit) osmibitovém (vstupy i výstupy mají osm vodičů, A0-A7, B0-B7 ,Y0-Y7) multiplexoru. Tyto obvody se používají velmi často, a to pro směřování signálů takovou cestou, jakou bychom si představovali. Například pokud znáte stroják procesoru 8080 nebo Z80, tak víte, že v instrukčním slovu u operace MOV a dalších je číslo registru ve třech bitech. Fyzicky v procesoru existuje osmikanálový multiplexor, který má tyto tři bity připojeny na svoje řídicí vstupy, na datových vstupech má připojeny jednotlivé registry, a podle hodnoty pak na vnitřní datovou sběrnici připojí jeden z těchto registrů.

## Otevřený kolektor, třetí stav, OE

Průběžně tu na to narážíme, tak si to pojďme rovnou popsat.

Už jsem tu říkal, že nesmíme jen tak spojit výstupy hradel TTL "nakrátko". Nejen že by tekly obvodem úplně zbytečné proudy, ale navíc by ani nebylo jasné, jaká hodnota je na výstupu - jednotlivá hradla by se o vedení přetahovala a vyhrálo by to nejsilnější. Proto: ne, nikdy, nikdy, nikdy!

Jenže! Občas potřebujeme, aby se na jedno společné vedení připojovalo víc funkčních jednotek. Třeba na datové sběrnici procesoru visí paměť RAM, paměť ROM a různé periferie, a podle adresy se vybírá konkrétní obvod, s nímž se komunikuje. Až dosud dobrý, ale teď mi řekněte: Co mají dělat ostatní obvody, připojené tamtéž, se svými výstupy? Nemůžou být ani v log. 1, ani v log. 0, tím by totiž rušily přenos dat.

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/f04xx05.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-285" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/f04xx05.jpg" alt="" width="384" height="299" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/f04xx05.jpg 384w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/f04xx05-300x234.jpg 300w" sizes="(max-width: 384px) 100vw, 384px" /></a>

Co mají udělat? Jedna z možností je použít obří multiplexor, a připojovat vždy pouze jediné zařízení. Jenže když si představíte, jak by to vypadalo u osmibitového počítače: máme třeba pět zařízení (RAM, ROM, časovač, paralelní port, sériový port), každé po osmi bitech, to je 40 bitů, dalších 8 bitů výstup, 3 bity řídicí - jen ten multiplexor by měl pouzdro větší než celý procesor!

V praxi se na to jde jinak. Existuje totiž něco, čemu se říká **třetí stav**. Označuje se Z, říká se mu taky "stav vysoké impedance" a znamená to, že obvod má výstup odpojený. Zkrátka neposílá na něj ani logickou 0, ani logickou 1, zkrátka nic.

Ne každý obvod má třístavové výstupy. Hradla, o nichž jsme se až dosud bavili, to většinou neumožňují. Složitější součástky, třeba dnes popsané demultiplexory, nebo třeba paměti či procesory, ale mají možnost, jak své výstupy odpojit. U pamětí, dekodérů, demultiplexorů a podobných najdeme vstup, který se jmenuje OE, tedy "output enable". Pokud je 1, obvod funguje normálně, pokud je 0, jsou všechny vývody nastavené do stavu Z, tedy odpojené.

Díky tomu může být na jedné datové sběrnici připojena paměť RAM i ROM i periferie. Všechny tyto obvody mají totiž výstupy ve stavu Z, a procesor si určí, ze kterého chce číst. Tomu pak povolí přístup na sběrnici tím, že mu pošle signál OE.

Speciálním typem třístavových zařízení, s nimiž se setkáme i u hradel, je takzvaný **výstup s otevřeným kolektorem**. Setkáte se s ním i dnes poměrně běžně - třeba sběrnice I2C, používaná pro připojování senzorů, má právě tento druh výstupů. Většina lidí si pamatuje jen to, že "musí přidat pull-up rezistor", ale my si teď řekneme i důvod, proč tomu tak je.

Výstup s otevřeným kolektorem má dva stavy: logickou 0, při níž je výstup spojen se zemí, a logickou 1, při níž je výstup odpojený (stav Z). Pokud spolu spojíme vodičem výstupy s otevřeným kolektorem, máme dvě možnosti: Buď jsou všechny výstupy v logické 1, jsou tudíž odpojené a na spojovacím vodiči je v tu chvíli "nic", nebo je některý z výstupů v logické 0, a spojovací vodič je tedy spojen se zemí.

V kurzu Arduina ukazujeme způsob připojení tlačítka, kdy jeho stisknutí propojí vstup procesoru se zemí. Jakmile je tlačítko puštěné, je spojení se zemí rozpojené, a na vstupu je tedy neurčitá hodnota. Díky rezistoru, připojenému mezi tlačítko a napájecí napětí, je v takovou chvíli na vstupu vysoká logická úroveň. Tlačítko zde funguje vlastně jako "výstup s otevřeným kolektorem" - jeden stav je logická 0, druhý stav je "odpojeno".

Proto se na takto spojené výstupy s otevřeným kolektorem připojuje přes rezistor napájecí napětí. V klidovém stavu, když jsou výstupy odpojené, je tak na výstupech přes rezistor logická 1. Pokud některý z výstupů sepne k zemi, bude na vedení logická 0. Samozřejmě si to můžeme nasimulovat pomocí přepínačů a žárovek, princip je stejný.

Řekněme, že máme tři stanoviště, kde může vzniknout poplach, a chceme, aby vždy všechna stanoviště viděla, že na některém vznikl poplach. Máme opět spoustu možností, ale když zvolíme "otevřený kolektor", vystačíme si s jediným signálovým vodičem.



Za normálního stavu, tedy v klidu, je signálový vodič připojen k žárovkám na jednotlivých stanovištích, a zároveň přes rezistor k napájecímu napětí. Je na něm tedy logická 1. Jakmile se na některém ze stanovišť přepne přepínač k zemi, tedy k logické 0, uzavře se obvod tak, že poteče proud přes žárovky na stanovištích přes tento přepínač k zemi. Žárovky se rozsvítí, a o to nám šlo!

U už zmíněné sběrnice I2C se jedná o podobný postup (stejně jako u 1-Wire). Zařízení jsou datovými vodiči s otevřeným kolektorem připojeny k signálu SDA, a v klidu je tento signál "zvedán" k log. 1 rezistorem. Pokud je třeba vyslat data, tak buď procesor, nebo zařízení (podle toho, jak se předem domluví), posílá podle hodinových pulzů data tak, že buď "stahuje výstup k nule", nebo jej nechává odpojený (=log. 1).

V tomto kurzu ještě nebudeme třístavové výstupy ani otevřený kolektor používat, to přijde až později, ale je dobré se to naučit teď.

## Dekodéry

Z kombinačních obvodů už nám toho k probrání moc nezbývá. Užitečné jsou dekodéry, které berou vstupní informaci, většinou vícebitovou, a převádí ji na nějaký jiný vícebitový kód. Asi nejpoužívanější jsou dekodéry, určené pro připojení sedmisegmentových displejů. Sedmisegmentovky pomocí rozsvěcení určitých segmentů ukazují číslice 0-9 (a s trochou dobré vůle i některá písmena). Jejich segmenty jsou označeny A až G, a to takto:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/220px-7_segment_display_labeled.svg_.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-286" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/220px-7_segment_display_labeled.svg_.png" alt="" width="220" height="220" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/220px-7_segment_display_labeled.svg_.png 220w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/220px-7_segment_display_labeled.svg_-150x150.png 150w" sizes="(max-width: 220px) 100vw, 220px" /></a>

Pokud pečlivě počítáte, zjistíte, že segmentů je osm, a tím osmým je desetinná tečka (DP). Tu ale můžeme zanedbat.

Pro přenos hodnot 0 až 9 potřebujeme čtyři bity. Čtyři bity dokážou přenést hodnoty 0 až 15. Otázka je, co s hodnotami 10 až 15. Nejjednodušší způsob je prohlásit je za zakázané. Výsledný čtyřbitový kód se pak nazývá **kód BCD - Binary Coded Decimal**, tedy dvojkově kódované desítkové číslo. Dekodér BCD-to-7segment pak pracuje podle následující tabulky:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/7segtruth.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-287" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/7segtruth.png" alt="" width="347" height="229" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/7segtruth.png 347w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/7segtruth-300x198.png 300w" sizes="(max-width: 347px) 100vw, 347px" /></a>

Takové obvody fakt existovaly a existují, například 7447 (za socialismu se vyráběl v NDR pod označením D147D a představoval nejjednodušší způsob, jak vytvořit nějaký displej se sedmisegmentovkami).

## ALU

Asi nejsložitější obvody, které můžeme stvořit kombinační technikou (tedy bezestavové), jsou násobičky a aritmeticko-logické jednotky (ALU). Takové obvody mívají většinou dva vícebitové vstupy, jeden vícebitový výstup, a k tomu sadu řídicích vstupů, které určují, jakou operaci obvod právě dělá. Většinou mívá i jakési "vedlejší výstupy", třeba přenos nebo informaci o tom, že jsou si čísla rovna.

Reálným příkladem takového obvodu je obvod 74181. Tento obvod zpracovává dva čtyřbitové vstupy (A0-A3 a B0-B3) a nabízí sadu funkcí pro sčítání, odčítání, logické operace či posuny, a to s přenosem i bez přenosu. Funkce se vybírají pomocí čtyřbitového řídicího vstupu S0-S3. Pro zajímavost se podívejme na schéma:

<figure id="attachment_288" aria-describedby="caption-attachment-288" style="width: 800px" class="wp-caption aligncenter"><a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/800px-74181aluschematic.png" rel="lightbox"><img loading="lazy" class="size-full wp-image-288" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/800px-74181aluschematic.png" alt="" width="800" height="605" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/800px-74181aluschematic.png 800w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/800px-74181aluschematic-300x227.png 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/800px-74181aluschematic-768x581.png 768w" sizes="(max-width: 800px) 100vw, 800px" /></a><figcaption id="caption-attachment-288" class="wp-caption-text">[CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/ "Creative Commons Attribution-Share Alike 3.0"), <a href="https://commons.wikimedia.org/w/index.php?curid=168473" rel="lightbox">Link</a></figcaption></figure>

Sčítání dvou čísel zabralo tomuto obvodu 22 nanosekund. Verze "Schottky" 74S181 sčítala dvě čísla 11 nanosekund, rychlá verze 74F181 sedm nanosekund. Rychlost byla dána rychlostí přepínání hradel a tvořila technologický limit - nějaké "zvyšování kmitočtu" nepomohlo.

Funkce se volily jednak pomocí výběru S0-S3 a dále pak vstupem, který přepínal logické a aritmetické instrukce (M). Obvod pracoval buď v normálním režimu, nebo v negovaném. Za normálního režimu dokázal následující operace (Cn je přenos z nižšího řádu):

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/Functional-table-for-active-low-inputs-and-outputs.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-289" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/Functional-table-for-active-low-inputs-and-outputs.jpg" alt="" width="423" height="461" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/Functional-table-for-active-low-inputs-and-outputs.jpg 423w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/Functional-table-for-active-low-inputs-and-outputs-275x300.jpg 275w" sizes="(max-width: 423px) 100vw, 423px" /></a>

Násobení nevedeme... Násobilo se buď postupně, nebo existovala hardwarová násobička - velmi drahá a objemná součástka. Pro vícebitové operace se zapojovalo několik těchto obvodů vedle sebe a propojovaly se pomocí přenosu Cn. Protože zpoždění signálu mohlo ovlivnit výsledek, používal se obvod 74182 pro "předvídání přenosu".

Aritmeticko-logická jednotka je základem každého moderního procesoru. Dnes už je integrovaná přímo na čipu, ale v dřevních dobách let 60. a 70. se opravdu skládala z hradel, později z takovýchto málobitových jednotek vedle sebe.

Tímto jsme probrali ty nejjednodušší číslicové obvody - kombinační, neboli takové, jejichž výstup závisí jen a pouze na nastavení vstupů (při zanedbání zpoždění), nikoli na předchozím stavu, jak je tomu u obvodů sekvenčních. Pojďme si před další lekcí prověřit znalosti ve velkém kvízu...

[WpProQuiz 5]

_Těším se na vás u dalšího pokračování._