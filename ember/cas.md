# CAS mot Ember

## Termer
<pre>
 SERVICE:  URL specifik för tjänsten. Den URL som inloggningen skickar tillbaka till efter lyckad inloggning
 TICKET:   Kod som tillsammans med SERVICE identifierar en korrekt inloggning
 BASEURL:  Bas till CAS-tjänsten
 FUNKTION: "login", "logout", "serviceValidate"
</pre>

 
## Länk till inloggning

Länken byggs helt enkelt upp enligt:

``${BASEURL}/${FUNKTION}?service=${SERVICE}``



## Inloggning

Inloggningsfunktionaliteten står CAS-tjänsten helt för



## Lyckad inloggning (misslyckad lämnar aldrig CAS-tjänsten)

När CAS-inloggning lyckats blir webbläsaren skickad till den URL som utgör SERVICE. Som extra parameter läggs till TICKET på formen:

``${SERVICE}?ticket=${TICKET}``

TICKET kan nu fångas via queryParams i ApplicationController

TODO: KODBLOCK

Följande sekvens sker nu:
1. ApplicationRoute.beforeModel() anropas
2. beforeModel ser nu att ticket finns och kör session.authenticate() med ticket/service som parametrar
3. session.authenticate() skickar ticket/service till backend
4. backend skickar ticket/service till CAS:serviceValidate
5. CAS:serviceValidate svarar med authenticationSuccess och det användarnamn som användes för inloggningen
6. backend skickar tillbaka en AccessToken med token+username
7. Ember tar emot token+username och fungerar sedan som alla våra andra inloggningsfunktioner

Efter denna sekvens används inte CAS mer alls i nuläget. Om man vill ta hänsyn till CAS löpande kan man låta backend spara TICKET tillsammans med AccessToken och göra kontroller mot CAS:serviceValidate och invalidera AccessToken om CAS slutar vara giltigt.
