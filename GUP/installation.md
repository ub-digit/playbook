# GUP Installation

## Servrar och miljöer

| **Server** | **Frontend** | **Backend** |
| --- | --- | --- |
| **app-lab-1.ub.gu.se** | gup-lab.ub.gu.se | gup-server-lab.ub.gu.se |
| **app-staging-1.ub.gu.se** | gup-staging.ub.gu.se | gup-server-staging.ub.gu.se |
| **app-production-1.ub.gu.se** | gup.ub.gu.se | gup-server.ub.gu.se |

## Förberedelser

- Lägg till användarens ssh-nyckel i installer-användarens och app-användarens authorized\_keys filer på respektive server.

## Solr

Principen är att ha en solr-instans per miljö så att om **staging** ligger på servern
**app-staging-1.ub.gu.se**, så har man också en solr-instans för **staging**
på samma server. När det gäller **production**-miljön hanterar vi den speciellt och den är tänkt att ha
sin solr-instans på en separat fysisk server.

Vi använder version 5.3.1 eftersom 6.X inte fungerar för oss p.g.a en annorlunda metod för konfiguration av "managed schema".

Som superuser på ifrågavarande app-server:

- Följ instruktioner i [dokument om Solr](../verktyg/solr.md).
- Hämta senaste JDBC-driver (t.ex. **postgresql-9.4.1211.jar**) från **https://jdbc.postgresql.org/download.html**
- Lägg JDBC-drivern i  **/opt/solr/dist** 
- All solr-konfiguration för gup-server ligger i config-reposet, närmare bestämt i **config/apps/gup-server/solr-config/default/opt/solr/server/solr**, nedan kallat {CONFIG}


| Fromm  | To  | 
| ---   | --- |
| **{CONFIG}/solrconfig.xml**                  | **/opt/solr/server/solr/gup-people/conf/** |
| **{CONFIG}/gup-people/schema.xml**           | **/opt/solr/server/solr/gup-people/conf/** |
| **{CONFIG}/gup-people/dataimportconfig.xml** | **/opt/solr/server/solr/gup-people/conf/** |

## Postgresql

- Skapa användare i databasen **gup** på app-servern.
- Se till att användaren har ett lösenord som används i **pg\_hba.conf**.

## Deploy av backend

Backend ligger i https://github.com/ub-digit/gup-server.git

Se till att aktuella config-fliler används. Dessa hämtas manuellt från config-reposet: https://github.com/ub-digit/config.git

- Vid behov kopiera config-filer från config-reposets katalog **apps/gup-server/config-data/**
- cap staging deploy:check
- cap staging deploy


## Deploy av frontend

Frontend ligger i https://github.com/ub-digit/gup.git

- se till att ha node+npm, bower, ember-cli körbart.
- gå till katalogen där gup är utcheckat
- kör ubdeploy.sh i config-reposets tool katalog: ubdeploy.sh staging clear
- eller ubdeploy.sh staging clear

### Ändringar i solrconfig.xml:

Lägga till filer \(postgres\*.jar och dataimporthandler\*.jar\) i classpath:

```xml
  <lib dir="${solr.install.dir:../../../..}/dist/" regex="postgresql.*\.jar" />
  <lib dir="${solr.install.dir:../../../..}/dist/" regex="solr-dataimporthandler-.*\.jar" />
```

Konfigurering av dataimporthandler:

```xml
<requestHandler name="/dataimport" class="solr.DataImportHandler">
    <lst name="defaults">
      <str name="config">dataimportconfig.xml</str>
    </lst>
  </requestHandler>
```

Ändra default sökfäkt till det som är satt i schema.xml

```xml
<str name="df">all</str>
```

- Konfigurera databas i dataimportconfig.xml
- Full indexering sker via DataImportHandler
