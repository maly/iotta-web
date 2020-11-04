---
id: 303
title: Základy číslicové techniky VII
date: 2017-04-12T18:30:17+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=303
permalink: /zaklady-cislicove-techniky-vii/
image: http://iotta.cz/wp-content/uploads/sites/17/2017/04/stažený-soubor-1.jpg
categories:
  - DIY
  - Hardware
tags:
  - Číslicová technika
---
[Minule](https://iotta.cz/zaklady-cislicove-techniky-vi/) jsme si ukázali kouzlo toho, čemu se říká zpětná vazba. Pojďme si říct na rovinu, že zpětná vazba je dobrý sluha, ale zlý pán. Může snadno způsobit to, že se celý obvod rozkmitá, začne fungovat úplně podivně, popřípadě přestane fungovat, a výsledek nebude předvídatelný. Zpětná vazba totiž do krásně deterministického světa kombinačních obvodů, kde je stav výstupů závislý jen a pouze na stavu vstupů, zavádí prvek zpětného působení výstupů na vstupy, a pokud vytvoříme něco jako "smyčku", vznikne nám obvod, jehož stav na výstupech není závislý jen na samotných vstupech, ale i na předchozím stavu obvodu. Ovšem tahle vlastnost není vždy jen negativní.

<!--more-->

Zpětná vazba je v elektronice, v číslicové technice i ve spoustě dalších oborů velmi užitečná. Na jednu stranu způsobí třeba to, že mikrofon strčený před zesilovač vygeneruje hnusné hlasité pískání, na druhou stranu když je vhodně zkrocená, tak může generovat pravidelné (i nepravidelné) impulsy a můžeme s ní navrhnout prvek, který si něco pamatuje. _(Ano, tušíte správně, základ všech polovodičových pamětí je zde!)_

Na to, abychom takové obvody drželi zkrátka a aby fungovaly tak, jak si představujeme, je potřeba dodržet určitá pravidla sebekázně. Minule jsme si ukázali, že u klopného obvodu R-S (Reset / Set) existuje takzvaný "hazadrní stav", totiž takový, v němž jsou oba vstupy aktivované a oba výstupy mají stejnou hodnotu. Pokud z takového stavu přepneme naráz oba vstupy do neaktivního stavu, nelze říct, v jakém stavu se bude obvod nacházet. Proto je záměrem konstruktérů hazardním vstupům předcházet.

Nejprve si rozšíříme náš klopný obvod R-S o takzvaný **povolovací vstup E** (Enable). Uděláme to jednoduše - na vstupy R a S připojíme hradla, která propustí řídicí signály R a S pouze v případě, že vstupní signál E bude log. 1:

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":true,<br /> "toolbox":[<br /> {"type":"DC"},<br /> {"type":"LED"},<br /> {"type":"PushOff"},<br /> {"type":"PushOn"},<br /> {"type":"Toggle"},<br /> {"type":"AND"},<br /> {"type":"NAND"},<br /> {"type":"OR"},<br /> {"type":"NOR"},<br /> {"type":"EOR"},<br /> {"type":"ENOR"}<br /> ],<br /> "devices":[<br /> {"type":"DC","id":"dev0","x":64,"y":8,"label":"DC"},<br /> {"type":"LED","id":"dev1","x":440,"y":24,"label":"Q"},<br /> {"type":"NOR","id":"dev2","x":352,"y":24,"label":"NOR1"},<br /> {"type":"NOR","id":"dev3","x":352,"y":176,"label":"NOR2"},<br /> {"type":"PushOn","id":"dev4","x":144,"y":8,"label":"Reset"},<br /> {"type":"PushOn","id":"dev5","x":144,"y":192,"label":"Set"},<br /> {"type":"DC","id":"dev6","x":64,"y":192,"label":"DC"},<br /> {"type":"LED","id":"dev7","x":440,"y":176,"label":"Q_"},<br /> {"type":"DC","id":"dev8","x":64,"y":96,"label":"DC"},<br /> {"type":"Toggle","id":"dev9","x":144,"y":96,"label":"Enable","state":{"on":false}},<br /> {"type":"AND","id":"dev10","x":264,"y":16,"label":"AND"},<br /> {"type":"AND","id":"dev11","x":264,"y":184,"label":"AND"}<br /> ],<br /> "connectors":[<br /> {"from":"dev1.in0","to":"dev2.out0"},<br /> {"from":"dev2.in0","to":"dev10.out0"},<br /> {"from":"dev2.in1","to":"dev3.out0"},<br /> {"from":"dev3.in0","to":"dev2.out0"},<br /> {"from":"dev3.in1","to":"dev11.out0"},<br /> {"from":"dev4.in0","to":"dev0.out0"},<br /> {"from":"dev5.in0","to":"dev6.out0"},<br /> {"from":"dev7.in0","to":"dev3.out0"},<br /> {"from":"dev9.in0","to":"dev8.out0"},<br /> {"from":"dev10.in0","to":"dev4.out0"},<br /> {"from":"dev10.in1","to":"dev9.out0"},<br /> {"from":"dev11.in0","to":"dev9.out0"},<br /> {"from":"dev11.in1","to":"dev5.out0"}<br /> ]<br /> }
</div>

Vidíte sami - do vlastního klopného obvodu se nedostane nic, pokud je E rovno 0. Pokud nastavíme E na 1, funguje obvod jako vždycky.

**TIP:** Tento postup si zapamatujte. Vždy, když potřebujete nějaký vstup, aktivní v logické 1, ošetřit tak, aby fungoval "jen někdy", použijte předřazené hradlo AND, a do něj zaveďte daný signál a řídicí (povolovací) signál E.

Máme teď hradlo R-S s povolovacím vstupem, ale to nijak neřeší hazardní stav (R=S=1). Zkuste přemýšlet - jak zařídit, aby nikdy nenastal stav, že R i S budou zároveň v logické 1? Tak samozřejmě, můžeme zapojit mezi oba vstupy hradlo AND, jehož výstup bude zároveň ovládat vstup E tak, že pokud nastane R=S=1, tak vstup E zavře... Nějak takto:

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":true,<br /> "toolbox":[<br /> {"type":"DC"},<br /> {"type":"LED"},<br /> {"type":"PushOff"},<br /> {"type":"PushOn"},<br /> {"type":"Toggle"},<br /> {"type":"AND"},<br /> {"type":"NAND"},<br /> {"type":"OR"},<br /> {"type":"NOR"},<br /> {"type":"EOR"},<br /> {"type":"ENOR"}<br /> ],<br /> "devices":[<br /> {"type":"DC","id":"dev0","x":64,"y":8,"label":"DC"},<br /> {"type":"LED","id":"dev1","x":528,"y":24,"label":"Q"},<br /> {"type":"NOR","id":"dev2","x":440,"y":24,"label":"NOR1"},<br /> {"type":"NOR","id":"dev3","x":440,"y":176,"label":"NOR2"},<br /> {"type":"DC","id":"dev4","x":64,"y":192,"label":"DC"},<br /> {"type":"LED","id":"dev5","x":528,"y":176,"label":"Q_"},<br /> {"type":"AND","id":"dev6","x":352,"y":16,"label":"AND"},<br /> {"type":"AND","id":"dev7","x":352,"y":184,"label":"AND"},<br /> {"type":"NAND","id":"dev8","x":200,"y":96,"label":"NAND"},<br /> {"type":"Toggle","id":"dev9","x":128,"y":192,"label":"Toggle","state":{"on":true}},<br /> {"type":"Toggle","id":"dev10","x":128,"y":8,"label":"Reset","state":{"on":true}}<br /> ],<br /> "connectors":[<br /> {"from":"dev1.in0","to":"dev2.out0"},<br /> {"from":"dev2.in0","to":"dev6.out0"},<br /> {"from":"dev2.in1","to":"dev3.out0"},<br /> {"from":"dev3.in0","to":"dev2.out0"},<br /> {"from":"dev3.in1","to":"dev7.out0"},<br /> {"from":"dev5.in0","to":"dev3.out0"},<br /> {"from":"dev6.in0","to":"dev10.out0"},<br /> {"from":"dev6.in1","to":"dev8.out0"},<br /> {"from":"dev7.in0","to":"dev8.out0"},<br /> {"from":"dev7.in1","to":"dev9.out0"},<br /> {"from":"dev8.in0","to":"dev10.out0"},<br /> {"from":"dev8.in1","to":"dev9.out0"},<br /> {"from":"dev9.in0","to":"dev4.out0"},<br /> {"from":"dev10.in0","to":"dev0.out0"}<br /> ]<br /> }
</div>

Což je řešení, které téměř nemá chybu. Ve skutečnosti má chybu zásadní, a to tu, že řídicí signál vzniká až v hradle NAND. A hradlo má, jak už víme, nějaké zpoždění, takže hazardní stav R=S=1 projde přes obě hradla o chviličku dřív, než je stihne nula na výstupu E "zabouchnout".

Použijeme tedy jiný trik: zrušíme dva vstupy R a S, a budeme používat jen jeden. Ten připojíme na oba vstupy - na vstup S přímo, na vstup R přes invertor. Takový vstup nazveme D (jako že "data"). Pokud bude D=0, bude S taky rovno 0 a R rovno 1. Pokud bude D=1, bude S=1 a R=0. A takto vygenerované signály pošleme do obvodu spolu s povolovacím vstupem E, jak jsme si ho ukázali výše. Vstup E se v takové konfiguraci označuje C (jako Clock - hodiny).

<div class="simcir">
  {<br /> "width":700,<br /> "height":400,<br /> "showToolbox":true,<br /> "toolbox":[<br /> {"type":"DC"},<br /> {"type":"LED"},<br /> {"type":"PushOff"},<br /> {"type":"PushOn"},<br /> {"type":"Toggle"},<br /> {"type":"BUF"},<br /> {"type":"NOT"},<br /> {"type":"AND"},<br /> {"type":"NAND"},<br /> {"type":"OR"},<br /> {"type":"NOR"},<br /> {"type":"EOR"},<br /> {"type":"ENOR"}<br /> ],<br /> "devices":[<br /> {"type":"DC","id":"dev0","x":64,"y":8,"label":"DC"},<br /> {"type":"LED","id":"dev1","x":528,"y":24,"label":"Q"},<br /> {"type":"NOR","id":"dev2","x":440,"y":24,"label":"NOR1"},<br /> {"type":"NOR","id":"dev3","x":440,"y":176,"label":"NOR2"},<br /> {"type":"DC","id":"dev4","x":64,"y":192,"label":"DC"},<br /> {"type":"LED","id":"dev5","x":528,"y":176,"label":"Q_"},<br /> {"type":"AND","id":"dev6","x":352,"y":16,"label":"AND"},<br /> {"type":"AND","id":"dev7","x":352,"y":184,"label":"AND"},<br /> {"type":"Toggle","id":"dev8","x":128,"y":8,"label":"D","state":{"on":true}},<br /> {"type":"PushOn","id":"dev9","x":128,"y":192,"label":"Clock"},<br /> {"type":"NOT","id":"dev10","x":248,"y":8,"label":"NOT"}<br /> ],<br /> "connectors":[<br /> {"from":"dev1.in0","to":"dev2.out0"},<br /> {"from":"dev2.in0","to":"dev6.out0"},<br /> {"from":"dev2.in1","to":"dev3.out0"},<br /> {"from":"dev3.in0","to":"dev2.out0"},<br /> {"from":"dev3.in1","to":"dev7.out0"},<br /> {"from":"dev5.in0","to":"dev3.out0"},<br /> {"from":"dev6.in0","to":"dev10.out0"},<br /> {"from":"dev6.in1","to":"dev9.out0"},<br /> {"from":"dev7.in0","to":"dev8.out0"},<br /> {"from":"dev7.in1","to":"dev9.out0"},<br /> {"from":"dev8.in0","to":"dev0.out0"},<br /> {"from":"dev9.in0","to":"dev4.out0"},<br /> {"from":"dev10.in0","to":"dev8.out0"}<br /> ]<br /> }
</div>

Když píšu "hodiny", nepředstavujte si prosím nic sofistikovaného, natož švýcarského natahovacího se strojkem. Je to prostě jen jeden vodič, na kterém se plus mínus pravidelně střídají 0 a 1. Tento signál podobně jako srdeční tep řídí pak celý obvod. V zásadě má funkci takovou, že říká, kdy nějaký obvod smí změnit svůj stav. A protože u většiny zapojení bývá opravdu pravidelný a přesný a určuje rytmus vnitřních dějů, říká se mu "hodinový puls".

Výchozí stav je takový, že vstup Clock je 0. Výstupy zůstávají stále ve stejné úrovni. Jakmile je Clock=1, nastaví se výstupy podle vstupu D. Pokud je nula, bude Q=0 a Q\_=1, pokud je jedna, bude Q=1 a Q\_=0. Dokud bude Clock=1, budou výstupy reagovat na vstup D. Jakmile přepneme vstup Clock zpátky na 0, zůstane obvod v posledním nastaveném stavu.

Výsledek se nazývá "klopný obvod D". Jeho funkce je popsána předchozím odstavcem. Můžeme si to představit jako paměťovou buňku pro jeden bit informace. Přivedeme tento bit na vstup D (buď 1, nebo 0), a "zapamatujeme" si jej tak, že na vstup Clock přivedeme krátký impuls. Impulsem je míněn přechod z 0 do 1 a opět zpátky na 0. Nějak takto:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/04/path3680.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-307" src="http://iotta.cz/wp-content/uploads/sites/17/2017/04/path3680.png" alt="" width="514" height="123" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/04/path3680.png 514w, https://iotta.cz/wp-content/uploads/sites/17/2017/04/path3680-300x72.png 300w" sizes="(max-width: 514px) 100vw, 514px" /></a>

Zapamatovaná hodnota je na výstupu Q, na výstupu Q_ (NOT Q, /Q, jak chcete...) je jeho negovaná hodnota.

U takového zapojení hovoříme o **synchronním** vstupu D. Slovo "synchronní" znamená, že se jeho změny projeví v obvodu až poté, co nastane nějaká událost - zde příchod hodinového pulsu. Pouze pokud přijde puls, může se vstup nějak projevit. Naproti tomu vstupy R, S u klopného obvodu R-S jsou vstupy **asynchronní** - změna na těchto vstupech se okamžitě projeví na výstupech.

Jak jsem psal už na začátku kapitoly: pokud je ve hře zpětná vazba, je potřeba ji zkrotit a dát ji zcela jasné mantinely. Takovým mantinelem jsou v číslicových systémech právě hodinové pulsy. Hodinové pulsy _synchronizují_ celý obvod a zajišťují, že se něco stane až po něčem jiném, že změny nastanou ve stejný okamžik, že jedna část obvodu se nezmění jindy než jiná, a pokud ano, tak že to nebude mít vliv na výstup, a tak dále...

Proto u složitějších obvodů vždy všechno řídíme hodinovými pulsy. Nechceme, aby se cokoli měnilo halabala, protože pak můžou nastat hazardní stavy. Vždy všechno důsledně řízené hodinami a synchronní. Asynchronní vstup znamená vždy výjimečnou událost, a jako takový by měl být používaný. RESET či přerušení je asynchronní událost, vše ostatní by mělo být synchronní. Jakmile ztratíme synchronizaci, rozsype se funkce celého obvodu, a to znamená třeba ztrátu dat při přenosu, vznik falešných dat a podobné chyby funkčnosti.

## Reálný klopný obvod 7474

Samozřejmě že máme k dispozici popsaný klopný obvod typu D jako součástku. A ano, dodneška se používá. Nejznámější obvod je 7474 (a my už víme, že to může být i 74LS74, nebo 74HCT74). Tento obvod obsahuje dvojici klopných obvodů typu D. Jeho datasheet naleznete třeba zde: [sn74ls74a](http://www.ti.com/lit/ds/symlink/sn74ls74a.pdf). V popisu je napsáno, že jde o "Dual D-type Positive-Edge-Triggered Flip-Flops with Preset and Clear". Pojďme si to v rámci cvičení přeložit.

"Dual" naznačuje, že tam jsou dva obvody stejného typu. Česky "dvojitý".

"D-type Flip-Flop" říká, že jde o klopný obvod (flip-flop - ne, vážně, takhle se v anglické literatuře říká klopným obvodům) typu D.

"Positive edge triggered" si rozluštíme jako "spouštěný náběžnou hranou". Trigger je spoušť, tady to symbolizuje onu akci, která se provede - třeba "zapamatování údajů". Positive edge je takzvaná "náběžná hrana". Když se podíváte na červenou vizualizaci hodinového pulsu, tak ta svislá čára vlevo, jak stoupá zespoda nahoru, tomu se říká "náběžná" (též "vzestupná") hrana hodinového pulsu. Ta druhá, přechod shora dolů, je sestupná hrana, anglicky pak "negative edge". Někdy se setkáte i s termíny "falling edge" (sestupná) a "lead edge" (náběžná).

"with preset and clear" říká, že jde o klopný obvod D, který má nastavovací a nulovací vstup. Někdy se dodává, aby to bylo jasnější, i slovo "asynchronous preset and clear". Jde v podstatě o naše vstupy R a S, jen s tím rozdílem, že jsou připojeny přímo do klopného obvodu a nečekají až na hodinový impuls.

Schéma je ve skutečnosti o něco složitější:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/04/7474-crop.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-308" src="http://iotta.cz/wp-content/uploads/sites/17/2017/04/7474-crop.jpg" alt="" width="600" height="571" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/04/7474-crop.jpg 600w, https://iotta.cz/wp-content/uploads/sites/17/2017/04/7474-crop-300x286.jpg 300w" sizes="(max-width: 600px) 100vw, 600px" /></a>

Naštěstí se jím nemusíme zabývat. Nám stačí znát funkční tabulku:

<table style="width: 400px;" border="1">
  <tr>
    <th style="text-align: center;" colspan="4">
      Vstupy
    </th>
    
    <th style="text-align: center;" colspan="2">
      Výstupy
    </th>
  </tr>
  
  <tr>
    <th style="text-align: center;">
      /PRE
    </th>
    
    <th style="text-align: center;">
      /CLR
    </th>
    
    <th style="text-align: center;">
      CLK
    </th>
    
    <th style="text-align: center;">
      D
    </th>
    
    <th style="text-align: center;">
      Q
    </th>
    
    <th style="text-align: center;">
      /Q
    </th>
  </tr>
  
  <tr>
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      X
    </td>
    
    <td style="text-align: center;">
      X
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      X
    </td>
    
    <td style="text-align: center;">
      X
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      X
    </td>
    
    <td style="text-align: center;">
      X
    </td>
    
    <td style="text-align: center;">
      1*
    </td>
    
    <td style="text-align: center;">
      1*
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
      ^
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
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
      ^
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
      1
    </td>
    
    <td style="text-align: center;">
      1
    </td>
    
    <td style="text-align: center;">
    </td>
    
    <td style="text-align: center;">
      X
    </td>
    
    <td style="text-align: center;">
      Q<sub></sub>
    </td>
    
    <td style="text-align: center;">
      /Q<sub></sub>
    </td>
  </tr>
</table>

Nerozumíte? Vysvětlení je zde:

Vstupy /PRE a /CLR jsou zmiňované "preset" (="nastav na 1") a "nuluj" (="nastav na 0"). Lomítko před názvem znamená, jak už jsme si řekli, negaci, takže klidový stav je 1, a požadovanou akci spustíme hodnotou 0. Zatím jsme se setkávali s pozitivními signály, tedy takovými, kdy 0 znamenala "klid" a 1 nějakou akci. U negovaných vstupů to je obráceně. Některé signály se v číslicové technice používají právě v takovéto negované podobě. Má to své důvody, které jsem popisoval v [odbočce](https://iotta.cz/zaklady-cislicove-techniky-ii/), ale nemusíte je nutně znát. Důležité je pamatovat si, že existují, a jak s nimi pracovat.

První řádek ukazuje stav, kdy je aktivní signál PRESET (na vstup /PRE je přivedena logická 0, na vstupu /CLR je logická 1, tedy "klid"). Vstupy CLK a D mohou být v libovolné úrovni, což značíme symbolem X. V takovém případě se obvod "nastaví", na výstupu bude 1, na negovaném výstupu bude tedy 0.

Druhý řádek. Je aktivní signál CLEAR (tedy /CLR je 0, /PRE je 1, CLK a D libovolné). Situace je analogická předchozí, ale obráceně: obvod se "vynuluje", na výstupu bude 0, na negovaném výstupu 1.

Třetí řádek označuje hazardní stav. Oba asynchronní vstupy PRE i CLR jsou aktivní (tedy v úrovni 0), oba výstupy jsou v logické 1, a v datasheetu je ještě u toho poznámka, že nelze zaručit správnou úroveň napětí na výstupu, takže do takového stavu by se neměl obvod dostávat.

Poslední tři řádky popisují situaci, kdy jsou asynchronní vstupy v klidu (=log. 1). V takové situaci obvod funguje tak, jak jsme si popsali výše. Pokud je CLK rovno 0, tak bez ohledu na stav vstupu D je na výstupech to, co tam bylo předtím (poslední řádek tabulky). Změna probíhá s náběžnou hranou signálu CLK (symbolizováno stříškou ^) - pokud je D=1, bude i Q=1 (a /Q logicky 0), pokud je D=0, bude Q=0 (a /Q=1).

Reálná součástka je v pouzdře DIL se 14 vývody, které jsou zapojeny takto:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/04/screenshot-www.ti_.com-2017-04-12-20-15-20.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-312" src="http://iotta.cz/wp-content/uploads/sites/17/2017/04/screenshot-www.ti_.com-2017-04-12-20-15-20.png" alt="" width="149" height="129" /></a>

A protože jsou uvnitř dva klopné obvody, jsou i všechny vstupy a výstupy v obvodu dvakrát. Při pohledu shora vidíte vlevo vývody obvodu 1 postupně pod sebou: /CLR, D, CLK, /PRE, Q a /Q. Vpravo je totéž pro obvod 2.

Možná vás mate, že hodiny se tu značí CLK, zatímco já vám výš tvrdil, že se značí C. Víte, ve skutečnosti je to jedno - CLK nebo C, obojí je přijatelné, když je jasné, o čem je řeč. Někdy se setkáte i s označením CK a někdy ještě jinak, ale vždy je někde napsáno, že se jedná o hodinový vstup (clock).

Pro dnešek stačí. Beztak máte hlavu jak pátrací balón a musíte informace vstřebat. Na příště si připravte svá nepájivá kontaktní pole, půjdeme do reálné akce!

_Těším se na vás u dalšího pokračování - to bude tentokrát praktické, protože doufám, že vám už dorazily součástky!_