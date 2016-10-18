# Definition of Done

_**Definition of Done**_ Definierar vilka moment som ska utföras innan en specifik uppgift kan betraktas som _klar_

**POLICY - Definition of Done (Förslag enl. 2015-12-11)**


### Kompletta tester finns (oplanerad)
  Komponenttester samt integrationstester ska finnas för samtlig logik i systemet.

### Dokumentation uppdaterad (oplanerad)
  * Kod ska vara tydlig och dokumenterad
  * APIer ska vara dokumenterade
  * Vid behov ska manualer / applikationsinstruktioner hållas uppdaterade

### Peer review genomförd (oplanerad)
  All kod ska återkopplas från annan utvecklare innan merge sker till master-branch. Detta kan genomföras via **Pull request** eller **Pair programming**.

### 100% Code Coverage (Rails) (oplanerad)
  Samtliga kodrader ska köras under en testsvit för Rails applikationer.

### Deploy till testmiljö genomförd (planerad och införd i JIRA)
  För att testa kodens deploybarhet, samt möjliggöra extern testning, ska all kod deployas till en testmiljö. Vanligen är detta "lab". 
  > 1. Utvecklaren gör "cap lab deploy", "ubdeploy.sh lab" eller motsvarande för aktuellt system.
  > 2. Utvecklaren flyttar DoD till **Acceptance Test** enligt "Acceptanstester uppfyllda"-punkten.

### Acceptanstester uppfyllda (planerad och införd i JIRA)
  Produktägaren ska **Acceptanstesta** storyn innan den anses vara **Done**.
  > 1. Utvecklaren flyttar DoD tasken till **Acceptance Test** kolumnen.
  > 2. PO (eller någon som delegerats av PO) testar funktionaliteten gentemot storyns innehåll.
  > 3. PO flyttar DoD till 
  4. **Done** ifall storyn är uppfylld, **eller**
  5. **In Progress** med kommentarer ifall storyn inte är klar. Felaktigheter noteras i en UAT-Bug som läggs i **Todo**

## Praktisk hantering
>1. Skapa en **task** för Storyn som heter **Definition of Done**, innehållande den för tillfället rådande definitionen.
2. Först när **Definition of Done** är uppfylld enligt definitionen flyttas tasken till **Done**.