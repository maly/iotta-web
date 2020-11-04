---
id: 115
title: Víkendový výběr
date: 2016-10-16T11:21:00+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/?p=115
permalink: /vikendovy-vyber/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/10/19681470919_9a9bcd5692_z.jpg
categories:
  - DIY
  - Hardware
tags:
  - CHIP
  - ESP32
  - iot
  - Raspberry
  - Tipy
---
Když máme tu neděli, tak se pojďme podívat na pár tipů, co tu krouží internetem. Devkity, zajímavá zapojení a další věci pro radost.

Před časem mi dorazil PocketCHIP. Jestli nevíte, oč jde, tak: [CHIP](https://getchip.com/pages/chip) je jednodeskový mikropočítač a la Raspberry. Stojí devět dolarů a uvnitř je procesor R8 (ARM Cortex),  dále WiFi, 4GB FLASH, 512MB RAM, Bluetooth a videovýstup, snadno upravitelný na VGA/HDMI. No a PocketCHIP je vlastně CHIP s klávesnicí a dotykovým displejem:



Pokud máte tuhle hračku taky, tak &#8211; už jste zkusili retročipovou hračku &#8211; emulátor C64? Já ano, a [bylo to docela jednoduché](http://www.rift.dk/blog/run-vice-on-your-pocketchip).

Výrobce už přichází s novou verzí [CHIP Pro](https://getchip.com/pages/chippro) za cenu 16 USD. K dispozici na konci roku, devkit už teď. Hlavní tahák by měl být v tom, že procesor s označením GR8 je k dispozici i pro další konstruktéry bez NDA.

* * *

Pokud vám vadí, že vám posílají maily jen samí spammeři, co takhle donutit vaše ESP8266, ať vám taky něco pošlou? Není to tak složité, [návod je zde](http://www.instructables.com/id/ESP8266-GMail-Sender/).

* * *

Máte-li už ESP32 a poohlížíte se po nějaké zajímavé aplikaci, ne nutně internetové, tak můžete zkusit [emulátor Nintenda](https://github.com/espressif/esp32-nesemu).

* * *

Měření spotřebované energie je taky oblíbené téma. Dělá to třeba český Energomonitor. Pokud nepotřebujete to, co Energomonitor nabízí navíc (tedy cloudovou aplikaci) a stačí vám jen to měření podle blikátka na elektroměru, [můžete si ho postavit sami](https://hackaday.io/project/6938-internet-of-things-power-meter). I s krabičkou, vytištěnou na 3D tiskárně, to vypadá docela dobře.

* * *

Pokud přemýšlíte nad tím, že byste nejradši někoho vystřelili do vesmíru, máte šanci &#8211; tedy pokud ten &#8222;někdo&#8220; bude Raspberry Pi. [European AstroPi Challenge](https://www.raspberrypi.org/blog/announcing-european-astro-pi-challenge/) nabízí šanci studentům, aby si zkusili naprogramovat aplikaci pro Raspberry se SenseHAT a kamerou, co krouží na palubě ISS.

* * *

Sedíte doma, programujete Arduino a říkáte si: To by bylo fajn, kdybych nemusel mít ten USB kabel&#8230; No, máte šanci. Zkuste podpořit [projekt WiLoader](https://www.kickstarter.com/projects/petuniatech/wiloader-the-wifi-programmer-for-avr), který umožňuje programovat Arduino přes WiFi.

* * *

A když už jsem zmínil ten Kickstarter, tak co třeba tenhle projekt? Jmenuje se [Familink](https://www.kickstarter.com/projects/familink/picture-sharing-with-all-generations-made-easy) a technicky jde o fotorámeček, kam nahráváte fotky. Hezké na tom je, že fotorámeček je třeba u vašich rodičů či prarodičů, co nelezou po internetech a posílat jim fotky z dovolené nebo fotky vnoučat mailem nemá moc smysl. Takhle pošlete fotku do &#8222;cloudu&#8220;, a ten ji přenese po 3G síti přímo k vašim (pra)rodičům do obýváku. Hezké, ne?