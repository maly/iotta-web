---
id: 135
title: Nahrání firmware do EduShieldu (Aktualizováno 09/2020)
date: 2016-11-09T13:57:37+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/?p=135
permalink: /nahrani-firmware-do-edushieldu/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/11/20161109_141225.jpg
categories:
  - Arduino
tags:
  - arduino
  - attiny2313
  - EduShield
  - ISP
  - TinyWireS
---
V EduShieldu je displej řízen pomocí ATtiny2313. V něm je firmware, napsaný ve Wiring. Tento firmware se dá jednoduše nahrát pomocí Arduina a Arduino IDE. Zde je step-by-step postup &#8222;jak na to&#8220;.

**AKTUALIZACE září 2020**

Původní návod naleznete níž, k němu jen několik změn:

**V kroku 2 je drobná změna**, není zapotřebí stahovat nic. Knihovna pro EduShield je v seznamu knihoven Arduina, lze ji tedy nainstalovat přes Manažér knihoven:<figure class="wp-block-image size-large">

<img loading="lazy" width="795" height="452" src="https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2020/09/edushield1.png" alt="" class="wp-image-386" srcset="https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2020/09/edushield1.png 795w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2020/09/edushield1-300x171.png 300w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2020/09/edushield1-768x437.png 768w" sizes="(max-width: 795px) 100vw, 795px" /> </figure> 

Je to ta první, ta, kde je v popisech uvedeno, že jde o shield od CZ.NIC

Krok 3 a další platí, až do kroku 12.

**V kroku 12 došlo ke změně**, desku nyní najdete pod jiným názvem:<figure class="wp-block-image size-large">

<img loading="lazy" width="848" height="445" src="https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2020/09/edushield2.png" alt="" class="wp-image-387" srcset="https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2020/09/edushield2.png 848w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2020/09/edushield2-300x157.png 300w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2020/09/edushield2-768x403.png 768w" sizes="(max-width: 848px) 100vw, 848px" /> </figure> 

Zbytek návodu platí tak, jak je napsán.

**Původní návod**

  1. Aktualizujte Arduino IDE
  2. Stáhněte si [aktuální software pro EduShield](https://github.com/maly/edushield). _(Pozor, viz aktualizace výše)_
  3. Ve složce _firmware naleznete podsložky &#8222;hardware&#8220;, &#8222;libraries&#8220; a &#8222;tiny2313&#8220;. Obsah složek &#8222;hardware&#8220; a &#8222;libraries&#8220; zkopírujte do pracovního adresáře Arduina (nejčastěji v domovském adresáři, podsložka Arduino). Složka &#8222;libraries&#8220; bude pravděpodobně existovat, &#8222;hardware&#8220; možná ne, tak jej vytvořte.
  4. Spusťte Arduino IDE a připojte Arduino Uno, kterým budete programovat. Bez EduShieldu!
  5. Z menu &#8222;Soubor &#8211; příklady&#8220; vyberte &#8222;Arduino ISP&#8220; a běžným způsobem jej nahrajte do Arduina.
  6. Z EduShieldu sundejte displej
  7. Switch J6 nad displejem rozpojte, viz obrázek (to je to modré nahoře pod piny 12, 11, označené RTC PWR):  
    <a rel="lightbox" href="http://iotta.cz/wp-content/uploads/sites/17/2016/11/20161109_141054.jpg"><img loading="lazy" width="800" height="450" class="aligncenter size-full wp-image-137" src="http://iotta.cz/wp-content/uploads/sites/17/2016/11/20161109_141054.jpg" alt="20161109_141054" srcset="https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/20161109_141054.jpg 800w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/20161109_141054-300x169.jpg 300w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/20161109_141054-768x432.jpg 768w" sizes="(max-width: 800px) 100vw, 800px" /></a>
  8. Propojte pomocí šesti propojovacích vodičů EduShield (šestivývodový konektor označený J3 ISP) s Arduinem (+5V, GND, datové piny 10, 11, 12 a 13). Správné propojení je naznačeno na následujícím obrázku:  
    <a rel="lightbox" href="http://iotta.cz/wp-content/uploads/sites/17/2016/11/Datový-zdroj-1.png"><img loading="lazy" width="495" height="366" class="aligncenter size-full wp-image-138" src="http://iotta.cz/wp-content/uploads/sites/17/2016/11/Datový-zdroj-1.png" alt="datovy-zdroj-1" srcset="https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/Datový-zdroj-1.png 495w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/Datový-zdroj-1-300x222.png 300w" sizes="(max-width: 495px) 100vw, 495px" /></a>
  9. Připojte elektrolytický kondenzátor cca 4.7uF či větší mezi piny RST a GND na Arduinu, jak je znázorněno na fotografii:  
    <a rel="lightbox" href="http://iotta.cz/wp-content/uploads/sites/17/2016/11/20161109_141128.jpg"><img loading="lazy" width="800" height="450" class="aligncenter size-full wp-image-136" src="http://iotta.cz/wp-content/uploads/sites/17/2016/11/20161109_141128.jpg" alt="20161109_141128" srcset="https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/20161109_141128.jpg 800w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/20161109_141128-300x169.jpg 300w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/20161109_141128-768x432.jpg 768w" sizes="(max-width: 800px) 100vw, 800px" /></a>
 10. Propojené komponenty by měly vypadat zhruba takto:  
    <a rel="lightbox" href="http://iotta.cz/wp-content/uploads/sites/17/2016/11/20161109_141225.jpg"><img loading="lazy" width="800" height="450" class="aligncenter size-full wp-image-140" src="http://iotta.cz/wp-content/uploads/sites/17/2016/11/20161109_141225.jpg" alt="20161109_141225" srcset="https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/20161109_141225.jpg 800w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/20161109_141225-300x169.jpg 300w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/20161109_141225-768x432.jpg 768w" sizes="(max-width: 800px) 100vw, 800px" /></a>
 11. Spusťte Arduino IDE a otevřete sketch Tiny2313 ze složky _firmware
 12. Vyberte jako desku &#8222;ATtiny2313 @ 1 MHz&#8220; a jako programátor &#8222;Arduino as ISP&#8220;, viz screenshot:  
    <a rel="lightbox" href="http://iotta.cz/wp-content/uploads/sites/17/2016/11/edushprog1.png"><img loading="lazy" width="395" height="298" class="aligncenter size-full wp-image-139" src="http://iotta.cz/wp-content/uploads/sites/17/2016/11/edushprog1.png" alt="edushprog1" srcset="https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/edushprog1.png 395w, https://maly.github.io/iotta-web/wp-content/uploads/sites/17/2016/11/edushprog1-300x226.png 300w" sizes="(max-width: 395px) 100vw, 395px" /></a>
 13. Přeložte a spusťte nahrávání.
 14. Po úspěšném nahrání odpojte EduShield od Arduina
 15. Vraťte zpátky switch J6 (musí spojovat oba vývody) a nasaďte displej.

Pro zájemce: přiřazení pinů v ATtiny2313 k vývodům na konektoru pro displej (pin 1 je vlevo dole):

AVR<figure class="wp-block-table">

<table>
  <tr>
    <td>
      PB4
    </td>
    
    <td>
      PB1
    </td>
    
    <td>
      PB0
    </td>
    
    <td>
      PB3
    </td>
    
    <td>
      PB2
    </td>
    
    <td>
      PD6
    </td>
  </tr>
  
  <tr>
    <td>
      PA1
    </td>
    
    <td>
      PA0
    </td>
    
    <td>
      PD2
    </td>
    
    <td>
      PD3
    </td>
    
    <td>
      PD4
    </td>
    
    <td>
      PD5
    </td>
  </tr>
</table></figure> 

Arduino<figure class="wp-block-table">

<table>
  <tr>
    <td>
      D13
    </td>
    
    <td>
      D10
    </td>
    
    <td>
      D9
    </td>
    
    <td>
      D12
    </td>
    
    <td>
      D11
    </td>
    
    <td>
      D8
    </td>
  </tr>
  
  <tr>
    <td>
      D2
    </td>
    
    <td>
      D3
    </td>
    
    <td>
      D4
    </td>
    
    <td>
      D5
    </td>
    
    <td>
      D6
    </td>
    
    <td>
      D7
    </td>
  </tr>
</table></figure> 

LED (CA označují pozice, SEG jednotlivé segmenty)<figure class="wp-block-table">

<table>
  <tr>
    <td>
      CA1
    </td>
    
    <td>
      SEGA
    </td>
    
    <td>
      SEGF
    </td>
    
    <td>
      CA2
    </td>
    
    <td>
      CA3
    </td>
    
    <td>
      SEGB
    </td>
  </tr>
  
  <tr>
    <td>
      SEGE
    </td>
    
    <td>
      SEGD
    </td>
    
    <td>
      SEGH(DP)
    </td>
    
    <td>
      SEGC
    </td>
    
    <td>
      SEGG
    </td>
    
    <td>
      CA4
    </td>
  </tr>
</table></figure>