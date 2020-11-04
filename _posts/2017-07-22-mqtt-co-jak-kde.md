---
id: 343
title: 'MQTT &#8211; co, jak, kde?'
date: 2017-07-22T13:39:07+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=343
permalink: /mqtt-co-jak-kde/
image: http://iotta.cz/wp-content/uploads/sites/17/2017/07/mqttorg.png
categories:
  - DIY
tags:
  - mqtt
---
_(Článek [vyšel na serveru Root.cz](https://www.root.cz/clanky/protokol-mqtt-komunikacni-standard-pro-iot/), zde je zkrácená obecnější verze)_

<p class="western">
  „Aha,“ povídal onehdá jeden zájemce o IoT, „takže měřidlo teploty pošle signál Otevři okno… nebo vypínač pošle signál žárovce: Rozsviť se!“ – „Ano, to by šlo,“ povídám mu, „ale lepší je udělat to trošku jinak. Lepší je, když okno říká: dejte mi vědět, když se změní teplota; žárovka říká: když vypínač pošle zprávu, pošlete ji mně! Je lepší, když se vyhodnocení dělá u těch zařízení, které provádějí nějaké akce, aby bylo jasné, na co se reaguje a aby ty informace byly na jednom místě.“
</p>

<!--more-->

<p class="western">
  V IoT systémech by snímače neměly určovat, co kdo má udělat. Ony snímají a posílají ty zprávy do centrálního místa. O víc se nestarají. Ti, co mají zájem o jejich zprávy, se přihlásí k jejich odběru, a nějak na ně reagují. V ideální podobě se přidává i další vrstva abstrakce, kde žárovka reaguje na pokyn „vypnout / zapnout“, vypínač informuje o tom, že byl stisknut, a někdo třetí, nějaký softwarový agent, čte zprávy od vypínače a nějak na ně reaguje. Pak je mnohem snazší doplnit různé funkce, jako například zhasnutí se zpožděním nebo přemapování fyzických zařízení tak, aby logicky fungovaly lépe.
</p>

<p class="western">
  Komunikace nodů se světem po internetových protokolech vypadala zpočátku jako poměrně jednoduchá, a vypadalo to, alespoň v hlavách nepříliš zkušených, jako hotová věc: „použijeme HTTP, všichni ho znají, všichni ho používají, je to celkem jednoduchý protokol, a bude. Na druhé straně může být REST API, jaký tedy problém?“ Postupem času se ukázalo, že HTTP má pro využití v internetu věcí mnoho nevýhod. Je hodně upovídaný. Jeho implementace není triviální (ale lze z něj používat nějaký minimální subset). Je založen na principu „klient táhne“, takže pro komunikaci typu „server tlačí“ je potřeba řešit různé hacky typu long polling, heartbeat, použít HTTP/2 apod. Nemá vyřešené různé úrovně QoS a pro dvě zařízení s protokolem HTTP je vždy nutné mít nějakou serverovou aplikaci, která jim umožní vzájemnou komunikaci, protože samotný HTTP server nedokáže sám data předávat mezi klienty.
</p>

<p class="western">
  <em>Z HTTP vznikla jeho binární podoba CoAP – ta měla v první řadě zjednodušit implementaci v MCU, snížit množství dat, protože textové hlavičky nahradila binárními příznaky, místo TCP používat UDP a řešit QoS. Jeho výhodou je snadné předávání zpráv mezi CoAP a HTTP. Pro komunikaci se serverem využívá „observer pattern“ a princip „odpověď po dlouhé době“. S volitelnými doplňky ale CoAP opět nabobtnal, čímž hlavní výhoda zmizela.</em>
</p>

<p class="western">
  MQTT (dříve: <em>Message Queuing Telemetry Transport</em>, dnes MQ Telemetry Transport) je jednoduchý a nenáročný protokol pro předávání zpráv mezi klienty prostřednictvím centrálního bodu – brokeru. Díky této nenáročnosti a jednoduchosti je snadno implementovatelný i do zařízení s „malými“ procesory a poměrně rychle se rozšířil. Navržen byl v IBM, dnes za ním stojí Eclipse foundation a před nedávnem proběhla standardizace OASIS.
</p>

<p class="western">
  U protokolu MQTT probíhá přenos pomocí TCP a používá návrhový vzor publisher – subscriber. Tedy existuje jeden centrální bod (MQTT broker), který se stará o výměnu zpráv. Zprávy jsou tříděny do tzv. témat (topic) a zařízení buď publikuje v daném tématu (publish), to znamená, že posílá data brokeru, který je ukládá a distribuuje dalším zařízením, nebo je přihlášeno k odběru tématu či témat (subscribe), a broker pak všechny zprávy s daným tématem posílá do zařízení. Jedno zařízení samozřejmě může najednou být v některých tématech publisher, v jiných subscriber.
</p>

<p class="western">
  Samotný obsah zpráv není nijak daný ani vyžadovaný, MQTT je „payload agnostic“. Obsah zprávy jsou prostě nějaká binární data, která jsou přenesena. Nejčastěji se používá JSON, BSON, textové zprávy, ale může to být opravdu cokoli. Velikost zprávy je v aktuální verzi protokolu omezena, je to necelých 256 MB, ale naprostá většina zpráv je mnohem menší.
</p>

<p class="western">
  MQTT minimalizuje množství balastních dat, takže přidává jen minimum servisních dat. Zavádí tři úrovně QoS (Quality of Service) – tedy potvrzování zpráv, kde ta nejnižší znamená, že zpráva je odeslána bez potvrzení a není zaručeno její doručení (at-most-once), prostřední úroveň říká, že zpráva je doručena alespoň jednou (at-least-once), a nejvyšší úroveň QoS 2 znamená, že každá zpráva je doručena právě jednou. Klient však nemusí podporovat všechny tři úrovně QoS.
</p>

<a href="http://iotta.cz/wp-content/uploads/sites/17/2017/07/cz7F6.png" rel="lightbox"><img loading="lazy" class="aligncenter size-full wp-image-344" src="http://iotta.cz/wp-content/uploads/sites/17/2017/07/cz7F6.png" alt="" width="829" height="372" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/07/cz7F6.png 829w, https://iotta.cz/wp-content/uploads/sites/17/2017/07/cz7F6-300x135.png 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/07/cz7F6-768x345.png 768w" sizes="(max-width: 829px) 100vw, 829px" /></a>

## MQTT podrobněji

<p class="western">
  Zprávy v MQTT patří do určitých témat (topic). Každá zpráva patří právě do jednoho tématu. Témata jsou hierarchická, oddělená lomítky, například hypotetické „světlo číslo 26 na stropě v místnosti 103, první patro, budova 1“ může mít topic „building-1/floor-1/room-103/ceiling/light-26“. Nebo třeba „dům/ložnice/světlo“ – témata jsou v MQTT řetězce v UTF-8, takže pojmenování s diakritikou není problém.
</p>

<p class="western">
  Hierarchie není pevně dána, záleží na aplikaci a na tom, jak si ji sami navrhnete. Jak správně navrhnout hierarchii může být netriviální úloha, ne vždy je nejsprávnější struktura ta „přirozená“. Důležité je už v rámci těchto úvah rozmyslet datový model a rozhraní – tedy vhodné uspořádání tak, aby bylo možné přihlásit takové topics, které logicky patří k sobě.
</p>

<p class="western">
  Publisher, tedy ten, kdo zprávu produkuje, zvolí topic a pošle ho spolu se zprávou. Nemusí topic nijak zakládat ani kontrolovat, broker funguje tak, že jakmile přijme zprávu pro topic, který ještě nemá, tak jej založí. Je ale zásadní věc, aby si publisher s odběratelem předem dohodli téma, jaké budou používat. Většinou to bude dáno právě datovým modelem
</p>

<p class="western">
  Zařízení se přihlašují k odběru zpráv úplně stejně – tedy pošlou brokeru speciální zprávu „subscribe“ s názvem topicu, který chtějí odebírat. Zde mohou využít zástupné znaky # a +. Znak „+“ nahrazuje jednu úroveň, např. „building1/floor1/+/door“ adresuje dveře ve všech místnostech na prvním patře budovy 1 – takže building1/floor1/room101/door, building1/floor1/room102/door, building1/floor1/toilettes/door apod. Zástupný znak # pak nahrazuje jednu či více úrovní a musí být vždy jako poslední. Odběr tématu „building1/floor1/#“ znamená, že budou chodit zprávy se všemi tématy, která se týkají prvního patra budovy 1.
</p>

<p class="western">
  Ke jmenné konvenci ještě jedna poznámka: Pokud jméno tématu začíná znakem „$“, jedná se o speciální téma. Taková témata se používají především pro zprávy, které potřebuje poslat sám broker. Nejužívanější způsob jsou témata, která začínají $SYS/ – nejsou zatím pevně specifikována a seznam doporučených <a href="https://github.com/mqtt/mqtt.github.io/wiki/SYS-Topics">naleznete na MQTT Wiki</a>.
</p>

## Výměna zpráv

<p class="western">
  Na počátku navazuje klient – (zařízení, node) spojení s brokerem pomocí TCP. Používá se nejčastěji port 1883, pro spojení s TLS pak 8883. Další používaná možnost je připojení přes WebSockets (ws / wss), nejčastěji na portech 8080 / 8081 (či přes reverzní proxy na portech 80 a 443), ale samozřejmě je možné nastavit si komunikaci jakkoli.
</p>

<p class="western">
  Po navázání spojení posílá zařízení zprávu CONNECT, nejčastěji s příznakem clean session, který zajistí, že se začíná s „čistým stolem“, žádná témata nejsou přihlášena k odběru. Připojení může probíhat i s nějakým základním ověřením jménem a heslem. Broker odpoví zprávou CONNACK, čímž potvrdí připojení.
</p>

<p class="western">
  Nyní může následovat jedna či více zpráv SUBSCRIBE s názvy témat, které chce zařízení odebírat. Podrobnosti viz výše. Broker potvrzuje pomocí SUBACK. Samozřejmě kdykoli po připojení a potvrzení (CONNACK) může zařízení přihlásit odběr dalších témat kdykoli.
</p>

<p class="western">
  Od okamžiku připojení a potvrzení (CONNACK) je vše nastavené a jak zařízení, tak broker mohou posílat zprávy pomocí PUBLISH.
</p>

<p class="western">
  Zařízení se může odhlásit od odběru určitého tématu pomocí zprávy UNSUBSCRIBE (broker potvrdí provedení zprávou UNSUBACK), a při ukončení práce posílá zprávu DISCONNECT.
</p>

<p class="western">
  V případě, že už zařízení bylo dřív připojeno, můžete využít připojení bez clean session, to znamená, že zůstanou zachovaná dříve přihlášená témata.
</p>

<p class="western">
  MQTT zajišťuje i detekci toho, že je zařízení stále živé. Pokud zařízení v určitém čase nepošle zprávu, považuje ho za násilně odpojené a posílá takzvanou závěť (will, testament), ke které se ještě dostaneme. Aby zařízení nespadlo do tohoto stavu, tak by mělo v případě, že nemá žádnou zprávu k poslání, posílat PINGREQ. Broker odpovídá zprávou PINGACK.
</p>

<p class="western">
  V případě ztráty spojení může broker poslat do určitého tématu zprávu „last will“, tedy vlastně závěť, „poslední vůli“, což je zpráva, kterou zařízení může poslat v rámci CONNECT zprávy při připojování k brokeru. Tato zpráva není povinná.
</p>

## QoS, retain

<p class="western">
  Tématem Quality of Service jsme se už trošku zabývali. Řekli jsme si, že jsou tři úrovně, 0, 1 a 2, které se liší podle toho, jak moc jsou potvrzené a jak je zajištěno dodání.
</p>

<p class="western">
  V úrovni 0 (at most once, též fire and forget) pošle publisher zprávu PUBLISH brokeru a dál se o nic nestará. Broker ji stejným způsobem pošle subscriberům daného tématu.
</p>

<p class="western">
  Na úrovni 1 (at least once) posílá publisher brokeru zprávu PUBLISH a čeká. Broker zprávu přijme a pošle ji odběratelům (opět PUBLISH). Odeslání proběhne buď s QoS 1, nebo s QoS 0, pokud zařízení QoS1 neumí. Ale obecně platí, že se odesílá s takovým QoS, s jakým byla přijata. V tomto případě pošle s QoS1 (PUBLISH) a čeká na potvrzení od příjemce. Jakmile odběratelé potvrdí přijetí zprávou PUBACK, broker zprávu odstraní a pošle PUBACK publisherovi zpět. Publisher ví, že zpráva prošla brokerem, a může ji zahodit. Broker může poslat PUBACK, aniž by měl potvrzení od všech příjemců. Přesné chování závisí na implementaci, většina to tak dělá a MQTT umožňuje oba scénáře, tj. čekat i nečekat.
</p>

<p class="western">
  Na úrovni 2 (exactly once) posílá publisher brokeru zprávu PUBLISH. Ten ji, stejně jako v předchozím případě, přijme, posílá ji odběratelům, a publisherovi vrátí zprávu PUBREC (tedy potvrzení přijetí). Publisher odpoví zprávou PUBREL, broker zprávu smaže a potvrdí zprávou PUBCOMP. Tím je výměna uzavřena.
</p>

<p class="western">
  Pro QoS platí, že broker odesílá zprávu na té úrovni, na které zpráva přišla, s možností snížit úroveň, pokud klient umí pouze nižší.
</p>

<div class="rs-img-center">
  <a href="http://iotta.cz/wp-content/uploads/sites/17/2017/07/mqttqos.png" rel="lightbox"><img loading="lazy" class="aligncenter wp-image-346 size-medium" src="http://iotta.cz/wp-content/uploads/sites/17/2017/07/mqttqos-300x265.png" alt="" width="300" height="265" srcset="https://iotta.cz/wp-content/uploads/sites/17/2017/07/mqttqos-300x265.png 300w, https://iotta.cz/wp-content/uploads/sites/17/2017/07/mqttqos-768x678.png 768w, https://iotta.cz/wp-content/uploads/sites/17/2017/07/mqttqos.png 782w" sizes="(max-width: 300px) 100vw, 300px" /></a>
</div>

<p class="western">
  Kromě QoS se u zprávy nastavuje i retain flag, tj. příznak, který říká, že broker nemá zprávu zahazovat po rozeslání, ale uložit a poslat novým odběratelům daného topicu. Posílá se vždy poslední uložená zpráva s příznakem retain.
</p>

<p class="western">
  Důležité je vědět, že publisher může zprávu poslat s jakýmkoli QoS brokeru. Broker ji se stejným QoS posílá odběratelům. Zde platí stejný postup, tedy např. že u QoS 2 ji pošle odběrateli, ten potvrdí příjem (PUBREC), broker potvrdí, že dostal potvrzení (PUBREL) a odběratel komunikaci uzavře PUBCOMP. Odběratel si ale v rámci odběru (SUBSCRIBE) může říct, jaký maximální QoS chce dostávat. Broker mu pak došlé zprávy posílá tak, že nemají vyšší QoS než zadaný. Pokud tedy přijde zpráva s QoS 2, ale odběratel chce maximálně QoS 1, broker mu pošle tutéž zprávu s QoS 1. Což samozřejmě znamená, že tento odběratel může tuto zprávu dostat vícekrát (protože QoS 1 nezaručuje doručení „právě jednou“).
</p>

<p class="western">
  <h2>
    Příklad MQTT v praxi
  </h2>
  
  <p class="western">
    Dejme tomu, že budeme implementovat tu nejprofláklejší situaci vůbec, totiž rozsvěcení žárovky vypínačem. Pomiňme teď fakt, že v reálném světě by asi nikdo z nás nespoléhal u světel jen na sofistikovaný systém a požadovali bychom, aby byla možnost rozsvítit světlo i při výpadku spojení či řídicího počítače. To nechme stranou a zaměřme se pouze na možnosti, jak takovou věc zapojit.
  </p>
  
  <p class="western">
    První nápad bude ten nejprostší, který jsme si tu už představili: Vypínač bude publisher, bude publikovat v tématu room/switch (pro jednoduchost; ve skutečnosti bude asi název komplexnější), a inteligentní žárovka bude přihlášena k odběru zpráv s tématem room/switch a bude reagovat na zprávy.
  </p>
  
  <div class="rs-img-center">
  </div>
  
  <p class="western">
    Druhý přístup je komplexnější a vyžaduje určitou inteligenci. Žárovka totiž nemusí poslouchat právě jen vypínač; do cesty může vstoupit nějaká minimální inteligence, která vyhodnocuje zprávy od publisherů a na jejich základě posílá pokyny odběratelům.
  </p>
  
  <div class="rs-img-center">
  </div>
  
  <p class="western">
    S takovýmto uspořádáním je mnohem snazší celému systému dodat další úroveň abstrakce. V ní můžeme snadno celý systém v případě potřeby například logicky přeuspořádat, popřípadě můžeme přidat logiku, kterou samotná zařízení neimplementují – například schodišťový vypínač s časovačem, nebo „křížové“ vypínače, kdy ovládáme jedno světlo z více míst, nebo i další inteligentní chování.
  </p>
  
  <h2>
    Použití MQTT
  </h2>
  
  <p class="western">
    Na přednáškách říkám posluchačům, že se s MQTT už jistě setkali, a ani nevěděli, že ho používají – totiž Facebook Messenger používá právě tento protokol.
  </p>
  
  <p class="western">
    MQTT je velmi snadno použitelný – existuje mnoho implementací brokerů (asi nejznámější a nejpoužívanější je open-source MQTT broker <a href="http://mosquitto.org/">Mosquitto</a>) a ještě víc klientských knihoven pro nejrůznější jazyky (od Javy přes Python, JavaScript, Ruby, Go až po Erlang) a zařízení (Arduino, mbed, netduino, …) S MQTT se setkáte i u cloudových služeb (AWS IoT, Azure IoT), nebo naopak u různých systémů pro domácí automatizaci (Domoticz) či nástrojů pro smartfony. Bez přehánění lze říct, že MQTT je opravdu jeden z mála IoT standardů.
  </p>
  
  <h2>
    Odkazy
  </h2>
  
  <ul>
    <li>
      <a href="http://mqtt.org/">MQTT</a>
    </li>
    <li>
      <a href="http://www.hivemq.com/blog/seven-best-mqtt-client-tools">Seznam klientů pro MQTT</a>
    </li>
    <li>
      <a href="http://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices">Best practices pro MQTT</a>
    </li>
    <li>
      <a href="http://www.4makers.info/instalace-mosquitto-na-raspi-ze-zdrojovych-kodu/">Instalace MQTT brokeru</a> Mosquito na Raspberry Pi
    </li>
    <li>
      <a href="https://iotta.cz/?s=mqtt">Moje experimenty s ESP8266 a MQTT</a>
    </li>
  </ul>