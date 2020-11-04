---
id: 110
title: Ovladače pro CH340G
date: 2016-10-14T15:20:34+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/?p=110
permalink: /ovladace-pro-ch340g/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/10/CH340G-1.png
categories:
  - Hardware
tags:
  - CH340G
  - Ovladače
  - USB
---
Pokud nakupujete elektroniku v Číně, nebo vlastníte levnější klony Arduin, [WeMos D1](https://esp8266.cz/wemos-d1-mini/), NodeMCU a jiných devkitů, pravděpodobně jste narazili na obvod CH340G. Je to obvod, který používají čínští producenti klonů namísto mnohem dražšího obvodu FTDI, především poté, co FTDI provedla tu známou legrandu s bricknutím padělaných obvodů.

CH340G poznáte snadno: je to SOIC16 s nápisem WCH (vzhůru nohama to vypadá jako HDM).

<a href="http://iotta.cz/wp-content/uploads/sites/17/2016/10/ch340g.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-111" src="http://iotta.cz/wp-content/uploads/sites/17/2016/10/ch340g.png" alt="ch340g" width="700" height="394" srcset="https://iotta.cz/wp-content/uploads/sites/17/2016/10/ch340g.png 700w, https://iotta.cz/wp-content/uploads/sites/17/2016/10/ch340g-300x169.png 300w" sizes="(max-width: 700px) 100vw, 700px" /></a>Výhodou je, že Arduino s tímhle obvodem stojí třeba o třetinu míň než s originálním čipem. Nevýhodou je, že pro Win a Mac potřebujete ovladače. A abyste je nemuseli hledat, zde je máte pod jednou střechou:

## Windows 7

Tuto verzi používáme na workshopu Arduino 101 a nemáme s ní problémy: [CH340G driver pro Windows 7, 64bit](/files/ch341ser-win7.zip)

### Windows 8

Tato verze by měla fungovat pro Windows 8 a Windows 10: [CH340G driver pro Windows 8 a Windows 10](/files/ch341ser-win8.zip)

### macOS 10.12 Sierra

Na této verzi způsobí připojení zařízení s obvodem CH340G okamžitý kernel panic. Ovladač pro tuto verzi je zde: [CH340G driver pro macOS 10.12 Sierra](/files/ch34x-install-osx-1.3.zip)

(Předchozí verze macOS mi fungovala s tímto ovladačem: [CH340G driver pro macOS 10.11](/files/ch34x-install-osx.zip))