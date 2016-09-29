# GUP Installation

## Förberedelser

* Lägg till användarens ssh-nyckel i installer-användarens och app-användarens authorized\_keys filer.

## solr

Principen är att ha en solr-instans per miljö så att om **staging** ligger på servern
**app-staging-1.ub.gu.se**, så har man också en solr-instans för **staging**
på samma server. När det gäller **production**-miljön hanterar vi den speciellt och den är tänkt att ha
sin solr-instans på en separat fysisk server.

* 
* Vi använder version 5.3.1 eftersom 6.X inte fungerar p.g.a ett nytt sätt att konfigurera managed schema.
* Följ instruktioner i [dokument om Solr](../verktyg/solr.md).
* Drivrutiner för postgres läggs i katalogen **\/opt\/solr\/dist** \(ex postgresql-9.4.1209.jar\)
* Kopiera **config\/solr\/solrconfig.xml** från gup-server-repot, lägg den i **\/opt\/solr\/server\/solr\/gup-people\/conf\/**
* Kopiera **config\/solr\/people\/schema.xml** från gup-server-repot, lägg den i **\/opt\/solr\/server\/solr\/gup-people\/conf\/**
* Kopiera **config\/solr\/gup-server-staging\/opt\/solr\/server\/solr\/gup-people\/conf\/dataimportconfig.xml** från config-repot, till i **\/opt\/solr\/server\/solr\/gup-people\/conf\/** på aktuell app-server.
* Skapa användare i databasen **gup** på app-servern.
* Se till att användaren har ett lösenord som används i **pg\_hba.conf**.
* cap staging deploy:check
* cap staging deploy
* se till att ha node+npm, bower, ember-cli körbart.
* kör ubdeploy.sh i config-reposets tool katalog: ubdeploy.sh staging clear
* eller ubdeploy.sh staging clear

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

Konfigurera databas i dataimportconfig.xml

Full indexering sker via DataImportHandler

