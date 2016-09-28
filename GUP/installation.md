# GUP Installation

## solr
En solr instans per miljö så att om __staging__ ligger på servern
__app-staging-1.ub.gu.se__, så ska det finnas en solr instans för __staging__
på samma server. När det gäller __production__-miljön så är den tänkt att ha
sin solr-instans på en separat server. 

- Lägg till användarens ssh-nyckel i installer-användarens och app-användarens authorized_keys filer.
- Vi använder version 5.3.1 eftersom 6.X inte fungerar p.g.a ett nytt sätt att konfigurera managed schema.
- Följ instruktioner i [dokument om Solr](../verktyg/solr.md).
- Drivrutiner för postgres läggs i katalogen __/opt/solr/dist__ (ex postgresql-9.4.1209.jar)
- Kopiera __config/solr/solrconfig.xml__ från gup-server-repot, lägg den i __/opt/solr/server/solr/gup-people/conf/__
- Kopiera __config/solr/people/schema.xml__ från gup-server-repot, lägg den i __/opt/solr/server/solr/gup-people/conf/__
- Kopiera __config/solr/gup-server-staging/opt/solr/server/solr/gup-people/conf/dataimportconfig.xml__ från config-repot, till i __/opt/solr/server/solr/gup-people/conf/__ på aktuell app-server.
- Skapa användare i databasen __gup__ på app-servern.
- Se till att användaren har ett lösenord som används i __pg_hba.conf__.
- cap staging deploy:check
- cap staging deploy
- se till att ha node+npm, bower, ember-cli körbart.
- kör ubdeploy.sh i config-reposets tool katalog: ubdeploy.sh staging clear
- eller ubdeploy.sh staging clear


### Ändringar i solrconfig.xml:
Lägga till filer (postgres*.jar och dataimporthandler*.jar) i classpath:
  <lib dir="${solr.install.dir:../../../..}/dist/" regex="postgresql.*\.jar" />
  <lib dir="${solr.install.dir:../../../..}/dist/" regex="solr-dataimporthandler-.*\.jar" />


Konfigurering av dataimporthandler:
  <requestHandler name="/dataimport" class="solr.DataImportHandler">
    <lst name="defaults">
      <str name="config">dataimportconfig.xml</str>
    </lst>
  </requestHandler>


Ändra default sökfäkt till det som är satt i schema.xml
<str name="df">all</str>



Konfigurera databas i dataimportconfig.xml



Full indexering sker via DataImportHandler



##
