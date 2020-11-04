---
id: 172
title: Základy číslicové techniky I
date: 2017-02-18T13:12:19+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=172
permalink: /zaklady-cislicove-techniky-i/
image: http://iotta.cz/wp-content/uploads/sites/17/2017/02/TI_SN7400N.jpg
categories:
  - DIY
  - Hardware
tags:
  - Číslicová technika
---
<p class="message-body">
  Kdysi před lety jsem napsal článek <a href="https://retrocip.cz/exkurze-mezi-jednicky-a-nuly/">Exkurze mezi jedničky a nuly</a>, kde jsem trošku nastiňoval, jak to s těmi číslicovými obvody je. Dodneška se mě na to lidé ptají, a já mám dojem, že to je docela jednoduchá záležitost a není důvod zájemce odradit nějakými složitými teoriemi. Podle mého názoru má zrovna číslicová technika docela hezkou učící křivku, a i když se nestanete mistrem a guru po seznámení se základy, to nevadí. Důležité je takříkajíc "chytit slinu". A přesně to mám v úmyslu udělat teď.
</p>

<p class="message-body">
  <!--more-->
</p>

## Trocha teorie být musí {.message-body}

Já vím, že sám hlásám: _pojďme od příkladu k teorii, nejdřív praktická ukázka, pak teoretický základ, pak jeho ověření v další praxi,_ ale tentokrát se toho držet nebudu. Musíme si totiž na začátku vymezit oblast, ve které se hodláme pohybovat.

**Číslicová** elektronika, též **digitální**, pracuje převážně se signály ve dvojkové soustavě (_kdo neznáte, doučit. Jako fakt!_) V praxi to znamená, že máme nějaký vodič, _signál_, a v něm buď je napětí vůči zemi, nebo není. Pokud je, říkáme, že signál má úroveň logické jedničky, pokud není, je to logická nula.

> _Není to tak vždy. Jsou systémy, kde to je přesně obráceně. Jsou i systémy, které používají záporné a kladné napětí. Ale to, co jsem popsal, je v současnosti nejpoužívanější, a pokud nezabrousíte do hodně speciální oblasti, tak se s jiným způsobem nepotkáte. Ale je dobré to vědět._

Když říkám "napětí", měl bych taky říct kolik voltů: má to být 230, nebo 48, nebo 15, nebo 12, nebo 1.5...? Odpověď zní: To záleží! Historicky nejrozšířenější obvody TTL používaly takzvanou pětivoltovou logiku, to znamená 0V = logická 0, +5V = logická 1. Jiné rodiny obvodů (CMOS) dokážou pracovat i s jinými hodnotami, ale z důvodu kompatibility se bere "pětivoltová logika" jako základ, a dodneška se s ní setkáte.

> _Postupem času vznikly i další normy: 3.3V, 1.8V a tak. Díky nižšímu napětí klesla spotřeba zařízení, a zjednodušeně platí: čím modernější a složitější stroj, tím nižší napětí bude asi používat. Problém nastává při zapojování novějších typů obvodů do starších systémů, protože třeba obvod, jehož pracovní napětí je 1.8V, po zapojení do obvodu s napájecím napětím 5V nejspíš shoří a přestane fungovat. Naopak 5V obvod v nízkonapěťovém systému zase nedostane to, co potřebuje, a nebude fungovat (ale aspoň se nezničí)._

Posílat po vodičích napětí je sice fajn, ale taky je potřeba ty signály nějak zpracovat, protože bez toho máme jen obyčejné blikátko:



Přepínače vlevo jsou pro nás "zdroj signálu". Když jsou v klidu, jsou "dole", a spodní vstup je připojen na zem, tedy logickou nulu. Když kliknete myší, přepnou se "nahoru", na vstup, který je připojen na napájecí napětí, tím pádem skrz ně projde proud do vedení, skrz vedení do LEDky, skrz LEDku do rezistoru a pak do země. Obvod je uzavřen, proud teče, LED svítí!

Zjednodušené zapojení vidíte v dalším simulátoru. Místo přepínačů jsou jen prostá tlačítka, která jsou buď rozpojená, tedy proud neteče, nebo spojená, a proud může téct.



Zatím to bylo jednoduché, že? Na druhou stranu jsme taky zatím nic nepředvedli.

<figure id="attachment_210" aria-describedby="caption-attachment-210" style="width: 410px" class="wp-caption aligncenter"><a href="http://iotta.cz/wp-content/uploads/sites/17/2017/02/schem_zn.jpg" rel="lightbox"><img loading="lazy" class="wp-image-210 size-full" src="http://iotta.cz/wp-content/uploads/sites/17/2017/02/schem_zn.jpg" width="410" height="124" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/02/schem_zn.jpg 410w, https://iotta.cz/wp-content/uploads/sites/17/2017/02/schem_zn-300x91.jpg 300w" sizes="(max-width: 410px) 100vw, 410px" /></a><figcaption id="caption-attachment-210" class="wp-caption-text">Úryvek z tabulky schematických značek...</figcaption></figure>

## Logické funkce

Možná znáte z matematiky, možná znáte z programování, možná odjinud. Někde se tomu říká i _Booleova algebra_, a pokud vám to připomíná typ Boolean, jste na správné stopě. Dovolte drobné opakování.

Máme dvě základní logické funkce: AND a OR. AND se nazývá taky česky "A" nebo "logický součin", OR je česky "NEBO" či "logický součet". V základním tvaru to jsou funkce se dvěma parametry, které značíme A a B, a jedním výsledkem, značeným Y. Někdy se můžete setkat i se zápisem pomocí značek ∨ a ∧ (NEBO, A)

Pro OR platí, že výsledek (Y) je 1, pokud **alespoň jeden** parametr je 1 (tedy "a **nebo** b" - nesplést s exkluzivním OR, kde je podmínka "**buď **a, **nebo** b"). Pro AND platí, že výsledek je 1, pokud jsou **obě** vstupní hodnoty rovny 1. Zapíšeme si to do tabulky pro všechny dostupné kombinace:

<table style="width: 200px;" border="1" cellspacing="1">
  <caption> </caption> <tr>
    <th>
      A
    </th>
    
    <th>
      B
    </th>
    
    <th>
      OR
    </th>
    
    <th>
      AND
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
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
</table>

<div>
  Máme čtyři možné kombinace, do jakých se vstupy mohou (legitimně) dostat. Z tabulky je vidět, že obě funkce jsou <em>komutativní</em>, to znamená, že když jim prohodíte hodnoty A a B, bude to jedno.
</div>

> <div>
>   <em>Neexistuje žádná hodnota "0.5", třeba že bychom pustili "ne úplně napájecí napětí, ale jen trochu". Ne, buď 0, nebo 1, a pokud bude na vstupu nejasná hodnota, bude výsledek podivný.</em>
> </div>

<div>
  Třetí logická funkce do party je logická funkce NOT. NOT je logická negace (značí se ¬ před parametrem) - je to funkce s jedním parametrem a jednou hodnotou, a pokud je parametr 0, je hodnota 1 a obráceně:
</div>

<table style="width: 100px;" border="1" cellspacing="1">
  <caption> </caption> <tr>
    <th>
      A
    </th>
    
    <th>
      NOT
    </th>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <strong></strong>
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
    </td>
  </tr>
</table>

Asi už tušíte, že o funkcích nepíšu jen tak zbůhdarma, a máte pravdu. Tyto základní logické funkce mají totiž svou fyzickou podobu - existují číslicové obvody, které dělají přesně tyto operace. Vstupy i výstupy jsou signálové vodiče (opravdové fyzické spoje), a součástka dělá AND, OR, NOT, NAND, NOR, ...

Moment, NAND? NOR?

Ano, ve světě číslicové techniky jsme si rozšířili základní typy logických operací na 4. K AND existuje NAND, tedy "negovaný AND", a k OR existuje NOR, čili "negovaný OR". Zkrátka za výstupem je ještě zapojený invertor, takže tabulka hodnot vypadá takto:

<table style="width: 200px;" border="1" cellspacing="1">
  <caption> </caption> <tr>
    <th>
      A
    </th>
    
    <th>
      B
    </th>
    
    <th>
      OR
    </th>
    
    <th>
      NOR
    </th>
    
    <th>
      AND
    </th>
    
    <th>
      NAND
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
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
</table>

V dalším zjednodušeném simulátoru si můžete vyzkoušet chování těchto čtyř typů obvodů (říkáme jim **hradla**) při změně vstupních signálů. Máme tu dva spínače, A a B, čtyři hradla a čtyři LED, které signalizují výstupní úroveň. Zkuste si přepínat a pozorujte, jak se chovají výstupy.

<div class="simcir">
  { "width":700, "height":400, "showToolbox":false, "devices":[ {"type":"LED","id":"dev0","x":400,"y":48,"label":"LED","color":"#ff0000"}, {"type":"AND","id":"dev1","x":304,"y":48,"label":"AND"}, {"type":"NAND","id":"dev2","x":304,"y":104,"label":"NAND"}, {"type":"LED","id":"dev3","x":400,"y":104,"label":"LED","color":"#ff0000"}, {"type":"OR","id":"dev4","x":304,"y":160,"label":"OR"}, {"type":"NOR","id":"dev5","x":304,"y":216,"label":"NOR"}, {"type":"Toggle","id":"dev6","x":136,"y":96,"label":"A","state":{"on":false}}, {"type":"LED","id":"dev7","x":400,"y":160,"label":"LED","color":"#ff0000"}, {"type":"LED","id":"dev8","x":400,"y":216,"label":"LED","color":"#ff0000"}, {"type":"DC","id":"dev9","x":56,"y":136,"label":"+5V"}, {"type":"Toggle","id":"dev10","x":136,"y":168,"label":"B","state":{"on":false}} ], "connectors":[ {"from":"dev0.in0","to":"dev1.out0"}, {"from":"dev1.in0","to":"dev6.out0"}, {"from":"dev1.in1","to":"dev10.out0"}, {"from":"dev2.in0","to":"dev6.out0"}, {"from":"dev2.in1","to":"dev10.out0"}, {"from":"dev3.in0","to":"dev2.out0"}, {"from":"dev4.in0","to":"dev6.out0"}, {"from":"dev4.in1","to":"dev10.out0"}, {"from":"dev5.in0","to":"dev6.out0"}, {"from":"dev5.in1","to":"dev10.out0"}, {"from":"dev6.in0","to":"dev9.out0"}, {"from":"dev7.in0","to":"dev4.out0"}, {"from":"dev8.in0","to":"dev5.out0"}, {"from":"dev10.in0","to":"dev9.out0"} ]}
</div>

Všimli jste si asi schematických značek pro jednotlivá hradla. Zde jsou shrnuta v obrázku (včetně hradla XOR, k němuž se ještě dostaneme). Schematické značky, co používáme, jsou podle normy ANSI. V ČSSR se koncem osmdesátých let začala prosazovat norma IEC, kde všechny vypadají jako obdélníčky se symbolem, setkáte se s oběma typy. Jedno mají společné: Negovaný vstup nebo výstup se značí kroužkem:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/02/arch1_2.gif" rel="lightbox"><img loading="lazy" class="aligncenter wp-image-207 size-full" src="http://iotta.cz/wp-content/uploads/sites/17/2017/02/arch1_2.gif" width="400" height="168" /></a>

Ve skutečnosti, věřte mi nebo ne, si v celé číslicové technice vystačíte s jedním jediným obvodem! Vše ostatní si z něj dokážete poskládat. Tímto obvodem je nejčastěji NAND (mohl by teoreticky být i NOR). Všimněte si tabulky obvodu NAND a přemýšlejte: Co se stane, když spojíte oba vstupy dohromady a na oba přivedete stejnou hodnotu? Jak se bude obvod chovat? Podívejte se na první a poslední řádek tabulky - nepřipomíná vám to nic? No jasně, získáte tak invertor!

A když zapojíte invertor na výstup hradla NAND, dostanete? Jasně, AND!

A co když invertujete vstupy hradla NAND? Jaký bude výsledek? (Napovím: všimněte si v tabulce podobnosti mezi sloupcem NAND a dalšími sloupci... A všimněte si, že při invertování A a B dostanete tutéž tabulku, ale pozpátku...) Podívejte se na simulaci, a pak si otestujte vědomosti v kvízu.

<div class="simcir">
  {"width":700,"height":400,"showToolbox":false,<br /> "devices":[<br /> {"type":"NAND","id":"dev0","x":248,"y":56,"label":"NAND"},<br /> {"type":"LED","id":"dev1","x":312,"y":56,"label":"LED"},<br /> {"type":"PushOn","id":"dev2","x":152,"y":56,"label":"PushOn"},<br /> {"type":"DC","id":"dev3","x":80,"y":56,"label":"DC"},<br /> {"type":"NAND","id":"dev4","x":312,"y":168,"label":"NAND"},<br /> {"type":"LED","id":"dev5","x":368,"y":168,"label":"LED"},<br /> {"type":"NAND","id":"dev6","x":240,"y":168,"label":"NAND"},<br /> {"type":"NAND","id":"dev7","x":176,"y":144,"label":"NAND"},<br /> {"type":"NAND","id":"dev8","x":176,"y":192,"label":"NAND"},<br /> {"type":"DC","id":"dev9","x":32,"y":144,"label":"DC"},<br /> {"type":"Toggle","id":"dev10","x":88,"y":144,"label":"Toggle","state":{"on":false}},<br /> {"type":"Toggle","id":"dev11","x":88,"y":200,"label":"Toggle","state":{"on":false}},<br /> {"type":"DC","id":"dev12","x":32,"y":200,"label":"DC"}<br /> ],<br /> "connectors":[<br /> {"from":"dev0.in0","to":"dev2.out0"},<br /> {"from":"dev0.in1","to":"dev2.out0"},<br /> {"from":"dev1.in0","to":"dev0.out0"},<br /> {"from":"dev2.in0","to":"dev3.out0"},<br /> {"from":"dev4.in0","to":"dev6.out0"},<br /> {"from":"dev4.in1","to":"dev6.out0"},<br /> {"from":"dev5.in0","to":"dev4.out0"},<br /> {"from":"dev6.in0","to":"dev7.out0"},<br /> {"from":"dev6.in1","to":"dev8.out0"},<br /> {"from":"dev7.in0","to":"dev10.out0"},<br /> {"from":"dev7.in1","to":"dev10.out0"},<br /> {"from":"dev8.in0","to":"dev11.out0"},<br /> {"from":"dev8.in1","to":"dev11.out0"},<br /> {"from":"dev10.in0","to":"dev9.out0"},<br /> {"from":"dev11.in0","to":"dev12.out0"}<br /> ]<br /> }
</div>

[WpProQuiz 1]

## Ještě troška teorie...

Tomu, co jsme si teď popsali, se teoreticky a vznešeněji říká _de Morganovy zákony_, a formulují se takto: Negace součinu je rovna součtu negací, negace součtu je rovna součinu negací. Když se vrátíme k matematickému zápisu:

¬ A ∧ ¬ B = ¬ (A ∨ B)  - -  _NOT A AND NOT B = NOT (A OR B)_  
¬ A ∨ ¬ B = ¬ (A ∧ B)  - -  _NOT A OR NOT B = NOT (A AND B)_

My si z toho můžeme vzít jednoduché ponaučení: z NAND se invertováním vstupů stane OR, z NOR se stane AND.

## Domácí úkol

Za domácí úkol se pokuste z hradel NAND vytvořit hradlo typu XOR. O funkci XOR jsme se ještě nebavili, ale pokud programujete, tak ji možná znáte. Její výsledek je 0, pokud mají vstupy stejnou hodnotu (všechny jsou 0, nebo všechny jsou 1). Pokud mají vstupy hodnotu rozdílnou, je výsledek 1. A pokud jste si zkusili kvíz, tak jste si vytvořili i pravdivostní tabulku pro tuto funkci. Máte k dispozici čtyři hradla NAND.

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":false,<br /> "devices":[<br /> {"type":"LED","id":"dev0","x":368,"y":168,"label":"LED"},<br /> {"type":"NAND","id":"dev1","x":176,"y":144,"label":"NAND"},<br /> {"type":"DC","id":"dev2","x":32,"y":144,"label":"DC"},<br /> {"type":"Toggle","id":"dev3","x":88,"y":144,"label":"Toggle","state":{"on":false}},<br /> {"type":"Toggle","id":"dev4","x":88,"y":200,"label":"Toggle","state":{"on":false}},<br /> {"type":"DC","id":"dev5","x":32,"y":200,"label":"DC"},<br /> {"type":"NAND","id":"dev6","x":176,"y":200,"label":"NAND"},<br /> {"type":"NAND","id":"dev7","x":176,"y":88,"label":"NAND"},<br /> {"type":"NAND","id":"dev8","x":176,"y":256,"label":"NAND"}<br /> ],<br /> "connectors":[<br /> {"from":"dev3.in0","to":"dev2.out0"},<br /> {"from":"dev4.in0","to":"dev5.out0"}<br /> ]<br /> }
</div>

## Konec prvního dílu

Zatím to moc složité nebylo, že? To je dobře,  protože už vás nic složitějšího nečeká. Z těchto obvodů totiž poskládáte naprosto vše. Z hradel kombinační obvody - multiplexery, dekodéry, sčítačky. Taky sekvenční obvody - klopné obvody, čítače, registry. S nimi už dokážete vytvořit paměť i procesor... Naprosto vážně!

Pojďme si ještě dát malou odbočku. Zatím to vypadalo všechno příliš abstraktně, ale věřte tomu, že opravdu existují obvody, které realizují přesně tyhle funkce. Už jsem na začátku zmínil zkratku "TTL". To byla svého času nejrozšířenější rodina logických obvodů, a dodneška se s ní lze setkat. Nejznámější obvody byly ty od Texas Instruments řady 74xx - vyráběly je i další firmy, včetně tehdejších zemí RVHP. TESLA například měla řadu MH74xx. Nejnižší číslo 7400 nesl obvod, který obsahoval čtyři hradla NAND.

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/02/7400.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-200" src="http://iotta.cz/wp-content/uploads/sites/17/2017/02/7400-243x300.jpg" alt="" width="243" height="300" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/02/7400-243x300.jpg 243w, https://iotta.cz/wp-content/uploads/sites/17/2017/02/7400.jpg 434w" sizes="(max-width: 243px) 100vw, 243px" /></a>

Ve stavebnici Logitronik byl právě jeden takový obvod, a zapojoval se přesně tak, jak jsme si popsali (a ještě popíšeme). Z řady 74xx vznikly další řady: 54xx, 84xx (odpovídaly řadě 74xx, ale měly vyšší výdrž a odolnost), 74Sxx (rychlejší, ale s vyšší spotřebou), 74Lxx (s nižší spotřebou, zato pomalejší), 74LSxx (rychlejší, ale se spotřebou jen o něco vyšší), 74ALSxx, 74HCxx (CMOS obvody se stejným rozložením vývodů), 74HCTxx (taky CMOS, ale se stejnými úrovněmi jako TTL), 74Fxx... Obecně ale platí, že 74LS00 má stejnou funkci jako 7400, stejnou jako 74ALS00, stejně jako 74HCT00... pouze se liší parametry.

Kdybyste si chtěli náhodou funkci NAND zkusit, není problém. Na nepájivém kontaktním poli si můžete poskládat přesně takový obvod. Nezapomeňte připojit vývod 7 na zem a vývod 14 na napájecí napětí.

<div id='gallery-1' class='gallery galleryid-172 gallery-columns-5 gallery-size-thumbnail gallery1'>
  <figure class="gallery-item"> 
  
  <div class="gallery-icon">
    <a href="http://iotta.cz/wp-content/uploads/sites/17/2017/02/20170218_134003.jpg" title="Použil jsem typ 74HCT00. Prostě jsem ho měl po ruce" rel="gallery1"><img src="http://iotta.cz/wp-content/uploads/sites/17/2017/02/20170218_134003-150x150.jpg" width="150" height="150" alt="Použil jsem typ 74HCT00. Prostě jsem ho měl po ruce" /></a>
  </div><figcaption class="gallery-caption" id="caption205">
  
  <span class="imagecaption">Použil jsem typ 74HCT00. Prostě jsem ho měl po ruce</span><br /> </figcaption></figure><figure class="gallery-item"> 
  
  <div class="gallery-icon">
    <a href="http://iotta.cz/wp-content/uploads/sites/17/2017/02/20170218_133955.jpg" title="Napájecí napětí jsem vzal z Arduina" rel="gallery1"><img src="http://iotta.cz/wp-content/uploads/sites/17/2017/02/20170218_133955-150x150.jpg" width="150" height="150" alt="Napájecí napětí jsem vzal z Arduina" /></a>
  </div><figcaption class="gallery-caption" id="caption202">
  
  <span class="imagecaption">Napájecí napětí jsem vzal z Arduina</span><br /> </figcaption></figure><figure class="gallery-item"> 
  
  <div class="gallery-icon">
    <a href="http://iotta.cz/wp-content/uploads/sites/17/2017/02/20170218_133921.jpg" title="Jeden vstup 1, jeden 0, výsledek: Svítí!" rel="gallery1"><img src="http://iotta.cz/wp-content/uploads/sites/17/2017/02/20170218_133921-150x150.jpg" width="150" height="150" alt="Jeden vstup 1, jeden 0, výsledek: Svítí!" /></a>
  </div><figcaption class="gallery-caption" id="caption204">
  
  <span class="imagecaption">Jeden vstup 1, jeden 0, výsledek: Svítí!</span><br /> </figcaption></figure><figure class="gallery-item"> 
  
  <div class="gallery-icon">
    <a href="http://iotta.cz/wp-content/uploads/sites/17/2017/02/20170218_133937.jpg" title="" rel="gallery1"><img src="http://iotta.cz/wp-content/uploads/sites/17/2017/02/20170218_133937-150x150.jpg" width="150" height="150" alt="" /></a>
  </div></figure>
</div>

## Anketa

Prosím, najděte si minutu času a [odpovězte na pár otázek](https://docs.google.com/forms/d/e/1FAIpQLSeJTL1lzI7_5rh9Xp2Yws4PB81fTX7qBZMnFdw0DYDKaJOdlg/viewform?entry.525375248), které mohou ovlivnit další podobu kurzu. Děkuju.