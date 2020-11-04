---
id: 17
title: Levný kit se STM32F103C8 (plus Arduino)
date: 2016-07-25T11:20:24+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/levny-kit-se-stm32f103c8-plus-arduino/
permalink: /levny-kit-se-stm32f103c8-plus-arduino/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/09/miniSTM32.png
categories:
  - Arduino
  - Hardware
  - STM32
tags:
  - arduino
  - arm
  - stm32
---
Před časem jsem zahlédl pár velmi levných kitů s procesorem STM32F103C8 (zvané též [BluePill](http://bluepill.cz)). Vlastně jen procesor na desce se spoustou vývodů, k tomu jedna LEDka, tlačítko a stabilizátor napájení 3.3V, a to za naprosto směšnou cenu. Vzal jsem jich rovnou pytel, a k nim přihodil i čínskou kopii ST-Link v.2





Přišlo to, já to zapojil přes ST-Link, přeložil jsem program v C, a po nějakém tom laborování jsem ho nahrál do jednočipu. LEDka neblikala, protože &#8211; nevím proč, asi proto, že u ARMu neumím nastavit port na blikání nebo tak něco. 🙂

Čínský &#8222;ST-Link v2&#8220; je taky podivnost sama. Nějak to fungovalo, ale jinak to jako spíš bolelo. Valná část utilit hlásí, že žádný ST-Link nevidí, a tak.

<img loading="lazy" class="aligncenter size-medium wp-image-857" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/07/STM32-Minimum-Page-1-650x590.png" alt="STM32 Minimum - Page 1" width="650" height="590" /> 

Což mě trochu hnětlo, protože jsem se zrovna na tuhle desku těšil. Použitý procesor [STM32F103C8](http://www.st.com/content/ccc/resource/technical/document/datasheet/33/d4/6f/1d/df/0b/4c/6d/CD00161566.pdf/files/CD00161566.pdf/jcr:content/translations/en.CD00161566.pdf) má jádro ARM Cortex M3, 64 kB FLASH, 20 kB SRAM a spoustu dalších hezkých vlastností, třeba spoustu periferií. Navíc na desce je téměř &#8222;holý&#8220;, kromě PC13 není zabraný žádný pin, je připojený (mikro)USB konektor, takže máte hodně velkou volnost.

<img loading="lazy" class="aligncenter size-medium wp-image-858" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/07/arduino_stm32f103c8t6_schematics-650x451.png" alt="" width="650" height="451" /> 

Hledal jsem tedy další možnost, jak tuto desku programovat, a našel jsem.

Dvojice pinheaderů jsou totiž vývody BOOT0 a BOOT1, a vy si pomocí jumperů nadefinujete, jaké jsou na těchto vstupech hodnoty. Pokud bude BOOT0 roven 1 a BOOT1 roven 0, ocitne se po RESETu čip v [boot loader módu](http://www.st.com/content/ccc/resource/technical/document/application_note/b9/9b/16/3a/12/1e/40/0c/CD00167594.pdf/files/CD00167594.pdf/jcr:content/translations/en.CD00167594.pdf), v němž je možné naprogramovat jej pomocí UART1 &#8211; tedy signály TX1 (PA9) a RX1 (PA10).

Pak tedy stačí už jen připojit USB-to-UART převodník (třeba oblíbený CH340, Prolific PL2303 nebo CP1202 &#8211; **pozor! Musí umět 3.3V!**) a stisknout tlačítko RESET.







A k samotnému programování jsem použil Arduino IDE. Použil jsem 1.6.8, a měl jsem problémy (cesty apod.), ale [po updatu na 1.6.9](https://github.com/rogerclarkmelbourne/Arduino_STM32/issues/156) vše funguje bez problémů.

Klíčem k úspěchu je knihovna [Arduino_STM32](https://github.com/rogerclarkmelbourne/Arduino_STM32). Stáhněte si ji a nahrajte do složky &#8222;hardware&#8220; v Arduinu (viz [Instalace](https://github.com/rogerclarkmelbourne/Arduino_STM32/wiki/Installation)). Po restartu Arduino IDE se v Manažeru desek objeví nové možnosti. Zvolil jsem desku &#8222;Generic STM32F103C series&#8220;, variantu &#8222;C8, 20kRAM, 64k Flash&#8220; a jako nahrávací metodu Serial (experimetoval jsem i se STLink, ale výsledek nebyl nijak pěkný).

První přeložený sketch byl tento:

<pre class="lang:arduino decode:true ">#define pinLED PC13

void setup() {
  Serial.begin(9600);
  pinMode(pinLED, OUTPUT);
  Serial.println("START");  
}

void loop() {
  digitalWrite(pinLED, HIGH);
  delay(1000);
  digitalWrite(pinLED, LOW);
  delay(1000);
  Serial.println("Hello World");  
}</pre>

Překlad je o něco delší než ten pro Arduino. Důležité je nezapomenout na přehození BOOT0 na hodnotu 1 a stisknutí RESET před nahráním. Pokud chcete, aby při příštím spuštění už boot nenabíhal, hoďte switch zpátky na 0.

Další tipy: [Sunspot STM32 Arduino](http://www.sunspot.co.uk/Projects/Arduino/STM32/STM32.html), [Arduino goes STM32](http://grauonline.de/wordpress/?page_id=1004)