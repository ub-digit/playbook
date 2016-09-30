# Installation och konfiguration av Solr (lokalt på utvecklardator)

## Installation
1. Hämta Java (v8 eller senare). Bäst resultat fås via http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html. Välj den senaste (här och nu jdk-8u102-linux-x64.tar.gz).
2. Hämta solr-5.5.3.tgz (eller motsvarande aktuell version) (http://apache.mirrors.spacedump.net/lucene/solr)
3. Packa upp på lämplig plats. Uppackningen bör göras som den användare som skall köra solr. Här har vi valt att packa upp som root till katalogen /opt.
4. I /opt: sudo su; 
  * tar xzvf jdk-8u102-linux-x64.tar.gz 
  * tar xzvf solr-5.5.3.tgz
5. Följaktligen kommer uppackningen att skapa katalogerna /opt/jdk1.8.0_102 och /opt/solr-5.5.3.
6. Skapa länkarna 
  * /opt/java8 -> /opt/jdk1.8.0_102 
  * /opt/solr -> /opt/solr-5.5.3.

Den härpå följande konfigurationen kan försiggå på två sätt - mha. **schema.xml** eller mha. solr's **baskonfiguration**.

## Konfiguration m.h.a. schema.xml
Nedanstående konfiguration gäller din lokala utvecklarmaskin. För serverinstallation se konfigurationen för respektive tjänst.

1. Gå till /opt/solr
2. sudo su
3. Kör solr: ```bin/solr start -a "-Djetty.host=127.0.0.1"```
4. För att skapa en indexplats (core), kör ```bin/solr create_core -c CORENAME -d basic_configs```
5. Kopiera in ```schema.xml``` till ```server/solr/CORENAME/conf/schema.xml```
6. Gå till http://localhost:8983/ eller motsvarande port som rapporteras av start-kommandot ovan.
7. I sektionen "Core Admin", välj CORENAME och klicka på "Reload"-knappen

Vid senare förändringar av schema.xml, kör stegen 4-6 ovan.

## Begränsa access till solr-index baserat på IP-nummer
Editera ```${SOLR_HOME}/server/etc/jetty.xml``` stoppa in ett konfigurationsavsnitt om IPAccessHandler
och referera till detta i befintligt avsnitt med rubriken *Set handler Collection Structure*.
I och med detta har vi en black-list som utestänger alla IP-adresser samt en white-list som tillåter endast
de utpekde IP-numren att ansluta till solr-serverns webbgränssnitt och ställa frågor gentemot solr-indexet.

    <!-- =========================================================== -->
    <!-- Configure IPAccessHandler                                   --> 
    <!-- =========================================================== -->
    <New id="IPAccessHandler" class="org.eclipse.jetty.server.handler.IPAccessHandler">
      <Call name="addWhite">
        <Arg>127.0.0.1</Arg>  <!-- Localhost -->
      </Call>
      <Call name="addWhite">
        <Arg>192.168.0.100</Arg>   <!-- IP address of server 1 -->
      </Call>
      <Call name="addWhite">
        <Arg>192.168.0.101</Arg>   <!-- IP address of server 2 -->
      </Call>
      <Call name="addWhite">
        <Arg>192.168.0.102</Arg>   <!-- IP address of server 3 -->
      </Call>
      <Call name="addWhite">
        <Arg>192.168.0.103</Arg>   <!-- IP address of server 4 -->
      </Call>
      <Set name="handler">
        <New id="Contexts" class="org.eclipse.jetty.server.handler.ContextHandlerCollection"/>
      </Set>
    </New>    

    <!-- =========================================================== -->
    <!-- Set handler Collection Structure                            -->
    <!-- =========================================================== -->
    <Set name="handler">
      <New id="Handlers" class="org.eclipse.jetty.server.handler.HandlerCollection">
        <Set name="handlers">
         <Array type="org.eclipse.jetty.server.Handler">
           <Item>
             <Ref id="RewriteHandler"/>
           </Item>
           <Item>
             <Ref id="IPAccessHandler"/>
           </Item>
           <Item>
             <New id="Contexts" class="org.eclipse.jetty.server.handler.ContextHandlerCollection"/>
           </Item>
           <Item>
             <New id="DefaultHandler" class="org.eclipse.jetty.server.handler.DefaultHandler"/>
           </Item>
           <Item>
             <New id="RequestLog" class="org.eclipse.jetty.server.handler.RequestLogHandler"/>
           </Item>
         </Array>
        </Set>
      </New>
    </Set>

## Baskonfiguration - enbart nåbar från localhost - dynamisk konfiguration

Vill man bara testa med Solr kan man sätta upp en dynamisk konfiguration. Vanligen kommer vi inte göra detta, men såhär gör man.

1. Gå till /opt/solr
2. Kör ```bin/solr start -a "-Djetty.host=127.0.0.1"```
3. För att skapa en indexplats (core), kör ```bin/solr create_core -c CORENAME```
4. Gå till http://localhost:8983/ eller motsvarande port som rapporteras av start-kommandot ovan.

