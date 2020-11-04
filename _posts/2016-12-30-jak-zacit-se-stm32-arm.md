---
id: 162
title: Jak začít se STM32 ARM?
date: 2016-12-30T13:24:00+00:00
author: Martin Maly
layout: post
guid: https://iotta.cz/?p=162
permalink: /jak-zacit-se-stm32-arm/
image: http://iotta.cz/wp-content/uploads/sites/17/2016/09/miniSTM32.png
categories:
  - Hardware
  - STM32
---
Došel mi milý a podnětný dotaz na Facebooku, a říkám si, že by odpověď mohla pomoci i ostatním.

> Zdravím, našiel som Vaše články na IoTta.cz Nechcem veľmi otravovať, len by som potreboval trosku poradiť, aby som sa vedel správne rozhodnúť. Podľa toho čo čítam, asi dnes najpopulárnejšie mikroprocesory sú ARM, je to tak? Nasledne asi aj cenovo sú ok STM32. Moja otázka je, v čom sa to programuje, ktorý prog. jazyk by som mal ovládať pre programovanie STM32 ARM? Je to čisté C alebo C++. Ďalšia otázka je, že už keď ísť do niečoho nového, tak ci radšej hneď Freertos? Freertos sa programuje v com? Vdaka za akúkoľvek informaciu.

ARM jsou prověřené a podporované procesory, a ty od ST jsou solidní. Programují se v čemkoli, od assembleru přes C/C++ po vyšší jazyky (MicroPython, JavaScript) či obskurity (Lua, FORTH). Ale asi nejčastější je to C/C++. FreeRTOS je vhodný pro aplikace, které mají běžet v reálném čase. Někdy to může být overkill, a asi bych do toho nešel ve chvíli, kdy bych se chtěl s ARM seznámit, to bych šel nejprve cestou &#8222;holého&#8220; C. Můžu doporučit [Atollic IDE (True Studio)](http://atollic.com/truestudio/). A vždycky se dá použít platforma Arduino, existuje podpora pro procesory STM32.

> Super, vdaka za tieto informácie. S Arduinom mam skúsenosti, aj mi to príde také prehľadnejšie, jednoduchsie. Cize cez Arduino IDE môžem kľudne programovat STM32 bez toho aby som stratil nejaké funkcionality?

<div class="_3hi clearfix">
  <div class="_38 direction_ltr">
    <p>
      Koukněte na <a class="_553k" href="https://bluepill.cz/programovani/arduino-ide/" target="_blank" rel="nofollow">https://bluepill.cz/programovani/arduino-ide/</a> Samozřejmě není to kompatibilita na 100 %, on ten ARM se od AVR v něčem liší, ale možnost to je. Arduino má AVR, STM32 je ARM, ty architektury se od sebe v něčem liší, takže ta kompatibilita není na 100 % &#8211; pokud třeba nějaké knihovna pro Arduino používá přímo PORTB, PORTD, tak nebude na ARM fungovat.
    </p>
    
    <div class="_3hi clearfix">
      <div class="_38 direction_ltr">
        <blockquote>
          <p>
            Ale napr stm32 discovery môžem programovat presne tak isto ako Arduino cez Arduino IDE? A zaujala ma aj informácia, že sa to da programovat v Javascripte. Klasický Javascript a potom mi to kompiler prepise tak aby to išlo na STM32?
          </p>
        </blockquote>
        
        <div class="_3hi clearfix">
          <div class="_38 direction_ltr">
            <p>
              Nevím jak Discovery, ten kit neznám, ale mělo by to jít. S tím JavaScriptem se to má tak, že v STM32 je celý JavaScript Engine, a vy se k tomu STM32 připojíte normálně terminálem a píšete JS program, který se pak v tom procesoru vykoná &#8211; tedy žádný kompiler, ale přímo interpret JS v STM. Tady je informace k tomu JavaScriptu na desce Discovery: <a class="_553k" href="https://www.espruino.com/Other+Boards" target="_blank" rel="nofollow">https://www.espruino.com/Other+Boards</a>
            </p>
            
            <blockquote>
              <p>
                Celkovo sa chcem učiť nový programovací jazyk a Javascript sa da využiť aj inde. Takže možno by som isiel do toho, len asi to bude mať aj háčik, ze na nete nebude pre Javascript podpora pre rôzne moduly. Možno sa nevyjadrujem správne, ale chcel by som to pochopiť. Cize napr. Zoberiem dosku stm32 a k tomu zapojím nejaký modul napr. DHT22. Celkovo pre ovládanie celej dosky a zber info zo senzora môžem použiť čisto Javascript? To by bolo úplne super. Aj keď ako som pisal, asi skôr nájdem zdrojak v C ako v Javascripte
              </p>
            </blockquote>
            
            <div class="_3hi clearfix">
              <div class="_38 direction_ltr">
                <p>
                  Viz <a class="_553k" href="https://www.espruino.com/Modules" target="_blank" rel="nofollow">https://www.espruino.com/Modules</a> Zrovna DHT22 tam je.
                </p>
                
                <blockquote>
                  <p>
                    To som dal iba ako príklad, islo by napr. aj o LoRa moduly atd.
                  </p>
                </blockquote>
                
                <div class="_3hi clearfix">
                  <div class="_38 direction_ltr">
                    <p>
                      No, pokud je úplně nejhůř, tak se ty moduly dají dopsat &#8211; buď v C, nebo v JavaScriptu
                    </p>
                    
                    <blockquote>
                      <p>
                        Napr. aj takáto doska by sa dala ovládať cez Javascript? <a class="_553k" href="http://www.miromico.ch/fmlr-lorawan-sensors.html" target="_blank" rel="nofollow">http://www.miromico.ch/fmlr-lorawan-sensors.html</a>
                      </p>
                    </blockquote>
                    
                    <div class="_3hi clearfix">
                      <div class="_38 direction_ltr">
                        <p>
                          Proč ne? LoRaWAN mívají jako rozhraní normální sériový port, tak by to mělo jít bez problémů.
                        </p>
                      </div>
                    </div>
                    
                    <p>
                      &nbsp;
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>