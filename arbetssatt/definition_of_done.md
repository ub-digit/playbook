# Definition of Done

_**Definition of Done**_ Definierar vilka moment som ska utföras innan en specifik uppgift kan betraktas som _klar_

**POLICY - Definition of Done (Förslag enl. 2015-12-11)**


* ### Kompletta tester finns (oplanerad)
    Komponenttester samt integrationstester ska finnas för samtlig logik i systemet.

* ### Dokumentation uppdaterad (oplanerad)
    * Kod ska vara tydlig och dokumenterad
    * APIer ska vara dokumenterade
    * Vid behov ska manualer / applikationsinstruktioner hållas uppdaterade

* ### Peer review genomförd(oplanerad)
    All kod ska återkopplas från annan utvecklare innan merge sker till master-branch. Detta kan genomföras via **Pull request** eller **Pair programming**.

* ### 100% Code Coverage (Rails) (oplanerad)
    Samtliga kodrader ska köras under en testsvit för Rails applikationer.

* ### Deploy till testmiljö genomförd (oplanerad)
    För att testa kodens deploybarhet, samt möjliggöra extern testning, ska all kod deployas till en testmiljö.

* ### Acceptanstester uppfyllda (oplanerad)
    Acceptanstester kan genomföras genom automatiska tester byggda på acceptanskriterier, eller genom testning av Produktägaren.

## Praktisk hantering
```policy
För att visa var en Job Story befinner sig enligt **Defintition of Done**, skapas en task för Job Storyn med namn _Definition of Done_ som innehåller information om definitionen vid det tillfället. 
<br/>Först när **Definition of Done** är uppfylld enligt definitionen flyttas tasken till **Done**.
```