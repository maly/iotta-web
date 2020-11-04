---
id: 41
title: Výukový shield pro Arduino, díl druhý
date: 2016-04-07T11:21:30+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/vyukovy-shield-pro-arduino-dil-druhy/
permalink: /vyukovy-shield-pro-arduino-dil-druhy/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/09/20160328_204219.jpg
categories:
  - Arduino
  - Arduino
  - Hardware
tags:
  - arduino
  - pcb
  - shield
---
Po minulém [dilematu s deskami plošných spojů](http://retrocip.uelectronics.info/vyukovy-shield-pro-arduino-dil-prvni/) jsme opět pokročili. Výroba PCB už běží, nakonec přes [SeeedStudio](http://www.seeedstudio.com/service/index.php?r=pcb)&#8230;

Klasika.

Dokončuju plošňák, a poslední akce je &#8222;zalití země&#8220; &#8211; tedy použít zbývající měď na desce jako zem. Zalil jsem, znelíbila se mi, odlil jsem. Upravil jsem, zase jsem zalil, zase se mi nelíbilo&#8230; a tak dál. Asi šestkrát. Nakonec jsem to celé vyexportoval, poslal do formuláře SeeedStudia, viděl jsem cenu 25 USD i s poštovným za 10 desek, kliknul jsem na Objednat, zaplatil kartou&#8230;

&#8230; a neuvěříte, co se stalo! Tedy uvěříte, pokud jste staří praktici. Samozřejmě že se kochám svým výtvorem, prohlížím si ty masky, které jsem předtím kontroloval, a najednou&#8230; najednou TO vidím! Ta zem! Ta blbá rozlitá zem! Není spojená se zemí u IO! Já to, pokud znáte Eagle, tak víte, zapomněl přejmenovat před rozlitím na GND! Ale co, je to prototyp, a jak pravil Štěpán &#8211; nic, co by nespravil kus drátu!

Ale když tu o tom točím, tak bych rád tak jako vypíchnul dvě věci. Zaprvé: U SeeedStudia mě vyjde 10 desek i s poštovným na necelých šest stovek, tedy tolik, kolik si Pragoboard chtěl nechat zaplatit za jednu desku. Ale to mi vadí nejmíň&#8230;

Víc mi vadí jiná věc. Ani Pragoboard, ani další oslovení nedokážou říct na rovinu: Plošňák X krát Y milimetrů, oboustraně s prokovy a maskou, bude stát tolik, plus mínus 10 procent podle náročnosti úprav. Ne, neumí. Ani do telefonu vám to neřeknou. &#8222;Pošlete mailem poptávku&#8230;&#8220;

Víte, já nechci posílat mailem poptávku, protože nežiju v roce 1999. Já chci, jako se to dělá všude jinde, tím SeeedStudiem počínaje a různými OSHParky konče, nahrát podklady k desce do formuláře, a za půl minuty vidět vyrenderované náhledy ke kontrole a k tomu cenu zakázky. Hned. A zaplatit to kartou. Rozumíme si? Tady neexistuje důvod, proč by &#8222;to nešlo&#8220;. Je to stejný opruz jako ty další (nejen) české služby, co tají cenu: &#8222;Pošlete si poptávku, rádi vám ji sdělíme!&#8220; Bože, a ty argumenty, proč to takhle dělají? Jako z panoptika: &#8222;Konkurence by viděla ceník na webu!&#8220; nebo &#8222;Ceny jsou individuální&#8230;&#8220; Fajn, OK, nakoupím jinde, protože jestli něco nesnáším, tak je to smlouvání. Chci se rozhodnout na základě relevantních informací, a když mi je neposkytnete tam, kde je chci, jdu jinam, víme?

Takže čekám na desky, a budu čekat dvakrát delší dobu, než kdybych si to objednal v ČR, ale bude to levnější, kvalita stejná, a s prominutím &#8211; při procesu objednání jsem neměl pocit, že se doprošuju, aby mi laskavě něco řekli, ale měl jsem pocit, že nakupuju službu.