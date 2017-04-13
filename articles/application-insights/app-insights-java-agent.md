<properties 
    pageTitle="Riippuvuudet, poikkeukset ja suorittamisen kellonaikojen Java web Apps-sovellusten valvonta" 
    description="Laajennettu Java-sivustoon ja sovelluksen havainnollistamisen seuranta" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Riippuvuudet, poikkeukset ja suorittamisen kellonaikojen Java web Apps-sovellusten valvonta

*Hakemuksen tiedot on esikatselu.*

Jos sinulla on [instrumented Java web-sovelluksen kanssa hakemuksen tiedot][java], voit käyttää Java-agentti saat tarkempaa tiedot ilman koodin muutoksia:


* **Riippuvuudet:** Tiedot, jotka sovelluksen tekee muita osia, kuten puheluista:
 * **Muut puhelut** HttpClient, OkHttp ja RestTemplate (Spring) kautta.
 * **Redis** -soitettua puhelua Jedis-asiakasohjelman kautta. Jos puhelun kestää kauemmin kuin 10s-agentti hakee myös puhelu-argumentit.
 * **[JDBC puhelut](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB tai Apache Derby DB. "executeBatch" puheluita tuetaan. Jos puhelun kestää kauemmin kuin 10s-agentti ilmoittaa MySQL-ja PostgreSQL-kyselyn suunnittelu. 
* **Pyydettyjen poikkeukset:** Tietoja poikkeuksia, jotka ovat käsittelemien koodisi.
* **Menetelmä suoritusaika:** Tietoja suorittaa tiettyjä menetelmiä kuluvaa aikaa.

Käyttämään Java-agentti, voit asentaa ohjelman palvelimellesi. Web-sovellukset on instrumented kanssa [Sovelluksen havainnollistamisen Java SDK][java].

## <a name="install-the-application-insights-agent-for-java"></a>Asenna Java sovelluksen tiedot-agentti

1. Tietokoneessa käynnissä Java-palvelimeen, [Lataa agentti](https://aka.ms/aijavasdk).
2. Muokkaa sovelluksen palvelimen käynnistys-komentosarjan ja lisää seuraavat JVM:

    `javaagent:`*agentti PURKKI tiedoston koko polku*

    Valitse esimerkiksi Tomcat Linux tietokoneeseen:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Käynnistä sovelluspalvelin.

## <a name="configure-the-agent"></a>Määritä agentti

Luo tiedosto nimeltä `AI-Agent.xml` ja sijoita se agentti PURKKI tiedostoa samaan kansioon.

Aseta xml-tiedoston sisällön. Seuraavassa esimerkissä haluat sisällyttää tai jättää ominaisuuksia haluat muokata. 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

Sinun on otettava raporttien poikkeuksen ja menetelmä ajoituksen yksittäisiä menetelmistä.

Oletusarvon mukaan `reportExecutionTime` on TOSI ja `reportCaughtExceptions` on EPÄTOSI.

## <a name="view-the-data"></a>Näytä tiedot

Sovelluksen tiedot-resurssin koostetun remote riippuvuuden ja menetelmä suorittamisen ajat näkyy [kohdassa suorituskyky-ruutu][metrics]. 

Voit etsiä yksittäiset esiintymät, riippuvuudet, poikkeuksen ja menetelmä raportit, avata [haun][diagnostic]. 

[Diagnosing riippuvuuden ongelmat – Katso](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Kysymyksiä? Ongelmia?

* Tietoja? [Määritä palomuuripoikkeukset](app-insights-ip-addresses.md)
* [Java vianmääritys](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
