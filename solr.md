# Solr lokalt på utvecklardator

## Installation
1. Installera Java (v7 eller senare)
2. Hämta solr-5.3.1.tgz (eller motsvarande aktuell version) (http://apache.mirrors.spacedump.net/lucene/solr/5.3.1/)
3. Packa upp någonstans där du tycker du vill ha din installation

## Baskonfiguration - enbart nåbar från localhost - dynamisk konfiguration
1. Gå till din installationskatalog
2. Kör ```bin/solr start -a "-Djetty.host=127.0.0.1"```
3. För att skapa en indexplats (core), kör ```bin/solr create_core -c CORENAME
4. Gå till http://localhost:8983/ eller motsvarande port som rapporteras av start-kommandot ovan.
