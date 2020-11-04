---
id: 253
title: Základy číslicové techniky IV
date: 2017-03-04T17:18:58+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=253
permalink: /zaklady-cislicove-techniky-iv/
image: http://iotta.cz/wp-content/uploads/sites/17/2017/03/Computerplatine_Wire-wrap_backplane_detail_Z80_Doppel-Europa-Format_1977.jpg
categories:
  - DIY
  - Hardware
tags:
  - Číslicová technika
---
Pojďme si teď udělat drobnou odbočku k praxi, a pak zase k teorii. Dneska nebudeme v simulátoru skládat hradla, místo toho si řekneme, jak takové hradlo vypadá ve skutečnosti, a co přesně máte udělat, když s ním chcete pracovat. Ukážeme si základní pomůcky a řekneme si něco o terminologii. Nebojte se, bude to i přesto zábavné.

<!--more-->

## Sběrnice

Na konci minulého dílu jsme si ukázali sčítačku pro čtyřbitová čísla. Mohli jste si všimnout toho, že pro každý bit čtyřbitového slova musíme natáhnout jeden vodič. Když máme obvod, třeba čtyřbitovou sčítačku, a chceme ji připojit na čtyřbitový zobrazovač, musíme vodivě propojit nejnižší bit výstupu sčítačky s nejnižším bitem vstupu zobrazovače, a totéž pro bit 1, pro bit 2 i pro bit 3 (všimněte si, že **bity se číslují od nuly**). Zatím to jde, ale představte si osmibitový, šestnáctibitový, dvaatřicetibitový systém... Ano, i tam musíme fyzicky natahat vodiče mezi výstupem a vstupem. Pokud propojujeme 64bitový procesor, tedy takový, který má fyzicky 64 bitů datové sběrnice, s 64bitovou pamětí, musíme fyzicky udělat 64 propojení.

Aby se ze schémat nestala změť čar, používá se koncept **sběrnice**. Místo mnoha vodičů nakreslíme jeden silnější a popíšeme jej tak, aby bylo jasné, jaká data přenáší. U osmibitových počítačů se například používají dvě sběrnice, datová a adresová. Hledejte např. "DATA BUS", nebo "D0-D7", popřípadě "D[0..7]". Pro adresovou sběrnici se používá "Address bus", nebo A0-A15, A[0..15]. Ve schématu (prosímvás, **čte se to [schéma], nikoli [šéma]!**) je tak jedna čára, ale konvencí je dáno, že zastupuje víc vodičů.

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/bus.png" rel="lightbox"><img loading="lazy" class="aligncenter size-large wp-image-254" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/bus-1024x485.png" alt="" width="640" height="303" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/bus-1024x485.png 1024w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/bus-300x142.png 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/bus-768x364.png 768w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/bus.png 1261w" sizes="(max-width: 640px) 100vw, 640px" /></a>

Na tomto obrázku je modře označená sběrnice, která obsahuje 13 vodičů. Pojmenované jsou VALVE0 až VALVE11 a EN. Vlevo je symbol nějakého konektoru, je tam nazančeno, jak jsou vodiče připojeny (EN na pin 9, VALVE11 na pin 10 atd.), ale už není každý jeden připojen k obvodům. Jsou spojeny do "sběrnice". K jednotlivým obvodům vpravo jsou přivedeny touto sběrnicí. Ke každému je připojen vstup EN a čtyři různé vodiče VALVE. V reálu budou samozřejmě natažené fyzické propojky (vodič či vodivá cesta) pro každý signál zvlášť, ale schéma je díky sběrnici čitelné. Je dobré naučit se číst taková schémata.

Mimochodem, schémata... Už jsme nakousli, pojďme to ale pro jistotu probrat pořádně!

## Schémata

Možná jste si všimli, že v realitě vypadají elektronické obvody jinak, než na papíře. Třeba takto:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/ram1.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-255" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/ram1.jpg" alt="" width="716" height="646" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/ram1.jpg 716w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/ram1-300x271.jpg 300w" sizes="(max-width: 716px) 100vw, 716px" /></a>

> Pro zajímavost - jde o desku paměti RAM z počítače SAPI-1. Tato deska měla kapacitu 48 kilobyte. Samotné obvody paměti vidíte vpravo - jsou to ty tři sloupce obvodů. Každý z těchto obvodů měl kapacitu 16k x 1 bit. Proto jsou poskládány do osmic (svisle), a všechny mají propojené odpovídající adresové vodiče (to je to vedení shora dolů). Datové vedení není vidět, to je z druhé strany desky. Osm obvodů pod sebou dalo blok 16 kB paměti, tři tyto bloky tvořily dohromady 48 kB. Levá polovina desky obsahuje obvody, které se staraly o správné dekódování adresy a výběr vhodného bloku. Pokud teď nechápete, jak to je celé navržené, nebojte se, na konci kurzu to bude pro vás brnkačka 🙂 PS: Ty silné stříbrné čáry, které vedou ke všem obvodům, jsou napájení - zem a +5V.

To, co vidíte, je finální výrobek. Ale před tím, než je vyroben, musí někdo nakreslit, jak bude vypadat, a zároveň pak jiní lidé musí vědět, jak je to celé zapojené, aby mohli případně najít chybu nebo zjistit, jak desku připojit. Proto je potřeba mít nákres v nějaké rozumné podobě. Samozřejmě, můžeme použít něco jako nástroj Fritzing a udělat si jednoduchý nákres, který má tu výhodu, že vypadá skoro jako reálný obvod.

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/breadboardexample1.gif" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-256" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/breadboardexample1.gif" alt="" width="501" height="380" /></a>

Je to o něco málo (no, o hodně málo) přehlednější než reálný obvod, který bude vypadat třeba takhle:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/Logic1NAND10.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-257" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/Logic1NAND10.jpg" alt="" width="318" height="245" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/Logic1NAND10.jpg 318w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/Logic1NAND10-300x231.jpg 300w" sizes="(max-width: 318px) 100vw, 318px" /></a>

Jenže zásadní nevýhoda takového zobrazení je, že je za A nepřehledné, a za B neříká nic o tom, jak zařízení funguje. Jsou tam nějaké černé obdélníky s nožičkami, všechny vypadají plusmínus stejně, a mezi nimi jsou natažené dráty. Co vlastně spojují a jak, to vidět není. Jen hodně cvičený člověk dokáže kouknout a říct: _Aha, tady jsou spojená hradla tak a tak a dohromady to je sčítačka! _V praxi je to ale velmi nepohodlné.

Proto se v elektronice používají schematické výkresy. Už jsme se s nimi setkali, tak jen pár informací.

Ve schématech se soustředíme na zapojení a funkci. Namísto reálného zapojení, kde je rozmístění součástek dané designovými rozhodnutími, je ve schématu obvod nakreslen tak, aby byl snadno pochopitelný. Schémata se zpravidla kreslí tak, že signál postupuje zleva doprava a shora dolů, podobně jako psaný text.

Spojení se značí obyčejnou čárou. Pokud se dvě čáry kříží, neznamená to, že jsou spolu spojené. Spojené jsou pouze v případě, že je v místě křížení malá tečka:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/simple.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-258" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/simple.png" alt="" width="315" height="189" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/simple.png 315w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/simple-300x180.png 300w" sizes="(max-width: 315px) 100vw, 315px" /></a>

Vlevo máme tři (vstupní) signály, A, B a C. Jak poznám, že jsou vstupní? No, napoví mi hned několik věcí. Zaprvé: jsou připojené na vstupy těch dvou hradel. Zadruhé: jsou označeny písmeny ze začátku abecedy. No a zatřetí - jsou nakreslené vlevo. U takto jednoduchého schématu není důvod konvenci porušit.

Vstup A je připojen do hradel IC1A a IC1B. Jde o hradla NAND, [s nimiž jsme se už seznámili](https://iotta.cz/zaklady-cislicove-techniky-i/). Vstup B je připojen k hradlu IC1A, vstup C k hradlu IC1B. Všimněte si, že hradla mají u vývodů malá čísla - jsou to čísla vývodů reálného obvodu 74AC00N. Pojďme se podívat na jeho vnitřní uspořádání:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/7400.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-259" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/7400.png" alt="" width="500" height="345" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/7400.png 500w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/7400-300x207.png 300w" sizes="(max-width: 500px) 100vw, 500px" /></a>

Vidíte, že v jednom pouzdru se 14 vývody jsou čtyři hradla NAND. První hradlo má vstupy připojené na vývodech 1 a 2, výstup na vývodu 3. Druhé hradlo má vstupy 4 a 5, výstup 6, atd. Protože jsou v jednom integrovaném obvodu (IC, integrated circuit) čtyři hradla, značíme je A, B, C a D - odtud označení IC1A, IC1B: prostě integrovaný obvod IC1, typu 74AC00N, a vnitřní hradlo A, B, ...

Vývody GND a Vcc slouží k napájení. GND je zem (Ground), Vcc je napájecí napětí - u řady 74 je to +5V. Mezi zem a Vcc se doporučuje zapojit odrušovací kondenzátor 100nF co nejblíž pouzdru - pro naše experimenty to zatím není nutné, ale v reálném zapojení právě tyto kondenzátory rozhodnou o tom, zda to celé bude fungovat, nebo zda to bude mrtvé...

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/74AC.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-260" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/74AC.png" alt="" width="229" height="112" /></a>

Všimněte si, že u integrovaných obvodů (je to vidět i na té fotografii nahoře) je v jednom místě klíč - drobný výlisek, který říká, jak je pouzdro orientované. Je taková konvence, že obvod je v "normální" pozici tak, že je vývody dolů, potisk je ve správné pozici (vodorovně a čitelný) a výlisek je nalevo. Pokud je obvod takto orientován, je vývod číslo 1 **vlevo dole**. Vývod 2 je napravo od něj, vývod 3 zase napravo od vývodu 2, a tak dále, až do konce pouzdra (vývod 7), a pak se pokračuje jakoby proti směru hodinových ručiček: vývod 8 je vpravo nahoře, vývod 9 vlevo od něj, až konečně vlevo nahoře, opět u výlisku, je vývod 14. Toto pravidlo platí pro integrované obvody v těchto pouzdrech obecně, bez ohledu na to, kolik vývodů mají. I když budete pracovat např. s obvody, které mají 28 vývodů, nebo 40 vývodů, vždy bude platit, že jsou označeny tam, kde je vývod 1, a při pohledu shora jsou číslovány proti směru hodinových ručiček.

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/arduino-uno.jpg" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-261" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/arduino-uno.jpg" alt="" width="500" height="375" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/arduino-uno.jpg 500w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/arduino-uno-300x225.jpg 300w" sizes="(max-width: 500px) 100vw, 500px" /></a>

Všimněte si, že i složité obvody, v případě Arduina je to jednočipový počítač ATmega328, mají označený vývod 1 - zde to je vpravo nahoře, to znamená, že obvod je na fotografii "vzhůru nohama".

Schematické značky hradel jsou dvojího typu. Jedny standardizované (IEC), které vypadají jako obdélníčky, v nichž je vepsaná funkce, druhé "tradiční", které jsou oblé. Více napoví následující obrázek:

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/03/Logic-gate-index.png" rel="lightbox"><img loading="lazy" class="aligncenter size-large wp-image-266" src="http://iotta.cz/wp-content/uploads/sites/17/2017/03/Logic-gate-index-1024x346.png" alt="" width="640" height="216" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/03/Logic-gate-index-1024x346.png 1024w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/Logic-gate-index-300x101.png 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/03/Logic-gate-index-768x259.png 768w" sizes="(max-width: 640px) 100vw, 640px" /></a>

Nejčastěji se setkáte se značkami IEC a US. Společné rysy jsou: vlevo vstupy, vpravo výstupy. Kroužek u vstupu / výstupu znamená, že výstup je negovaný (srovnejte např. dvojice NAND/AND, NOR/OR).

Když už jsme nakousli pouzdra a značení, pojďme si něco říct ještě k tomuto tématu...

## Číslicové obvody v reálu

Číslicovou techniku si budeme ukazovat na pramáti číslicových integrovaných obvodů, na řadě TTL 74xx. TTL je technologie, jakou jsou na křemíkové desce z tranzistorů poskládána hradla, a 74xx je označení celé rodiny obvodů, které začal používat výrobce Texas Instruments. První dvojčíslí bylo vždy 74 a pak následoval číselný kód konkrétního typu. Například 7400 byla čtveřice dvouvstupových hradel NAND, 7404 šestice invertorů, 7432 čtveřice hradel OR, 7410 trojice třívstupových hradel NAND, ...

Označení začínalo písmeny "SN". Další výrobci vyráběli vlastní obvody z této logické řady, ale značili je jinými kódy. Nejčastěji zachovávali původní značení, jen místo písmen SN dávali svoje (například Tesla používala písmena MH), ale někteří výrobci změnili označení naprosto a zgruntu (v DDR se vyráběl obvod 7400 pod označením D100, v Maďarsku 7400APC, v Sovětském Svazu to bylo K155LA3) - pro zájemce doporučuju [heslo na Wikipedii](https://en.wikipedia.org/wiki/7400_series).

Průmyslovým standardem pro tuto řadu bylo plastové pouzdro se čtrnácti vývody (složitější obvody měly vývodů víc), rozmístěnými do dvou řad po sedmi, jak jsme si naznačili výš. Proto se tomuto pouzdru říká DIL (Dual In-Line), popřípadě DIP (Dual In-line Package). Setkat se lze i s označením PDIP (Plastic DIP) a CDIP (Ceramic DIP) Rozestupy mezi vývody jsou 2,54 mm (protože to je jedna desetina palce - nezapomeňme, že tyto obvody vznikly v USA, kde se používají palce), řady jsou od sebe vzdáleny 7,62 mm - tedy trojnásobek rozestupu mezi vývody (0.3")

Později se začaly používat i další typy pouzder, především s nástupem povrchové montáže (surface mounted devices, SMD), které mají nejčastěji čtvercový tvar s krátkými vývody po stranách, případně zespod, a někdy jsou vývody pojaté formou malých cínových kuliček (BGA, Ball Grid Array). Pro naše účely jsou použitelné především obvody v pouzdrech DIL - s nimi lze pracovat snadno v nepájivém kontaktním poli. Pokud začneme pracovat se složitějšími obvody, zjistíme, že se v těchto pouzdrech už ani nevyrábí, a musíme sáhnout k SMD pouzdrům. Takové obvody ale už musíme pájet, a ruční pájení vyžaduje dobré vybavení a cvik.

Každý obvod má své označení, které znalému prozradí spoustu informací. Například:

**DM74ALS00N**

DM označuje, jak jsme si řekli, výrobce - zde je to National Semiconductor (NatSemi). 74 napovídá, že půjde o obvod z řady 74xx - konkrétně 7400, tedy čtveřice dvouvstupových hradel NAND. Písmena ALS označují konkrétní technologii - zde Advanced Low-power Schottky, no a konečně poslední písmeno N se nejčastěji používá pro označení typu pouzdra, konkrétně DIL.

Obvod 7400 se vyrábí nejrůznějšími technologiemi. Ta stará, původní TTL, nemá žádné speciální označení. Po ní přišla řada "Schottky" (74S00) - hradla se přepínala rychleji, takže byla vhodná pro vyšší frekvence signálů, ale nevýhodou byl vyšší odběr. Tam, kde na odběru záleželo, používala se technologie Low Power, která měla menší proudy, ale zase byla pomalejší (74L00). Kombinací těchto dvou technologií vznikla řada LS - Low power Schottky, jejím dalším vylepšením pak ALS - Advanced LS. Jsou rozumným kompromisem mezi rychlostí, spotřebou a ziskem.

> Zisk u hradla znamená, kolika vstupy lze zatížit jeden výstup. U standardní řady 7400 lze na jeden výstup připojit až deset vstupů, u obvodů ALS to je až 20.

Mimochodem, velmi se vám bude hodit schopnost číst v **datasheetu**. Datasheet, též "katalogový list", popisuje všechny možné aspekty dané součástky. Najdete v něm nejen to, jak vypadá, jak má zapojené vývody a jak uvnitř funguje, ale i nejrůznější další informace, např. spotřebu, pracovní napětí apod. Použijte Google, hledejte např. "74ALS00 datasheet".

## Kvíz

[WpProQuiz 4]



## Reálné pokusy pro ještě více radosti

Vlastně byste mohli už začít se samostatnými pokusy. Já se snažím dát teď dohromady patřičné množství součástek a dalšího vybavení, abych vám mohl nabídnout vhodné startovní sady, ale než je budu moci nabízet, dám aspoň těm největším nedočkavcům pár tipů, co a kde koupit. (Ti trpělivější si mohou počkat, až mi přijdou komponenty.)

### Co budete potřebovat?  
_(alias BASIC KIT)_

Můžete to celé nakoupit třeba v Alze, ale otázka je, jestli chcete dát 160 Kč za sadu 70 drátů, když totéž koupíte z Číny za jeden dolar. V Alze ale neseženete další součástky. Pro nákup součástek v ČR můžete použít [GM](https://www.gme.cz/), [GES](https://www.ges.cz) nebo [TME](http://www.tme.eu/cz/).

  * Nepájivé kontaktní pole ([GME](https://www.gme.cz/nepajive-kontaktni-pole-zy-60), [GES](http://www.ges.cz/cz/sd-102-GES07713718.html))
  * Sadu propojovacích vodičů
  * Vhodný napájecí adaptér na 5V (např. z USB)
  * Integrované obvody - začněte tím, který si popisujeme teď, tedy 7400. Další obvody, které budeme používat, jsou 7404, _7408, 7416, 7432,_ 7474, _7475,_ 7493, 74138 - ty kurzívou nejsou nezbytné. Můžete použít libovolnou technologii, podle toho, jaké nejsnáz seženete. Pravděpodobně půjde o obvody 74ALS, popřípadě HCT (Hihg-speed CMOS TTL).
  * LED pro zobrazování výstupů. Použijte libovolné, které seženete. K nim nezapomeňte koupit omezovací rezistory, jinak je přepálíte. Rezistor by měl mít hodnotu 330 (ohmů, 330R)
  * Tlačítka, přepínače, spínače...
  * Trochu rezistorů základních hodnot (pro začátek 220, 330, 1K, 2K2, 3K3, 10K)
  * Několik kondenzátorů s kapacitou 100n, několik elektrolytických s kapacitou např. 4.7uF nebo 10uF.

Já osobně volím pomalejší, ale výrazně levnější cestu nákupu z Číny. Příkladem může být nepájivé kontaktní pole. GME ho nabízí za 62 Kč, GES za 199 Kč, na eBay jej koupíte za cca 30 Kč.



Propojovací kabely vybírejte podle klíčových slov "Male jumper wire":



Ke kontaktnímu poli si kupte takovýto hezký adaptér - napájí se to přes USB nebo z normálního zdroje:



LEDky vezměte různobarevné:



Rezistory si vezměte ty nejobyčejnější. Můžete buď vybrat hodnoty, nebo prostě koupíte balíček, co se jmenuje "resistor assortment set", kde najdete cca 400 kusů rezistorů nejrůznějších hodnot, a to za pár desetikorun:



Kondenzátory 100n slouží především k filtraci rušení, a taky trošku k časování. Budeme jich potřebovat tak pět, ale když jich je v balíčku sto za dvacku, nekupte to...





Integrované obvody nakoupíte i v českých e-shopech za korunové částky (74HCT00 za 9 Kč). Z Číny to lze taky, ale za desetikorunové částky koupíte balíčky třeba po 20, 50 kusech, a to se asi nevyplatí.

Bonusové doporučení: Bude se vám hodit multimetr. Multimetry umí měřit napětí, proud i odpor, takže s nimi snadno zjistíte, jestli je někdě zapojené něco, co zapojené být nemá, nebo naopak. Stojí řádově 100-200 Kč. I tady lze nakoupit z Číny:



_Pokusím se co nejrychleji poskládat součástky, abych mohl vážným zájemcům nabídnout kompletní sety k experimentům._