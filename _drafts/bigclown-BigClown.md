---
id: 34
title: BigClown
date: 2016-06-01T11:21:16+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/bigclown/
permalink: /?p=34
image: http://iotta.cz/wp-content/uploads/sites/17/2016/09/bigclown.jpg
categories:
  - Hardware
tags:
  - arm
  - bezpecnost
  - cortex-m0
  - iot
  - kryptografie
  - stm32
---
Před pár lety, kdy jsem si redefinoval svoje profesní cíle a fantazie, tak jsem si říkal: Jaká by byla úplně ideální práce, kterou bych chtěl dělat? A uvědomil jsem si, že by mě ze všeho nejvíc bavilo:

  * Hrát si s elektronikou. Jako že mě zaujme nějaké zajímavé čidlo, tak si ho koupím, zkusím s ním něco dělat, nějaké zapojení navrhnout, vyzkoušet to, pohrát si. A pak sednout a&#8230;
  * Psát o tom, jak se s čipem pracuje, na jaké problémy jsem narazil, co se mi líbilo a co ne&#8230; Dát dohromady tipy, odpovídat lidem, které to zaujalo, pomoci s použitím a nasazením&#8230;
  * Programovat pro to &#8211; drobné utility, uživatelské rozhraní a tak. Nic velkého, žádný bankovní systém v Javě, ale spíš jen malá aplikace, drobnost, _proof of concept_&#8230;
  * Školit to, učit, psát o elektronice&#8230;

Celé to dohromady se jaksi přirozeně točí okolo Internetu věcí, ať už to je co chce.

No a tak jsem decentně směřoval k tomu cíli. Kombinoval jsem něco jako zaměstnání, něco jako koníček, však víte sami, pokud mě sledujete.

Logicky se na mne časem začali obracet různí lidé, abych jim buď poradil, nebo řekl svůj názor na jejich &#8222;IoT technologii&#8220;. Tak jsem se dostal k Sigfoxu, k LoRaWan od Things.cz a ke spoustě dalších věcí. Jedním z týmů, který mě oslovil a žádal o zpětnou vazbu, byla parta z Jablotronu, která přišla s platformou pro domácí automatizaci, nazvanou BigClown. Na první letmý pohled jedním okem to vypadalo jako ekosystém &#8222;a la Arduino + vybrané senzory a aktuátory + koncentrátor&#8220;, ale jedna věc mě zaujala enormně, a to&#8230; Zkusíte hádat?

Když Jablotron, tak bezpečnost!

Ty jednotlivé endpointy, tedy samotné &#8222;věci&#8220;, jsou modulární. Hlavní modul (&#8222;core&#8220;) je postavený na procesoru [STM32L083CZ](http://www.st.com/content/st_com/en/products/microcontrollers/stm32-32-bit-arm-cortex-mcus/stm32l0-series/stm32l0x3/stm32l083cz.html), což je Cortex M0+, s low power, s generátorem náhodných čísel a akcelerací AES128. U něj je čip ATSHA204, tedy kryptografický čip, který umožňuje bezpečné ukládání klíčů a další kryptografické lahůdky (do podrobností se _hodlat nehodlám, nemám nyní na to čas_&#8230;)

Takže máme &#8222;věci&#8220; s integrovaným kryptografickým hardware, kde šifrování není možnost, ale vlastnost. Komunikace s koncentrátorem (hubem) probíhá po rádiu (v pásmu 868MHz) a je šifrovaná. Zprávy jsou technicky MQTT s JSON payloadem, serializované, komprimované a šifrované.

<img loading="lazy" class="aligncenter size-medium wp-image-813" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/06/bigclown-650x583.jpg" alt="bigclown" width="650" height="583" /> 

V roli koncentrátoru může být jakýkoli počítač, klidně i Raspberry Pi nebo Turris Omnia (router od CZ.NIC), který se stará o další věci &#8211; příjem zpráv, odesílání, jejich zpracování, komunikaci do internetu (ostatně ani být nemusí; můžete si to připojit třeba k televizi a sledovat či řídit) a další věci. Důležitá vlastnost je tu automatická aktualizace a záplatování. Koncentrátor hraje rovněž zásadní roli při distribuci klíčů a párování se zařízením &#8211; to probíhá fyzicky přes NFC.

Idea &#8222;modulární stavebnice domácí automatizace, kde je bezpečnost nikoli přidaná hodnota, ale transparentní vlastnost&#8220; je něco, co [Michal Altair Valášek](http://www.altairis.cz/) ohodnotil slovy: &#8222;Tohle jsem u IoT hledal, takovouhle stavebnici, kde je kryptografie vyřešená, ale nikde nenašel!&#8220;

A celé tohle je open source hardware a open source software, uvolněné pod licencemi MIT, CC apod. (ne, žádné GPL, naštěstí!)

No a když jsme se bavili o mém názoru na věc, tak jsem říkal, že se mi to moc líbí, že rád pomůžu a že se to blíží mé &#8222;práci snů&#8220;.

A tak jsme si plácli.

Jestli vás to zaujalo, tak si přihlašte newsletter na firemní stránce [BigClown Labs](https://www.bigclown.com/cz/), nebo pište mně.