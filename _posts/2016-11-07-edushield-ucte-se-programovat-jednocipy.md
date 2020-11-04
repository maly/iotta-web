---
id: 128
title: 'EduShield: učte se programovat jednočipy'
date: 2016-11-07T14:49:21+00:00
author: Martin Maly
layout: post
guid: http://iotta.uelectronics.info/?p=128
permalink: /edushield-ucte-se-programovat-jednocipy/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/11/20161107_145537.jpg
categories:
  - Arduino
  - Hardware
tags:
  - arduino
  - CZ.NIC
  - EduShield
---
Před téměř rokem a půl jsme se Štěpánem Bechynským začali objíždět republiku s workshopem [Arduino 101](http://arduino101.cz). Cílem tohoto workshopu bylo přivést lidi k Arduino a elektronice vůbec, a ukázat jim hlavně to, že není potřeba se bát, že to nezvládnete, nebo se ostýchat.

Během té doby prošlo workshopem na 400 lidí, od dvanáctiletých školáků až po seniory, muži i ženy, programátoři i neprogramátoři&#8230; Cílem workshopu nebylo vytvořit za tři hodiny dvanáct elektroinženýrů, to ani náhodou. Cílem bylo ukázat těm dvanácti lidem zajímavý svět, o kterém věděli, jak je velký a zajímavý, ale báli se, že se v něm nevyznají. Udělali jsme s nimi prvních pár kroků a ukázali základy. Ne, žádné složitosti, žádná &#8222;nejdřív-teorie-pak-teprve-praxe&#8220;, žádné nudné povídání na hodinu&#8230; Praktická ukázka, pak vysvětlení, proč to tak je, co se děje, pak nechat účastníky, ať si zkusí nějaké vlastní úlohy&#8230; Ohmův zákon se doučíte doma, na to, abyste si postavili &#8222;bastlteploměr&#8220; ho nepotřebujete. Věříme totiž, že je nejdůležitější zaujmout a zbavit počátečního strachu. Pak teprve přijde na řadu teorie, bez které nemá cenu se do něčeho většího pouštět. Ale upřímně: když chcete někoho nadchnout pro mikroelektroniku, tak je lepší přijít a říct: &#8222;Udělejte toto, a vidíte, bliká to! Takhle to je jednoduché!&#8220; a pak vysvětlit, proč a co je za tím, než si před něj stoupnout a vyprávět mu dvě hodiny o proudu, napětí, TTL, tranzistoru, P-N přechodu, &#8230;

První pokusy proběhly s takovými těmi starter kity, které znáte: Arduino, pytel součástek, halda drátů, nepájivé pole. Ale tenhle přístup má zásadní nevýhodu: pokud učíte začátečníky, tak se stane, že někdo něco špatně zapojí, že nějakou součástku otočí, a následuje složité hledání příčiny, které zbrzdí celý workshop. Tato nevýhoda, alespoň podle nás, převáží nad výhodou (&#8222;opravdu si to osaháte&#8220;).

Druhá věc, kterou jsme řešili, byl nedostatek výukových materiálů. Ne snad že by nebyly nikde příklady, těch je dost. Co chybělo, alespoň nám, byly metodické materiály, tedy postupy co ukazovat, jak, jaké techniky zvolit, na co dát důraz, co kterým příkladem demonstrujete, &#8230; To byla i nejčastější otázka ze skupiny lidí, kterým pracovně říkám &#8222;táta s dětmi&#8220; &#8211; tedy rodič, který sám nějaké povědomí má, a rád by svou ratolest v její zvídavosti podpořil, ovšem nenapadá ho, co je adekvátní jaké úrovni znalostí, a nenapadají ho třeba ani náměty na samostatnou činnost.

Nakonec jsme se tedy rozhodli použít shield, původně určený k zapojování digitálních hodin. Výhodou bylo, že obsahoval několik různých senzorů (termistor, fotorezistor, tlačítka), a zároveň i několik různých výstupů (LED, repro) a periferií (hodiny reálného času DS1307, čtyřmístný LED displej). Bohužel jeho zapojení nebylo úplně šťastné &#8211; neumožňoval demonstraci HW přerušení, a nedovoloval ani připojení Ethernet shieldu, protože obsazoval některé piny pro SPI.

Proto jsme nakonec spojili síly se [sdružením CZ.NIC](https://www.nic.cz/), které naše myšlenka zaujala, a společnými silami jsme navrhli a vyrobili desku, nazvanou EduShield.

<a href="http://iotta.uelectronics.info/wp-content/uploads/sites/17/2016/11/20161107_145525.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-129" src="http://iotta.uelectronics.info/wp-content/uploads/sites/17/2016/11/20161107_145525.jpg" alt="20161107_145525" width="800" height="450" srcset="https://iotta.cz/wp-content/uploads/sites/17/2016/11/20161107_145525.jpg 800w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/20161107_145525-300x169.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/20161107_145525-768x432.jpg 768w" sizes="(max-width: 800px) 100vw, 800px" /></a>EduShield vychází z hardware, který jsme použili v kurzu Arduino 101, ale opravuje výše zmíněné problematické části. Vynechali jsme reproduktor, který způsoboval při výuce někdy krušné okamžiky, místo něj jsme přidali RGB LED, buzenou přes PWM, tedy se schopností plynulého rozsvěcení a zhasínání. Tlačítko jsme připojili na vstup, který dokáže vyvolat přerušení; stejně tak i výstup ALARM od hodin reálného času.

Zajímavý kompromis jsme udělali u displeje. Originální shield používal čínský obvod TM163x, který jsme použít nechtěli. Místo něho jsme navrhli použití specializovaného budiče od firmy Maxim. Ten se ale ukázal jako poměrně drahý, a tak jsme jej nahradili jednočipem &#8211; konkrétně ATtiny2313. Tento procesor je naprogramován tak, že svými výstupy budí čtyřmístný sedmisegmentový displej, a pro Arduino se tváří jako periferie na sběrnici I2C. Což nám zároveň otevřelo další možnosti. Firmware pro tento procesor je napsaný rovněž v jazyce Wiring, a lze jej do procesoru nahrát pomocí Arduina. Shield tak můžete přeprogramovat, překonfigurovat, a namísto displeje použít třeba úplně jinou periferii.

<a href="http://iotta.uelectronics.info/wp-content/uploads/sites/17/2016/11/20161107_145552.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-131" src="http://iotta.uelectronics.info/wp-content/uploads/sites/17/2016/11/20161107_145552.jpg" alt="20161107_145552" width="800" height="450" srcset="https://iotta.cz/wp-content/uploads/sites/17/2016/11/20161107_145552.jpg 800w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/20161107_145552-300x169.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/20161107_145552-768x432.jpg 768w" sizes="(max-width: 800px) 100vw, 800px" /></a>

CZ.NIC se ujal výroby hardware, a v těchto dnech vzniká série 300 kusů. Cena se bude, podle předběžných informací, pohybovat někde okolo 200 Kč. Tato první série je primárně zaměřená na výuku lektorů &#8211; spolu s Akademií CZ.NIC se chystáme &#8222;vyškolit školitele&#8220;, kteří budou tento shield používat při vlastní výuce. Chtěli bychom oslovit i školy, pro které by tento výukový kit mohl být zajímavý svými možnostmi i dostupností, a proto bychom rádi, právě ve spolupráci s CZ.NIC, zajistili akreditaci kurzu v rámci DVPP.

Jak metodické materiály, které právě dolaďujeme, tak samotný hardware i software jsou pod svobodnými licencemi (MIT, CC).

Informace o dostupnosti i o možnosti přihlásit se do kurzů budou na stránkách CZ.NIC, nebo je najdete i zde.

<a href="http://iotta.uelectronics.info/wp-content/uploads/sites/17/2016/11/20161107_145558.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-132" src="http://iotta.uelectronics.info/wp-content/uploads/sites/17/2016/11/20161107_145558.jpg" alt="20161107_145558" width="800" height="450" srcset="https://iotta.cz/wp-content/uploads/sites/17/2016/11/20161107_145558.jpg 800w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/20161107_145558-300x169.jpg 300w, https://iotta.cz/wp-content/uploads/sites/17/2016/11/20161107_145558-768x432.jpg 768w" sizes="(max-width: 800px) 100vw, 800px" /></a>