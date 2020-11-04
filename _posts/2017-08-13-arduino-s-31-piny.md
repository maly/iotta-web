---
id: 354
title: Arduino s 31 piny
date: 2017-08-13T12:16:32+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=354
permalink: /arduino-s-31-piny/
image: http://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_130500_HDR.jpg
categories:
  - Arduino
tags:
  - arduino
  - ATmega
  - AVR
  - Bootloader
  - ISP
---
Na jednu konstrukci se mi hodilo Arduino, ale potřeboval jsem mít víc pinů, než standardních 20 (14 + 6). Co s tím? Co třeba udělat si vlastní Arduino z &#8222;velkého&#8220; jednočipu ATmega? Třeba ATmega32 &#8211; v podstatě totéž, co má Arduino Uno, ale v pouzdru se 40 piny. Nneí to tak složité, stačí breadboard, Uno a pár drátů&#8230;

<!--more-->

Arduino je v podstatě &#8222;holý&#8220; jednočip ATMega328. To, co z něj dělá Arduino, je bootloader a mechanismus nahrávání programů.

**Bootloader** je krátký kód, který se provede hned po startu jednočipu. Chvíli čeká, jestli se na sériové lince neobjeví data z programátoru. Pokud ne, spustí nahraný program, pokud ano, předpokládá, že data jsou kód k nahrání, a nahraje je do paměti.

Použít jiný AVR jednočip, třeba ATmega32, není velký problém, jen musíte zajistit pár věcí:

Zaprvé &#8211; musíte do něj nahrát ten zmíněný bootloader. Jednou stačí, pak už to bude fungovat.

Zadruhé &#8211; musíte mít vhodný USB převodník. Modulů, které z USB dělají TTL signály TxD a RxD je spousta, vy ale potřebujete takový, který má i signál DTR. Musíte chvilku hledat&#8230;

Zatřetí &#8211; musíte mít pro Arduino IDE vhodnou knihovnu pro práci s těmito procesory.

Pojďme na to krok po kroku&#8230;

## Programátor

Pokud máte programátor, tedy nástroj, jak nahrávat programy přímo do procesoru, máte vyhráno. Ale ne každý ho má. Naštěstí můžete využít obyčejné Arduino Uno a nahrát do něj sketch, který se jmenuje &#8222;Arduino ISP&#8220;. Najdete ho v menu Soubor, podmenu Příklady, je tam hned jako příklad 11.

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a1.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-357" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a1.jpg" alt="" width="562" height="469" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/a1.jpg 562w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/a1-300x250.jpg 300w" sizes="(max-width: 562px) 100vw, 562px" /></a>

Tento sketch jenom přeložíte a nahrajete do obyčejného Arduina, jak jste zvyklí. Arduino se tím přeměnilo v &#8222;Arduino ISP&#8220;, tedy v programátor jednočipů AVR.

## Knihovny pro AVR

Teď si připravte vhodné knihovny, které umí pracovat s &#8222;holými&#8220; AVR. Já našel pěknou knihovnu [MightyCore](https://github.com/MCUdude/MightyCore#how-to-install). Její instalace je jednoduchá:

  1. Do URL Správce desek (Soubor &#8211; Vlastnosti) přidejte adresu _https://mcudude.github.io/MightyCore/package\_MCUdude\_MightyCore_index.json<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a2.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-358" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a2.jpg" alt="" width="690" height="342" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/a2.jpg 690w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/a2-300x149.jpg 300w" sizes="(max-width: 690px) 100vw, 690px" /></a>_
  2. Otevřte Manažér desek (Nástroje &#8211; Deska &#8211; Manažér desek):  
    <a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a3.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-359" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a3.jpg" alt="" width="793" height="257" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/a3.jpg 793w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/a3-300x97.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/a3-768x249.jpg 768w" sizes="(max-width: 793px) 100vw, 793px" /></a>
  3. Vyberte MightyCore a dejte Instalovat<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a4.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-360" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a4.jpg" alt="" width="794" height="455" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/a4.jpg 794w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/a4-300x172.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/a4-768x440.jpg 768w" sizes="(max-width: 794px) 100vw, 794px" /></a>

## Nahrání bootloaderu

Teď si připravte ATmega32 (klidně 64, nebo 16, jak chcete) do breadboardu a propojte jej takto:

  * Piny 11 a 31 ATmega na pin GND z Arduina
  * Piny 10 a 30 ATmega na pin +5 V Arduina
  * Pin 6 ATmega na pin 11 Arduina (MOSI)
  * Pin 7 ATmega na pin 12 Arduina (MISO)
  * Pin 8 ATmega na pin 13 Arduina (SCK)
  * Pin 9 ATmega na pin 10 Arduina (RESET)

Pro snazší pochopení nákres + foto:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-boot_bb.png" rel="lightbox"><img loading="lazy" class="aligncenter size-large wp-image-361" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-boot_bb-710x1024.png" alt="" width="640" height="923" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-boot_bb-710x1024.png 710w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-boot_bb-208x300.png 208w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-boot_bb-768x1107.png 768w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-boot_bb.png 945w" sizes="(max-width: 640px) 100vw, 640px" /></a>

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_123922_HDR.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-355" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_123922_HDR.jpg" alt="" width="800" height="450" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_123922_HDR.jpg 800w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_123922_HDR-300x169.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_123922_HDR-768x432.jpg 768w" sizes="(max-width: 800px) 100vw, 800px" /></a>

Jakmile máte propojeno, udělejte následující v Arduino IDE:

  1. V menu Nástroje &#8211; Deska zvolte MightyCore ATmega32:<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a5.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-362" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a5.jpg" alt="" width="801" height="698" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/a5.jpg 801w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/a5-300x261.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/a5-768x669.jpg 768w" sizes="(max-width: 801px) 100vw, 801px" /></a>
  2. Nástroje &#8211; Pinout: Standard
  3. Nástroje &#8211; Clock: 8 MHz internal
  4. Nástroje &#8211; Compiler LTO: Disabled
  5. Nástroje &#8211; BOD: Disabled
  6. Nástroje &#8211; Programátor: zvolte **Arduino as ISP (MightyCore) **(pozor, nesplést s _ArduinoISP!!!_)<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a6.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-363" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a6.jpg" alt="" width="600" height="995" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/a6.jpg 600w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/a6-181x300.jpg 181w" sizes="(max-width: 600px) 100vw, 600px" /></a>
  7. Jakmile máte vše zvolené, klikněte na Nástroje &#8211; Vypálit zavaděč. Arduino bude chvíli blikat, a na konci byste měli dostat nějaký takovýto výpis:<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a7.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-364" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/a7.jpg" alt="" width="467" height="594" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/a7.jpg 467w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/a7-236x300.jpg 236w" sizes="(max-width: 467px) 100vw, 467px" /></a>
  8. Pokud je vše hotové, jásejte &#8211; vaše ATmega se právě proměnila v Arduino.

## Programování ATmega pomocí Arduino IDE

Máte ATmegu s bootloaderem, pojďme si tedy do ní nahrát nějaký program přes Arduino IDE.

V první řadě změňte programátor (Nástroje &#8211; Programátor) na **AVRISP mkII (MightyCore)**.

Pak si otevřte třeba Blink, změňte LED_BUILTIN na konstantu 13, a můžete zkusit překlad.

Já jsem si pro vlastní provoz vzal druhý breadboard, připojil jsem USB-to-UART převodník a &#8222;zadrátoval&#8220; jsem si ho takto:

  * Piny 11 a 31 ATmega na pin GND z převodníku
  * Piny 10 a 30 ATmega na pin +5 V převodníku
  * Pin 14 ATmega na TxD převodníku
  * Pin 15 ATmega na RxD převodníku
  * Pin 9 ATmega přes rezistor 10k na sousední pin 10 (+5 V)
  * Pin 9 ATmega také přes kondenzátor 100 nF na výstup DTR z převodníku

A aby byl vidět nějaký výsledek, tak jsem si na pin 19 ATmega připojil LED. Fyzický vývod číslo 19 je &#8222;Arduino 13&#8220;, jak ukazuje tento obrázek:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/687474703a2f2f692e696d6775722e636f6d2f4652344759634d2e6a7067.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-365" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/687474703a2f2f692e696d6775722e636f6d2f4652344759634d2e6a7067.jpg" alt="" width="800" height="651" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/687474703a2f2f692e696d6775722e636f6d2f4652344759634d2e6a7067.jpg 800w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/687474703a2f2f692e696d6775722e636f6d2f4652344759634d2e6a7067-300x244.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/687474703a2f2f692e696d6775722e636f6d2f4652344759634d2e6a7067-768x625.jpg 768w" sizes="(max-width: 800px) 100vw, 800px" /></a>

Zapojení tedy vypadá nějak takto:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-ard_bb.png" rel="lightbox"><img loading="lazy" class="aligncenter wp-image-366 size-medium" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-ard_bb-277x300.png" alt="" width="277" height="300" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-ard_bb-277x300.png 277w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-ard_bb-768x831.png 768w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/atmega32-ard_bb.png 918w" sizes="(max-width: 277px) 100vw, 277px" /></a>

V reálu trošku jinak, ale plus mínus&#8230;

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_130500_HDR.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-356" src="http://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_130500_HDR.jpg" alt="" width="800" height="450" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_130500_HDR.jpg 800w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_130500_HDR-300x169.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/08/20170813_130500_HDR-768x432.jpg 768w" sizes="(max-width: 800px) 100vw, 800px" /></a>

Pokud máte všechno správně propojené a bootloader funkční, tak nahrání sketche proběhne na první dobrou&#8230;

&nbsp;

<a href="http://s.click.aliexpress.com/e/RzjAqf6" target="_parent"><img src="//ae01.alicdn.com/kf/HTB1ohSuSpXXXXagXXXXq6xXFXXXE/Integrated-Circuits-ATMEGA32A-PU-font-b-ATMEGA32-b-font-AVR-MCU-32K-FLASH-16MHZ-DIP-40.jpg_220x220.jpg" /><span style="display: block;">ATMEGA32A-PU 32K FLASH 16MHZ DIP-40</span></a>

<a href="http://s.click.aliexpress.com/e/nEAa2vn" target="_parent"><img src="//ae01.alicdn.com/kf/HTB1KdKAHFXXXXaMaXXXq6xXFXXXl/CP2102-Module-With-font-b-DTR-b-font-Pin-STC-Downloader-Module-font-b-USB-b.jpg_220x220.jpg" /><span style="display: block;">CP2102 USB to TTL Module With DTR Pin</span></a>