---
id: 26
title: Komprese GPS
date: 2016-07-19T11:20:38+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/komprese-gps/
permalink: /komprese-gps/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/09/screenshot-mapy.cz-2016-07-19-12-56-47.png
categories:
  - Software
tags:
  - gps
---
U [minulého článku](http://iotta.cz/hratky-se-sigfoxem/) jsem zmínil, že jsem přemýšlel nad tím, jak přenést souřadnice pomocí krátké zprávy Sigfox (12 bajtů). Jaké jsou možnosti?

GPS obecně udává dvě souřadnice (výšku tentokrát zanedbáme), a to zeměpisnou šířku (latitude) a délku (longitude). Šířka se udává ve stupních 0 až 90, kde 0 je rovník, 90 je pól. Další informace je, zda se jedná o severní nebo jižní polokouli. U délky se hodnoty udávají ve stupních v rozsahu 0 až 180, kde 0 je Greenwich, na východ od něj je východní polokoule, na západ západní. Kompletní adresa tedy vypadá třeba takto: 15E 50N.

Jenže celý stupeň je hodně velká oblast. Pro představu &#8211; tohle je prostor mezi 15 a 16 stupňem východní délky a 49 a 50 stupněm severní šířky:

<img loading="lazy" class="aligncenter size-full wp-image-839" src="http://iotta.cz/wp-content/uploads/sites/6/2016/07/screenshot-mapy.cz-2016-07-19-12-56-47.png" alt="screenshot-mapy.cz 2016-07-19 12-56-47" width="393" height="588" /> 

Tedy flák země. Proto se stupně dále dělí na minuty, vteřiny, a ty se přenášejí v desetinné podobě. Jenže minuty a vteřiny používají šedesátkové dělení, což je u počítačů nepraktické, proto se často používá desetinné dělení stupňů. Například [49.2244914N, 13.2719244E](https://mapy.cz/zakladni?x=13.2719244&y=49.2240848&z=16&source=area&id=14410).

Z vlastností reálných čísel vyplývá, že přidáváním dalších míst za desetinnou tečku zpřesňujeme polohu.Pokud snížíme počet číslic, sníží se přesnost souřadnice, respektive zvětší se označená plocha na zemském povrchu. Pozice je tedy &#8222;ztrátově kompresována&#8220; už tím, že jde o reálná čísla.

Pokud bych přenášel čísla v desítkové ASCII podobě, vyšlo by mi šest znaků na každou souřadnici. &#8222;49.22413.271&#8220; &#8211; tedy přesnost na tisíciny stupně, je nedostatečná. Pro představu:

<img loading="lazy" class="aligncenter size-full wp-image-840" src="http://iotta.cz/wp-content/uploads/sites/6/2016/07/screenshot-mapy.cz-2016-07-19-13-22-25.png" alt="screenshot-mapy.cz 2016-07-19 13-22-25" width="222" height="258" /> 

Ušetřím jedno místo, když vypustím desetinnou tečku. Ale v takovém případě už musím vycházet z toho, že část před ní bude mít fixní počet číslic. Pokud chci třeba operovat na území České republiky, nemám problém. Ale pro obecnou kompresi GPS se to příliš nehodí: délka může mít rozsah 0 až 180, tedy 1 až 3 číslice, navíc potřebuju zakódovat kvadranty severní / jižní, západní / východní.

Na dvojnásobek mohu přesnost zvednout, pokud použiju BCD. Do jednoho bajtu se pak vejdou dvě číslice, a díky tomu, že mám nevyužité kódy A-F, mohu jimi zakódovat desetinnou tečku i kvadrant. Dostanu se tak na velmi vysoké rozlišení.

Jenže jaksi sami cítíte, že taková komprese není příliš efektivní. Je tam spousta &#8222;volného prostoru&#8220;.

Další možnost je pracovat s reálnými čísly ve tvaru &#8222;floating point&#8220;, nebo &#8222;fixed point&#8220;, osekanými na určitý počet bitů v mantise. Do toho se mi ale moc nechtělo.

Druhý nápad, se kterým jsem pracoval, vyšel z toho, že si určím jižní a západní polokouli jako zápornou (-180..+180, resp. -90..+90) a posunu soustavu souřadnou tak, aby byla v rozmezí <[0,0] &#8211; [360,180]>. Tento rozsah pak rozdělím na menší obdélníky. Jejich počet bude určovat jednak přesnost, ale i požadovaný počet bitů informace. Jako dostačující přesnost mi připadá pěti- či šestibajtová (20 či 24 bitů per souřadnice). Pokud rozdělíme plochu na šestnáctimiliontiny (1/16777216, 24 bitů), dostáváme obdélník o délce 0.0000214 stupně a šířce 0.0000107 stupně. To je vzdálenost několika kroků:

<img loading="lazy" class="aligncenter size-full wp-image-841" src="http://iotta.cz/wp-content/uploads/sites/6/2016/07/screenshot-mapy.cz-2016-07-19-13-38-09.png" alt="screenshot-mapy.cz 2016-07-19 13-38-09" width="420" height="546" /> 

V případě nutnosti přidám sedmý bajt a přesnost bude v každém směru 16x větší.

Když tu ukazuju hrubé odhady toho, kolik je jedna desetitisícina stupně, tak platí zhruba pro naše území. Z vlastností zeměpisných souřadnic vyplývá, že vzdálenost jednoho stupně délky na rovníku je mnohem vyšší než u nás, a i tady je mnohem vyšší než třeba na severu Švédska, a v bezprostřední blízkosti pólu klesá až k nule. Toho by se dalo využít, a v případě, že neděláte aplikaci pro polárníky, tak by jistě šlo zeměpisnou délku pro šířky blízké pólům &#8222;decimovat&#8220; &#8211; snížit jim přesnost v bitech, a tak ušetřit, při relativně malé degradaci přesnosti.

No a konečně poslední možnost, se kterou jsem pracoval, je umělé snížení rozsahu GPS souřadnic, pokud je jasné, že přenášená hodnota z těchto souřadnic nevystoupí. Pro Českou republiku můžeme zvolit hrubý &#8222;obdélník opsaný&#8220;, omezený mezi 12. a 19. stupeň východní délky a 49. až 51. stupeň severní šířky. Tady je problém, protože malá část našeho území spadá až nad 51. stupeň:

<img loading="lazy" class="aligncenter size-full wp-image-843" src="http://iotta.cz/wp-content/uploads/sites/6/2016/07/screenshot-mapy.cz-2016-07-19-13-46-40.png" alt="screenshot-mapy.cz 2016-07-19 13-46-40" width="288" height="129" /> 

Pokud chcete vědět úplně přesně, tak vězte, že obdélník opsaný má souřadnice [48.5519972N, 12.0906633E] &#8211; [51.0556997N, 18.8591456E]. Pokud republiku pokryjete sítí 65536 x 65536 obdélníků, tedy 16 x 16 bitů, získáte dostatečnou přesnost pro velké množství aplikací ve čtyřech bajtech. První souřadnici by šlo degradovat, protože republika je orientována směrem východ &#8211; západ, a dal by se tak ušetřit bit či dva.

Na podobných principech pracují i další metody &#8222;komprese zeměpisných souřadnic&#8220;. Naleznete jistě i sofistikovanější metody. Já přemýšlel pouze nad těmi, které bych dokázal, doufejme, bez větších obtíží implementovat v Arduinu.