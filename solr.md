# Solr lokalt på utvecklardator

## Installation
1. Installera Java (v7 eller senare)
2. Hämta solr-5.3.1.tgz (eller motsvarande aktuell version) (http://apache.mirrors.spacedump.net/lucene/solr/5.3.1/)
3. Packa upp någonstans där du tycker du vill ha din installation

## Baskonfiguration - enbart nåbar från localhost - dynamisk konfiguration
1. Gå till din installationskatalog
2. Kör ```bin/solr start -a "-Djetty.host=127.0.0.1"```
3. För att skapa en indexplats (core), kör ```bin/solr create_core -c CORENAME```
4. Gå till http://localhost:8983/ eller motsvarande port som rapporteras av start-kommandot ovan.

## Konfiguration när schema.xml finns
1. Gå till din installationskatalog
2. Kör ```bin/solr start -a "-Djetty.host=127.0.0.1"```
3. För att skapa en indexplats (core), kör ```bin/solr create_core -c CORENAME -d basic_configs```
4. Kopiera in ```schema.xml``` till ```server/solr/CORENAME/conf/schema.xml```
5. Gå till http://localhost:8983/ eller motsvarande port som rapporteras av start-kommandot ovan.
6. I sektionen "Core Admin", välj CORENAME och klicka på "Reload"-knappen

Vid senare förändringar av schema.xml, kör stegen 4-6 ovan.
