---
id: 51
title: Displej pro MQTT
date: 2016-09-22T11:45:28+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/displej-pro-mqtt/
permalink: /displej-pro-mqtt/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/09/14355731_10153964291112496_6747077918353356864_n.jpg
categories:
  - Arduino
  - DIY
  - ESP8266
  - Hardware
  - MQTT
tags:
  - bigclown
  - esp8266
  - oled
  - wemos
---
Ke svému měřicímu soustrojí (viz [minulé](http://retrocip.uelectronics.info/esp8266-mqtt/) [články](http://retrocip.uelectronics.info/zpracovavam-data-z-mqtt/)) jsem si dobastlil malinkatý displej pro jednu hodnotu.

Vyšel jsem ze svého oblíbeného kitu [WeMos D1 Mini](http://wemos.cz/) a k němu jsem přes devboard připojil miniaturní displej OLED. WeMos sice nabízí OLED shield, ale s polovičním rozlišením, než jsem si sehnal sám. Displej má rozhraní I2C, tak jsem ho zapojil stejně, jako má originál, tedy SCL na D1 a SDA na D2 (což [odpovídá GPIO 4 a 5](http://esp8266.cz/wemos-d1-mini/wemos-d1-mapovani-vyvodu-ruznych-verzi/)).

Použil jsem knihovnu od Adafruit ([Adafruit SSD1306](https://github.com/mcauser/Adafruit_SSD1306/tree/esp8266-64x48)), a k ní samozřejmě Adafruit GFX. Při překladu mi řval kompiler, že je potřeba upravit soubor Adafruit_SSD1306.h &#8211; tak jsem ho upravil. Úprava spočívá v zakomentování a odkomentování řádků podle konfigurace displeje &#8211; takto:

<pre class="lang:default decode:true">#define SSD1306_128_64
//   #define SSD1306_128_32
//   #define SSD1306_96_16</pre>

Dál jsem použil PubSubClient pro MQTT a knihovnu WiFiManager.

[Zdrojáky jsou na GitHubu.](https://github.com/maly/mqtt-display)

Připojuju se ke svému MQTT serveru, kam mi tečou data z [BigClown](https://www.bigclown.com)ích senzorů. Chytám si topic s prvním teploměrem, a payload ani moc neparsuju, jen úplně primitivně hledám znak &#8222;[&#8222;, který v payloadu označuje začátek dat, a kopíruju čtyři znaky. Ty pak zobrazím na displeji. Pokud budete mít data jinak, upravte si příslušnou pasáž v [kódu](https://github.com/maly/mqtt-display).

Neřeším žádné uspávání ani šetření baterie, jedu furt naplno &#8211; počítám s tím, že displej poběží napojený na adaptér.

Další úpravy: co třeba střídání několika různých hodnot po deseti sekundách, a k tomu nějaký titulek?

<a href="http://iotta.cz/wp-content/uploads/sites/17/2016/09/14355731_10153964291112496_6747077918353356864_n.jpg" rel="lightbox"><img loading="lazy" class="aligncenter wp-image-52 size-full" src="http://iotta.cz/wp-content/uploads/sites/17/2016/09/14355731_10153964291112496_6747077918353356864_n.jpg" alt="14355731_10153964291112496_6747077918353356864_n" width="528" height="960" /></a>