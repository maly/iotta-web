---
id: 156
title: Konzola pro I2C
date: 2016-12-11T18:10:42+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=156
permalink: /konzola-pro-i2c/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/12/I²C_bus_logo.svg_.png
categories:
  - Arduino
  - Arduino
tags:
  - arduino
  - I2C
---
Testuju teď nějaká zařízení s rozhraním I2C a hodilo by se mi mít možnost je snadno ovládat a testovat.

Jasně, je to jednoduché, připojím je k Arduinu, napíšu si program, vypíšu hodnoty&#8230; Jenže při testování chci simulovat různé věci, zápisy, čtení a kdesi cosi, a pokaždé překládat program není moc příjemné. Tak jsem si udělal takovou konzolu &#8211; máte připojenou sériovou linku a prostě posíláte příkazy. Žádná sofistikovanost, jen ty nejzákladnější: zapiš, přečti, zapiš + přečti.

Formát příkazů je následující:

<pre class="">[2 hexadecimální znaky adresy](1 znak příkaz)[parametry podle příkazu - vždy dvoumístná hexadecimální čísla]</pre>

Když chci poslat dva bajty, třeba 01 a 55, do zařízení 0x27, pošlu po sériové lince toto:

<pre class="">27.0155</pre>

Tedy hexadecimálně adresu (27), příkaz pro zápis (tečka) a bajty, co se mají poslat.

Pro čtení používám příkaz &#8222;otazník&#8220; a za ním počet bajtů, které chci přečíst (vždy dvoumístné hexadecimální číslo):

<pre class="">27?02</pre>

přečte dva bajty ze zařízení 27.

A když chci zapsat a číst, třeba s RTC DS1307 &#8211; zapíšu 0 a načtu 7 bajtů v jedné transakci:

<pre class="">68.00?07</pre>

Tedy zařízení 68 (DS1307), zapisuju (tečka) jeden bajt s hodnotou 00, a pak čtu (otazník) 07 bajtů.

A abych nemusel psát furt dokola adresu, udělal jsem si takovou zkratku &#8211; napíšu třeba &#8222;27!&#8220; (tedy příkaz vykřičník) a tím řeknu, že všechny další příkazy, pokud budou bez adresy, použijou tuto zadanou. Takže třeba výše zmíněné příkazy:

<pre class="">27!
.0155
?02</pre>

Už adresu neuvádím, konzole bere tu, co jsem zadal s vykřičníkem. Pokud ji uvedu, použije se samozřejmě ta, co jsem uvedl

Kód je jednoduchý, máte ho tu v gistu: