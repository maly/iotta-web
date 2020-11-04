---
id: 144
title: Digispark
date: 2016-11-30T16:29:26+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/?p=144
permalink: /digispark/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/11/digispark-photo.jpg
categories:
  - Arduino
  - DIY
tags:
  - arduino
  - atmel
  - ATtiny85
  - kit
---
To, že mám rád devkity, to se o mně ví. V poslední době mě oslovil [WeMos D1 Mini](https://esp8266.cz/wemos-d1-mini/) (a nejsem sám, například [LoRaWAN moduly](https://www.arduinotech.cz/produkt/lorawan-arduino-uno/), které tu testuju, mají stejné rozložení vývodů). No a teď mám novou hračku, totiž kit [Digispark](http://digistump.com/products/1) s ATtiny85.

Originál má podobu USB donglu, klony, které seženete za dolar v Číně, mají třeba i MicroUSB konektor. Na destičce je ATtiny85, stabilizátor, dvě LEDky a bižuterie okolo USB &#8211; pullup rezistor a Zenerovy diody.

<a href="http://iotta.uelectronics.info/wp-content/uploads/sites/17/2016/11/digispark.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-large wp-image-145" src="http://iotta.uelectronics.info/wp-content/uploads/sites/17/2016/11/digispark-1024x593.jpg" alt="digispark" width="640" height="371" srcset="https://iotta.cz/wp-content/uploads/sites/17/2016/11/digispark-1024x593.jpg 1024w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/digispark-300x174.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/digispark-768x445.jpg 768w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/digispark.jpg 1183w" sizes="(max-width: 640px) 100vw, 640px" /></a>

Všimněte si, že tu není žádný konvertor, žádné FTDI ani CH340, namísto toho se o celou USB magii stará samotný procesor. ATtiny85 má 8 kB FLASH, 256 bytů RAM, půl kila EEPROM, dva čítače / časovače, watchdog, USI (tedy I2C a SPI v jednom), přerušení, ADC, interní oscilátor a spoustu dalších věcí.

<p class="">
  Programuje se to klidně i přes Arduino IDE. Pro Windows si musíte stáhnout <a href="https://github.com/digistump/DigistumpArduino/releases/download/1.6.7/Digistump.Drivers.zip">USB ovladač</a>. V Arduino IDE si přidejte v Preferences do pole &#8222;Additional boards manager URLs&#8220; položku:
</p>

<pre class="">http://digistump.com/package_digistump_index.json</pre>

Pak v menu Tools &#8211; Board vyberete Board manager a nainstalujete &#8222;Digistump AVR Boards&#8220; (záložka Contributed). A je to.

<a href="http://iotta.uelectronics.info/wp-content/uploads/sites/17/2016/11/digispark-photo.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-146" src="http://iotta.uelectronics.info/wp-content/uploads/sites/17/2016/11/digispark-photo-300x300.jpg" alt="digispark-photo" width="300" height="300" srcset="https://iotta.cz/wp-content/uploads/sites/17/2016/11/digispark-photo-300x300.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/digispark-photo-150x150.jpg 150w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/digispark-photo-768x768.jpg 768w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/digispark-photo.jpg 800w" sizes="(max-width: 300px) 100vw, 300px" /></a>

Při práci jsem vybral desku &#8222;Digispark (default)&#8220;.

V procesoru je použit USB bootloader [Micronucleus](https://github.com/micronucleus/micronucleus), který zabere přibližně 1.5 kB FLASH. Po resetu čeká nějakou dobu na to, jestli s ním začne někdo komunikovat po USB. Pokud ne, spustí normální firmware. Pokud ano, spustí se proces programování.

Programování vypadá tak, že v Arduino IDE dáte Přeložit, a když všechno proběhlo bez problémů, tak se objeví výzva, abyste připojili zařízení. Máte na to 60 sekund. Když během té doby připojíte (nebo resetujete) zařízení, spustí se programování.

Zapojení je patrné ze schématu výš. Pro jistotu ještě tabulka:

<table>
  <tr>
    <th>
      digitalPin
    </th>
    
    <th>
      AVR pin
    </th>
    
    <th>
      Funkce
    </th>
  </tr>
  
  <tr>
    <td>
    </td>
    
    <td>
      5
    </td>
    
    <td>
      PB0/MOSI/SDA
    </td>
  </tr>
  
  <tr>
    <td>
      1
    </td>
    
    <td>
      6
    </td>
    
    <td>
      PB1/MISO/(LED)
    </td>
  </tr>
  
  <tr>
    <td>
      2
    </td>
    
    <td>
      7
    </td>
    
    <td>
      PB2/SCK/SCL/AD1
    </td>
  </tr>
  
  <tr>
    <td>
      3
    </td>
    
    <td>
      2
    </td>
    
    <td>
      PB3/AD3/USB D-
    </td>
  </tr>
  
  <tr>
    <td>
      4
    </td>
    
    <td>
      3
    </td>
    
    <td>
      PB4/AD2/USB D+
    </td>
  </tr>
  
  <tr>
    <td>
      5
    </td>
    
    <td>
      1
    </td>
    
    <td>
      PB5/Reset
    </td>
  </tr>
</table>





A k čemu jsem to použil? Nechte se překvapit, bude to v dalším článku!

Odkazy:

  * [Digispark Basics](http://digistump.com/wiki/digispark/tutorials/basics)
  * [Digispark Connecting](http://digistump.com/wiki/digispark/tutorials/connecting)
  * [V-USB](https://www.obdev.at/products/vusb/index-de.html)