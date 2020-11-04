---
id: 37
title: Liška kontra papoušek
date: 2016-04-20T11:21:22+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/liska-kontra-papousek/
permalink: /liska-kontra-papousek/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/09/IMG_4201-2.jpg
categories:
  - Hardware
  - LoRa
  - Sigfox
tags:
  - lora
  - lorawan
  - sigfox
---
Tak se to nějak sešlo, že mám teď doma tři kousky na testování. Dvakrát Sigfox, jednou LoRa od Things.cz. (Fox a Lóra, jasný? Slovní hříčka&#8230;)

[Desky na shieldy](http://retrocip.uelectronics.info/vyukovy-shield-pro-arduino-dil-druhy/) přijdou zítra nebo pozítří, takže je zatím čas na hraní. Zde jsou moje první dojmy z vybraných kitů.

<img loading="lazy" class="aligncenter size-medium wp-image-792" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/04/IMG_4201-2-650x526.jpg" alt="" width="650" height="526" /> 

Jako první dorazil Arduino shield pro Sigfox. Je na něm modul od ATIM, připojený tím nejjednodušším možným způsobem přes adaptér úrovní 3.3V &#8211; 5V.

Abych to konkretizoval: Nejjednodušší možný způsob zde znamená, že modul, co komunikuje přes sériový port, připojili na sériový port Arduina. Já nevím, jestli víte, co to znamená&#8230; na tomtéž sériovém portu totiž probíhá programování Arduina. Data, co tečou při programování do Arduina, se volně míchají s daty, co tečou z toho modulu po resetu (a že jich tam teče dost), a když Arduino něco odpoví, tak to modul pošle do sítě. A nakonec to, logicky, skončí s chybou. Takže programování, přátelé, programování znamená, že musíte odpojit Arduino, sundat shield, připojit Arduino, naprogramovat, odpojit Arduino, připojit shield, připojit Arduino.

Navíc ten modul má takovou zvláštnost. On totiž sice umí takové ty standardní AT příkazy, co má Sigfox, ale nejdřív mu musíte nějakou escape sekvencí znaků (podle manuálu je to &#8222;+++&#8220;, ale taky možná &#8222;&&&&#8220;) říct, že budete posílat AT příkazy. Pokud to neuděláte, tak modul bere to, co mu pošlete, a po shlucích dvanácti bajtů to posílá do sítě. Damned.

A než jsem na to metodou pokus &#8211; omyl přišel, tak byl shield ošoupaný jako levné sako. Manuál je roztomilý, protože půlka je ve francouzštině, půlka v angličtině, naštěstí ta, co se týká Sigfoxu, je anglicky. Le jupí!

Přitom ten modul má, jestli jsem to dobře pochopil, něco jako enable vstup. Kdyby na té destičce bylo prosté, himbajs, tlačítko, které by odpojilo modul, takže by ho při programování stačilo přidržet, bylo by to rázem řádově lepší! No, tlačítko není, a v SimpleCell by se asi zlobili, kdybych si ho na zapůjčený shield přimontoval sám. 🙂

Jinak jako jo, práce s tím je extrémně jednoduchá (když si tímhle projdete), a pokud si sestavíte zařízení, co bude někde fungovat, tak to je naprostá pohoda a vystačíte si s naprosto obyčejným Serial.print, ani ty AT příkazy neřešíte. Ale NEŽ to zařízení sestavíte a odladíte, tak to bude asi bolest.

Od bolesti dál. Druhý testovací bazmeg dorazil od Things.cz, tedy od společnosti, co chce provozovat nekomerční síť na standardu LoRa, resp. LoRaWAN. A protože okolí mého bydliště pokryté není, pokryl jsem si ho vlastními silami, respektive zapůjčeným přístupovým bodem. Zatím nemohu potvrdit dosah, protože testuju z ložnice do obýváku, a to doslova. Testuju to na dodaném LoRaWAN modulu, který je zatím, jestli to správně chápu, neveřejný, takže ho ani fotit nebudu&#8230;

Po kratších peripetiích, způsobených nastavením přístupového bodu a tím, že klíč je nutno poslat UPPERCASE, což jsem netušil, začalo zařízení fungovat takříkajíc na první dobrou. Vyzkoušel jsem uplink, tedy posílání zpráv, i downlink, tedy příjem odpovědí, a nezaznamenal jsem žádný problém, s jedinou výjimkou, a to bylo potvrzování downlinku, které se občas neodeslalo. Podle výrobce je příčinou použitý chipset v tom kusu, co mám k dispozici, s novým by to mělo fungovat bez problémů.

Třetí deska, co dorazila, je takový bastlířský ekvivalent šlehačkového dortu s karamelovou polevou, zmrzlinou a želatinovými bonbóny. Jmenuje se [SmartEverything](http://www.smarteverything.it/) a je to taková, pardon za to slovo, &#8222;beruška&#8220;. Ani jsem se nedíval na cenu, protože&#8230; Helejte:

<img loading="lazy" class="aligncenter size-full wp-image-793" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/04/SmartEveryThing1.png" alt="" width="550" height="233" /> 

Je to deska, velká jako Arduino. Je tam Atmel SAMD, tedy 32bitový Cortex. Je tam GPS. Je tam Sigfox. A taky teplotní čidlo, vlhkostní čidlo, akcelerometr, gyroskop, kompas, tlakoměr, NFC, proximity senzor, RGB ledka a vodotrysk. Ne, kecám, jedno z toho tam není&#8230;

Připojil jsem, dle návodu nainstaloval prostředí pro desky, spustil blink.ino, přeložil, a v půlce získal zvláštní chybu.

Totiž ono je takové pravidlo. Jakmile se pokouším instalovat nebo zprovoznit něco, o čem je někde v nějakém diskusním fóru napsáno &#8222;Zkouším to a objevila se mi tam taková chyba&#8220; a na to třicet odpovědí: &#8222;To se nestává, to slyším prvně&#8220;, tak mně se ta chyba projeví. Takže tady taky. Nakonec pomohlo aktualizovat Arduino IDE, a už to šlapalo jak hodinky.

Ke všem těm komponentám jsou i knihovny a demíčka, takže jsem vesele posílal data přes Sigfox, měřil teplotu, tlak, vlhkost, blikal LEDkou, zjišťoval pozici (což šlo doma docela blbě) &#8211; ta výbava zkrátka slibuje velkou radost při hraní. Dokonce jsem někde koutkem oka zahlédl, že to lze napájet i z baterie, to by bylo fakt pěkné&#8230;

(Jo, a ta [cena je nějakých pětasedmdesát liber&#8230;](http://uk.rs-online.com/web/p/radio-frequency-development-kits/9015121/))

(Doplnění, díky Pavlovi Sodomkovi: Na českém rs-online se to skrývá pod kryptickým označením [Vývojová sada MCS7561](http://cz.rs-online.com/web/p/vyvojove-sady-pro-rf/9015121/), cena necelých 2900 Kč)

Víc jsem zatím po večerech nestihnul, ale zůstaňte vytuněni, jak se říká, jen co bude Troška času, budu experimentovat dál.