---
id: 22
title: ATtiny a Arduino
date: 2016-07-24T11:20:33+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/attiny-a-arduino/
permalink: /attiny-a-arduino/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/09/pinoutattiny2313.png
categories:
  - Arduino
  - Arduino
  - Hardware
tags:
  - arduino
  - attiny2313
---
V [EduShieldu](http://retrocip.uelectronics.info/vyukovy-shield-pro-arduino-dil-druhy/) jsem použil pro řízení displeje jednočip ATtiny2313. Je to vlastně legendární AT90S2313, ale s novějším jádrem a novými periferiemi. Původně jsem sice chtěl použít jiný typ, ale ten nebyl dostupný. Inu co už, použil jsem tento.

Otázka ale byla: Jak tu Atinu (familiární označení, jistě to říkáte taky&#8230;) naprogramovat. A protože jde o shield pro Arduino, nabízelo se naprosto jednoduché řešení, totiž využít Arduino jako programátor.

Dokonce na to mají i tutoriály &#8211; [Arduino as ISP](https://www.arduino.cc/en/Tutorial/ArduinoISP). Stačí k tomu pár propojovacích drátů a jeden kondenzátor (ten tam neukazují, ale je zapojený mezi RESET a zem na Arduinu).

<img loading="lazy" class="aligncenter size-medium wp-image-852" src="http://retrocip.uelectronics.info/wp-content/uploads/sites/6/2016/07/pinoutattiny2313-650x352.png" alt="pinoutattiny2313" width="650" height="352" /> 



Propojení je prosté:

| Arduino    | ATTiny        |
| ---------- | ------------- |
| digital 10 | pin 1 (RESET) |
| digital 11 | pin 17 (MOSI) |
| digital 12 | pin 18 (MISO) |
| digital 13 | pin 19 (SCK)  |
| GND        | pin 10        |
| 5V         | pin 20        |

Nejprve ale [nahrajte do Arduina sketch &#8222;ArduinoISP&#8220;](https://www.arduino.cc/en/Tutorial/ArduinoISP). Najdete ho mezi Examples.

Jako druhý krok připojte elektrolyt ~10µF mezi zem a RESET na Arduinu (záporným pólem k zemi, logicky).

Teď můžete vesele připojit ATTiny2313 a programovat.

No jo, to se řekne &#8211; programovat. Ale čím?

Já jsem si řekl, že bude kůl a vůbec užitečné, když firmware bude taky sketch pro Arduino. A jak jsem řekl, tak jsem i udělal. K tomu, aby to celé fungovalo, ale musíte do Arduina přidat podporu pro ATtiny. Já použil [tuto](https://code.google.com/archive/p/arduino-tiny/), ale chtělo to trošku laborovat s cestami v souboru boards. Ony se totiž mezi verzí 1.5 a aktuální 1.6 změnily, a tak byl překlad spíš festival chyb než radostná událost. Ale nakonec se podařilo.

Vybral jsem příklad &#8222;blink&#8220;, přeložil ho pro desku &#8222;ATtiny2313@1MHz&#8220;, vybral jsem programátor &#8222;Arduino as ISP&#8220; a tradá &#8211; vše fungovalo.

Trošku trápení bylo s I2C v roli &#8222;slave&#8220;, ale nakonec po chvilce bastlení s přerušením začala Atina přijímat povely a reagovala na ně. Standardní knihovna Wire totiž moc nešlape, tak jsem zvolil [TinyWireS](https://github.com/rambo/TinyWire/tree/master/TinyWireS).

(Netestoval jsem [Arduino Tiny Core](https://github.com/SpenceKonde/ATTinyCore) od Spencera Konde, vypadá zajímavě).

S touto sestavou tedy trvalo vytvoření firmware pro EduShield jedno odpoledne. Naprogramování pak vyžaduje jen šest drátů, kondenzátor a pět minut práce.