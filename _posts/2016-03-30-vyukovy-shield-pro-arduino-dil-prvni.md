---
id: 42
title: Výukový shield pro Arduino, díl první
date: 2016-03-30T11:21:35+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/vyukovy-shield-pro-arduino-dil-prvni/
permalink: /vyukovy-shield-pro-arduino-dil-prvni/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/03/20160328_204219-650x365.jpg
categories:
  - Arduino
  - Arduino
  - Hardware
tags:
  - arduino
  - atmel
  - attiny2313
  - pcb
---
V [kurzu Arduino 101](http://arduino101.cz) učíme lidi pracovat s Arduinem. Používáme k tomu takzvaný &#8222;Clock Shield&#8220;, což je shield, pomocí kterého se dělají různé &#8222;hodinové&#8220; aplikace. Má docela zajímavou sestavu senzorů a periferií, ale trošku nešikovně zapojenou. Takže jsme se rozhodli udělat to znovu, a lépe!

<!--more-->

Originální shield má třeba tři tlačítka. Skvělá věc, bohužel ani jedno z nich není zapojené na pinu, který umí vyvolat přerušení. To je škoda&#8230; Jsou i další věci, co nám na shieldu nevyhovují, například že zabírá piny, které používá wifi shield. Takže padlo rozhodnutí: Znovu, lépe, radostněji! [sc:ebay item=&#8220;Clock Shield Arduino TM1636&#8243;] 

Seznam periferií zůstal plusmínus stejný: Čtyřmístný displej (sedmisegmentovky), tři tlačítka, LEDky, bzučák, fotorezistor, termistor, hodiny reálného času DS1307. K tomu jsme přidali ještě RGB ledku.

V originálním shieldu budí displej obskurní čínský čip TM1636. Nejen že k němu není pořádný datasheet, ale ani ten obvod není k sehnání v nějakém rozumném množství. Navrhl jsem tedy zapojení s obvodem MAX7219, ale kolegové z [CZ NIC](http://nic.cz), kteří s námi na shieldu spolupracují, namítali, že jde o poměrně drahý obvod, a jestli bychom tedy neměli levnější řešení&#8230;

Zamysleli jsme se a z mého frajerského &#8222;tak tam sáme třeba nějaký jednočip, co, hehe?&#8220; se nakonec stala vážná idea, a když jsme zjistili, že místo jednoho MAX7219 seženeme za stejnou cenu čtyři ATtiny2313, bylo rozhodnuto. Objednali jsme součástky z Číny a za několik týdnů se komponenty začaly scházet&#8230;

Největší obavy jsem měl právě z toho neortodoxního řešení displeje. Nakonec se to ukázalo jako záležitost jednoho večera. ATtiny jsem naprogramoval přes Arduino jako Arduino, tedy jestli mi rozumíte&#8230; V Arduinu Uno mám sketch Arduino ISP, který z Arduina udělá atmelský programátor. No a k němu je připojená ta ATTiny. A já se k ní chovám jako k Arduinu&#8230; Takže i ten ovladač displeje je napsaný jako sketch. Což je hezké&#8230;

<a href="http://iotta.cz/wp-content/uploads/sites/17/2016/03/20160328_204219-650x365.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-166" src="http://iotta.cz/wp-content/uploads/sites/17/2016/03/20160328_204219-650x365-300x168.jpg" alt="" width="300" height="168" srcset="https://iotta.cz/wp-content/uploads/sites/17/2016/03/20160328_204219-650x365-300x168.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2016/03/20160328_204219-650x365.jpg 650w" sizes="(max-width: 300px) 100vw, 300px" /></a>

&nbsp;

A když už to celé mám hezky naprototypované, tak nastal čas na prototyp plošného spoje. Aby to bylo rychle, protože čas tlačí (ten vždycky tlačí!), tak jsem si říkal, že poptám nějakého českého výrobce desek, který umí dvoustranné desky s prokovy v malém množství. U Pragoboardu vyšla cena tak neskutečně absurdní (několik set Kč za jednu desku 5&#215;6 centimetrů), že holt zase dám vydělat do Asie. Sice to bude další tři týdny zpoždění, ale přátelé&#8230; tři tisíce za pět destiček nedám!

_Pokračování příště&#8230;_