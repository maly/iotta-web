---
id: 151
title: 'Moudře hlavou pokýval&#8230;'
date: 2016-12-01T18:17:59+00:00
author: Martin Maly
layout: revision
guid: http://iotta.uelectronics.info/148-autosave-v1/
permalink: /148-autosave-v1/
---
Navážu na minulý zápisek o [Digisparku](http://iotta.uelectronics.info/digispark/). Psal jsem, že ukážu, k čemu ho použít. Ale než se dostanu k tomu, co zamýšlím, totiž propojení s BigClownem, tak dovolte jednu takovou hračku.

Před časem mě zaujal návod na Instructables, kde autor ukazoval, [jak si postavit Head Mouse](http://www.instructables.com/id/Head-Mouse-With-MPU6050-and-Arduino-Micro/?ALLSTEPS) &#8211; zjednodušeně řečeno zařízení, kde nakláněním hlavy ovládáte kurzor myši. Ono to takhle zní jako děsně velká geekovina, ale udělaný je to jednoduše. Autor použil normální MPU-6050 a k tomu nějaké Arduino, které má procesor ATmega32u2. V tom &#8222;U2&#8220; je celé kouzlo &#8211; tyhle procesory totiž mají řadič pro USB Device, a můžete je naprogramovat tak, že se chovají kupříkladu jako USB HID. HID, pro ty méně znalé, je Human Interface Device, tedy méně vznešeně: klávesnice, myš, joystick&#8230;

Střih. Digispark má USB připojený přímo k ATtiny. To znamená, že můžete využít třeba V-USB a podobné knihovny, které vytvoří jednoduché USB zařízení ryze softwarově. HID jsou poměrně jednoduchá, a tak se do osmi kil, co Digispark má, pohodlně vejdou. Můžete se podívat na demo: [Digispark Keyboard](https://github.com/digistump/DigistumpArduino/tree/master/digistump-avr/libraries/DigisparkKeyboard), [Digispark Mouse](https://github.com/digistump/DigistumpArduino/tree/master/digistump-avr/libraries/DigisparkMouse), [DigisparkJoystick](https://github.com/digistump/DigistumpArduino/tree/master/digistump-avr/libraries/DigisparkJoystick). Hned jsem s tím experimentoval, a ono to fungovalo! Takže jsem nejdřív dělal různé blbůstky, jako USB klávesnici, co píše vzkazy, nebo náhodný pohyb myší, a pak jsem zkusil, jestli by to nešlo dohromady s tím MPU-6050. A co byste řekli? Šlo to!

Musel jsem udělat drobnou úpravu &#8211; do souboru I2Cdev.h dopsat na začátek

<pre class="">#define BUFFER_LENGTH 1</pre>

a změnit Mouse na DigiMouse. Důležité bylo nahradit &#8222;delay&#8220; za &#8222;DigiMouse.delay()&#8220; &#8211; bez toho softwarová obsluha USB nepoběží.

Celé dohromady to fungovalo na první zapojení. Nejdřív jsem se lekl, kam mi ustřelil kurzor, a pak jsem ho mírným nakláněním desky s akcelerometrem šoupal po obrazovce&#8230; No nádhera. Ovládat bych tím nic nechtěl, ale jako hračka je to dostatečně krásné.