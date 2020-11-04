---
id: 8
title: 'Zpracovávám data z MQTT&#8230;'
date: 2016-08-28T11:19:46+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/zpracovavam-data-z-mqtt/
permalink: /zpracovavam-data-z-mqtt/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/09/bc0.png
categories:
  - DIY
  - Hardware
  - MQTT
tags:
  - esp8266
  - iot
  - mqtt
  - thingspeak
---
V [minulém článku](http://retrocip.uelectronics.info/esp8266-mqtt/) jsem nastínil postup sběru dat ze snímačů (použil jsem tagy [BigClown](https://bigclown.com)). Data hezky tečou do MQTT a já je mohu sledovat, ale to se mi moc nehodí. Já totiž v první řadě potřebuju sledovat, jak mi klesá napětí na baterii.

  * Možnost číslo 1 &#8211; pustit si nějakého démona, co bude logovat napětí a někam mi to zapisovat. Moc se mi nelíbila. Je to moc práce a málo užitku. Určitě by se našla nějaká
  * Možnost číslo 2 &#8211; tedy taková, kde použiju už něco existujícího.

Fajn, tak co se nabízí? Třeba [Thingspeak](https://thingspeak.com). Má to jednoduché API, kam lze posílat data, a jednoduchý dashboard, kde lze vytvořit grafy pro různé hodnoty. Nevýhoda: nemá to MQTT. Takže je potřeba něco mezi.

Naprostou náhodou jsem objevil věc, co se jmenuje [Node-RED](http://nodered.org/). Běží to na Node.js, a je to takový vizuální editor toku dat. Doleva si dáte MQTT, pak to nějak zpracujete, pak to pošlete ven. Jednoduché jak trojky vidle.

<a href="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/08/bc0.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-880" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/08/bc0-650x308.png" alt="bc0" width="650" height="308" /></a>

Hezké je, že na vstupu může být spousta různých věcí, od MQTT přes HTTP a TCP až po sériový port. Na výstupu totéž. Nebo soubor. Nebo ještě něco jiného. Třeba Twitter. Nebo něco vlastního &#8211; kupříkladu ten Thingspeak.

Já čtu data ze tří MQTT topiců, preparuju si z nich pouze číslo, pak tři zprávy (teplota, osvětlení a baterie) skládám do jedné, v ní dělám nějaké drobné úpravy a volám API Thingspeak. Všechno bylo velmi intuitivní a nastavené během půl hodiny.

Navíc se to všechno dá programovat v JavaScriptu a můžete si udělat vlastní konektory a funkce.

Složité komponenty si můžete rozdělit do &#8222;subflows&#8220; &#8211; já mám takhle celé zařízení zapouzdřené do subflow &#8222;ESPclown&#8220;. V něm se skládají data ze tří senzorů:

<a href="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/08/bc2.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-881" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/08/bc2.png" alt="bc2" width="531" height="240" /></a>

A každý senzor se skládá z konektoru na MQTT a zpracování payloadu:

<a href="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/08/bc3.png" rel="lightbox"><img loading="lazy" class="aligncenter size-medium wp-image-882" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/08/bc3-650x150.png" alt="bc3" width="650" height="150" /></a>

A výsledek? Hezký:







&nbsp;