# Definition of Done

_**Definition of Done**_ Definierar vilka moment som ska utföras innan en specifik uppgift kan betraktas som _klar_

**POLICY - Definition of Done (2015-12-11)**

* ### Peer review genomförd
    All kod ska återkopplas från annan utvecklare innan merge sker till master-branch.
* ### Acceptanstester uppfyllda
    Acceptanstester kan genomföras genom automatiska tester byggda på acceptanskriterier, eller genom testning av Produktägaren.
* ### Tester för all logik
    Komponenttester samt integrationstester ska finnas för samtlig logik i systemet.
* ### 100% Code Coverage (Rails)
    Samtliga kodrader ska köras under en testsvit för Rails applikationer.
* ### Dokumentation uppdaterad
    * Kod ska vara tydlig och dokumenterad
    * APIer ska vara dokumenterade
    * Vid behov ska manualer / applikationsinstruktioner hållas uppdaterade
* ### Deploy till testmiljö genomförd
    För att testa kodens deploybarhet, samt möjliggöra extern testning, ska all kod deployas till en testmiljö.