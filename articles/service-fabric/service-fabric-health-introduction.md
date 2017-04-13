<properties
   pageTitle="Kunnon seuranta palvelun kangasta | Microsoft Azure"
   description="Azure-palvelu kangasta kunnon seuranta-malli, joka sisältää seuranta klusterin ja sen sovellusten ja palveluiden esittely."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="introduction-to-service-fabric-health-monitoring"></a>Johdanto kangasta palvelun kunnon valvonta
Azure palvelun kangasta esitellään kunto mallia, joka sisältää monipuolisia, joustava ja extensible kunto arviointi ja raportointi. Mallin avulla klusterin ja palveluita ei tilan seuranta lähellä-reaaliajassa. Voit hankkia kuntoa ja korjaa mahdolliset ongelmat, ennen kuin he limittäin ja aiheuttaa valtaviin katkokset. Normaali-mallissa palvelut lähettää paikallisen näkymien perustuvia raportteja ja tiedot yhdistetään antamaan yleinen klusterin tason näkymä.

Palvelun kangasta osat RTF kuntoa tämän mallin avulla voit raportoida niiden nykyinen tila. Voit käyttää samalla raportin kunto sovellukset. Jos sijoittamaan laadukkaita kunto raportoinnissa, joka sisältää mukautettuja ehtoja, voit tunnistaa ja korjaa ongelmia käynnissä sovelluksen paljon aiempaa helpommin.

> [AZURE.NOTE] On aloittanut kunto-alirakenne valvottu päivitykset tarvitaan sähköpostiosoite. Palvelun kangasta tarjoaa valvottu sovelluksen ja klusterin päivitykset, jotka varmistavat koko saatavuus, ei ole käyttökatkot ja vähän käyttäjän toimia. Saavuttamiseksi mainitut päivitys tarkistaa mukaisesti terveyden määritettyjä päivityksen käytäntöjä ja Jatka vain, kun kunto kunnioitetaan haluamasi raja-arvot päivityksen avulla. Muussa tapauksessa päivitys on automaattisesti palautettu tai keskeytetty antaa järjestelmänvalvojien voit ratkaista ongelmat. Lisätietoja sovellusten päivityksiä, lue [artikkeli](service-fabric-application-upgrade.md).

## <a name="health-store"></a>Kunto-säilö
Kunto-kaupan pitää kohteiden terveyteen liittyviä tietoja klusterin kohteessa ja arviointia varten. Se on toteutettu kuin palvelun kangasta samanlainen tilallisten palvelun suuren käytettävyyden ja skaalattavuus varmistamiseksi. Kunto-kaupan kuuluu **kangasta: / järjestelmän** sovellus, ja se on käytettävissä, kun klusterin on käytössä ja käytössä.

## <a name="health-entities-and-hierarchy"></a>Kuntotietojen yksiköt ja hierarkia
Kunto-kohteet on järjestetty looginen hierarkia, joka kuvaa vuorovaikutukset ja välisten riippuvuuksien vaatimat eri kohteet. Kohteiden ja hierarkian sisältyvät automaattisesti kunto säilön raporttien vastaanotti palvelun kangasta osien perusteella.

Kunto-kohteiden peilikuvat palvelun kangasta kohteita. (Esimerkiksi **kunto sovelluksen kohteen** vastaa sovelluksen käyttöön klusterin, kun **kunto solmu kohteen** vastaa palvelun kangasta klusterin solmun esiintymä.) Kunto-hierarkia sieppaa vuorovaikutukset järjestelmän-kohteet ja se on perusta Lisäasetukset kunto arvioitavaksi. Lisätietoja palvelun kangasta keskeisten käsitteiden [palvelun kangasta teknisiä tietoja](service-fabric-technical-overview.md). Saat lisätietoja sovelluksen- [palvelun kangasta sovelluksen malli](service-fabric-application-model.md).

Kunto-objektit ja hierarkian Salli klusterin ja sovelluksia tehokkaasti ilmoitettu, aloittaessasi, ja seurata. Kunto-malli on oikein, *hajautetun* esittäminen monta liikkuvat osat klusterin kunto.

![Kuntotietojen yksiköt.][1]
Kunto-kohteiden järjestetty hierarkia ylä-ja alatason yhteydet perusteella.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

Kunto-kohteet ovat seuraavat:

- **Klusterin**. Edustaa palvelun kangasta klusterin kunto. Klusterin kuntoraportit kuvaavat ehdot, jotka vaikuttavat koko klusterin ja ei tuloksia vähintään yksi perusasemassa lasten. Esimerkkejä tällaisista tiedostoista ovat aivot klusterin jakaminen jakaminen tai yhteydenotto verkko-ongelmien vuoksi.

- **Solmun**. Edustaa palvelun kangasta solmu kunto. Solmun kuntoraportit kuvaavat ehdot, jotka vaikuttavat solmu-toimintoja. Yleensä ne vaikuttavat siihen kaikki käyttöön käytössä olevat kohteet. Esimerkkejä tällaisista tiedostoista ovat kun solmu on levyn välilyönnin (tai toisen tietokoneen-ominaisuutta, kuten muistin yhteydet) ja solmu ei ole käytettävissä. Solmu-kohteen tunnistetaan solmu nimeltä (merkkijono).

- **Sovelluksen**. Edustaa käynnissä klusterin sovelluksen esiintymä kunto. Sovelluksen kuntoraportit kuvaavat ehdot, jotka vaikuttavat sovelluksen yleinen kunto. Ne et voi olla tuloksia yksittäisiä lasten (‑palveluista ja ‑sovelluksista otettu käyttöön). Esimerkkejä eri palveluiden välillä lopusta loppuun-vuorovaikutus-sovelluksessa. Sovelluksen kohteen tunnistetaan sovelluksen nimen (URI).

- **Palvelun**. Edustaa käynnissä klusterin palvelun kunto. Palvelun kuntoraportit kuvaavat ehdot, jotka vaikuttavat yleistä palvelun kunto ja niitä et voi olla tuloksia osion tai replikan. Esimerkkejä tällaisista tiedostoista ovat palvelun kokoonpanon (esimerkiksi portti tai ulkoisen tiedoston jakaminen), joka aiheuttaa ongelmia kaikki osiot. Palvelu-kohteen tunnistetaan palvelunimi (URI).

- **Osion**. Edustaa service-osion kunto. Osion kuntoraportit kuvaavat ehdot, jotka vaikuttavat koko replikajoukosta. Esimerkkejä tällaisista tiedostoista ovat määrä on on alle kohde määrä ja kun osio on quorum menettämisen. Osiota kohteen tunnistetaan osion GUID-tunnus.

- **Replikan**. Edustaa tilallisten palvelun replikan tai tilattomien palvelun esiintymän kunto. Pienin yksikkö, watchdogs ja järjestelmäosat voit raportoida sovelluksen. Tilallisten services esimerkkejä raportointia, kun sitä ei voi replikoida toimintojen secondaries ja kun replikointia ei ole jatkamista osoitteessa odotettua ensisijainen replikan tahtiin. Tilattomien esiintymän raportoida myös, kun se suoritetaan resurssit tai on yhteysongelmat. Replikan kohteen tunnistetaan osion GUID-tunnus ja replikan tai esiintymän tunnus (long).

- **DeployedApplication**. Edustaa kunto *sovellusta solmun*. Sovelluksen kuntoraportit kuvaavat ehdot tietyn sovelluksen-solmu, joka ei tuloksia service-paketteja, jotka on otettu käyttöön sama solmu. Esimerkkejä tällaisista tiedostoista ovat kun sovelluspaketin lataaminen ei onnistu, valitse solmun ja ollessa sovelluksen suojauksen pääkäyttäjät solmun ongelma-määrittäminen. Sovelluksen tunnistetaan sovelluksen (URI) ja Solmunimi (merkkijono).

- **DeployedServicePackage**. Edustaa palvelupakettiin, klusterin solmu käytössä kunto. Se kuvaa tietyn palvelun pakettiin määritetyt ehdot eivät vaikuta muiden pakettien palvelun saman solmun saman sovelluksen. Esimerkkejä koodi-paketti, joka ei voi aloittaa palvelupakettiin ja määritys-paketti, joka ei voi lukea. Käyttöön palvelupakettiin tunnistetaan sovelluksen nimi (URI), Solmunimi (merkkijono) ja palvelun luettelon nimi (merkkijono).

Rakeisuuden kunto-mallin avulla on helppo tunnistaa ja korjaa ongelmat. Esimerkiksi jos palvelu ei vastaa, on mahdollista ilmoittaa, että sovelluksen esiintymää on perusasemassa. Raportointia että tason ei ole ihanteellinen, mutta koska ongelma saattaa ei vaikuta sovelluksen kaikki palvelut. Raportin olisi sovellettava perusasemassa-palveluun tai tietyn ali-osioon, jos lisätietoja osoittaa kyseisen osion. Tietoja automaattisesti hierarkian ja perusasemassa osion Näyttömalli käyttää tehdään näkyvä palvelu ja sovelluksen tasolla. Tämä kooste voit paikantaa ja korjata ongelman syy pääkansion nopeammin.

Kuntotietojen hierarkian koostuu ylä-ja alatason yhteydet. Klusterin koostuu solmujen ja sovellukset. Sovellusten palveluja ja sovellusten käyttöön. Käyttöön sovellukset on otettu käyttöön palvelupakettien. Palvelulla olla osioissa ja kunkin osion on yksi tai useampi replikoida. On erityinen suhde solmujen ja otettu käyttöön kohteet. Jos solmu on perusasemassa ilmoittaa sen myöntäjä järjestelmäosan (automaattisesti hallintapalvelu), se vaikuttaa käyttöön sovellukset, palvelupakettien ja replikoiden ottaa sen käyttöön.

Kuntotietojen hierarkian edustaa uusin järjestelmän perusteella uusimman kuntoraportit, joka on melkein reaaliaikaisia tietoja.
Sisäisiin ja ulkoisiin watchdogs raportoida saman kohteiden sovelluksen kielikohtaiset logiikan tai mukautettujen valvottu ehtojen perusteella. Käyttäjän raporttien olla Järjestelmäraportit.

Suunnittele sijoittamaan raportin ja vastata kunto suunnitteluvaiheessa suuri pilvipalvelussa palvelu on helpottaa virheenkorjaus, valvoa ja toimia.

## <a name="health-states"></a>Kuntotietojen tilat
Palvelun kangasta käyttää kuvaus, onko kohteen kunnossa kunto kolmesta tilasta: OK varoitus ja virhe. Minkä tahansa raportin lähetetään kunto Store on määritettävä yksi osavaltion. Kuntotietojen arvioinnin tuloksena on osavaltion.

Mahdollisia [kunto tilat](https://msdn.microsoft.com/library/azure/system.fabric.health.healthstate) ovat:

- **OK**. Kohde on kunnossa. Jakautuvat ei ole ilmoitettu se tai (tarvittaessa) sen alisivustojen käyttöä rajoitetaan tunnetut ongelmat.

- **Varoitus**. Kohteen ilmenee ongelmia, mutta se ei ole vielä perusasemassa (esimerkiksi on viiveitä, mutta ne eivät aiheuta toimintojen ongelmia vielä). Joissakin tapauksissa varoitus ehto saattaa korjata itse ilman erityistä toimia ja se on hyödyllinen antamaan näkyvyys mikä on kyse. Muussa tapauksessa varoitus ehto voi heikentää vakavia ongelma viittauksista kyselyjä.

- **Virhe**. Kohde on perusasemassa. Toiminto olisi otettava korjaa kokonaisuus, tila, koska se ei toimi oikein.

- **Tuntematon**. Kohde ei ole kunto-kaupasta. Tämä tulos saadaan hajautettu kyselyt, joka yhdistää useita osia tulokset. Esimerkiksi saada siirtyy **FailoverManager** ja **HealthManager**, hanki sovellus luettelosta kyselyn siirtyy **ClusterManager** ja **HealthManager**solmu kyselyä. Nämä kyselyt yhdistää useita järjestelmäosat tulokset. Jos toinen järjestelmäosan palauttaa kohteen, joka ei ole saavuttanut tai on ole tyhjennetty kunto-kaupasta, yhdistetyt tulos on tuntematon kuntotietojen tarkistus.

## <a name="health-policies"></a>Kuntotietojen käytännöt
Kunto-kaupan koskee kuntokäytännöt määrittääksesi, onko kohde on kunnossa sen raportteja ja sen alisivustojen käyttöä rajoitetaan perusteella.

> [AZURE.NOTE] Kuntotietojen käytännöt voidaan määrittää klusterin luettelo (arvioitavaksi klusterin ja solmu kunto) tai (sovelluksen arviointi ja sen alisivustojen käyttöä rajoitetaan jokin) varten sovellusluettelo. Kuntotietojen arvioinnin pyynnöt voit myös siirtää mukautetun kunto arvioinnin käytäntöjä, jota käytetään vain arviointia varten.

Oletusarvon mukaan palvelun kangasta koskevat säännöt (kaikki tiedot on oltava kunnossa) tarkka ylä-ja alatason hierarkkinen suhde. Jos jopa jokin lasten on yksi perusasemassa tapahtuman, ylemmän tason pidetään perusasemassa.

### <a name="cluster-health-policy"></a>Klusterin kunto käytäntö
[Klusterin kuntokäytännön](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.aspx) käytetään arvioida klusterin kuntotietojen ja solmu kunto hyötyä. Klusterin luettelo voidaan määrittää käytännön. Jos ei ole määritetty, käytetään oletuskäytäntö (nolla siedetty virhettä).
Klusterin kunto-käytäntö sisältää:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.considerwarningaserror.aspx). Määrittää, onko, käsittele varoitus kunto raportoi virheinä kunto arvioinnin aikana. Oletus: false.

- [MaxPercentUnhealthyApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications.aspx). Määrittää sovellukset, jotka voivat virheelliseksi, ennen kuin klusterin katsotaan virhe siedetty enimmäisprosenttiarvon.

- [MaxPercentUnhealthyNodes](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes.aspx). Määrittää, jotka voivat virheelliseksi, ennen kuin klusterin katsotaan virhe solmujen siedetty enimmäisprosenttiarvon. Suuri klustereissa joidenkin solmujen koskevat aina alaspäin tai loitontaa korjaukset, niin käyttävät tätä prosentteina, joka sallii.

- [ApplicationTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap.aspx). Sovelluksen tyyppi kunto käytännön kartan voidaan klusterin kunto arvioinnin aikana kuvaamaan määräten sovelluksen tyyppejä. Oletusarvon mukaan kaikki sovellukset resurssivarantoon laittaa ja lasketaan MaxPercentUnhealthyApplications kanssa. Jos sovelluksen Jotkin tiedostotyypit käsitellään eri tavalla, ne voidaan ottaa pois yleinen resurssivarantoon. Sen sijaan ne arvioidaan vastaan prosenttiosuudet liittyvät niiden sovelluksen nimi-määrityksen. Esimerkiksi klusterissa on tuhansia erityyppisiä sovellukset ja muutamia ohjausobjektin Palvelusovellusten esiintymät määräten sovelluksen tyyppi. Ohjausobjektin sovellukset pidä koskaan olla virhe. Voit määrittää yleisen MaxPercentUnhealthyApplications 20 %, Hyväksy joitakin virheitä, mutta "ControlApplicationType" arvoksi 0 MaxPercentUnhealthyApplications sovelluksen tyyppi. Tällä tavalla, jos jotkin monet sovellukset ovat perusasemassa, mutta yleinen perusasemassa prosenttiosuutta klusterin laskettava varoitus. Varoitus kuntotietojen ei vaikuta klusterin päivitys tai muita seuranta käynnistämä virhe kuntotietojen tarkistus. Mutta myös ohjausobjektista sovelluksen virhe tekee klusterin perusasemassa, jonka palautus käynnistää tai keskeyttää klusterin päivityksen, Päivitä asetuksista riippuen.
Sovelluksen eri määritetty määrityksen kaikki sovelluksen esiintymät otetaan sovellusten yleinen resurssivarantoon ulos. Ne arvioidaan sovellusten sovelluksen tyypin käyttämällä tietyn MaxPercentUnhealthyApplications muuntomalli kokonaismäärän perusteella. Kaikki sovellukset muiden säilyvät yleinen resurssivarantoon ja arvioidaan MaxPercentUnhealthyApplications kanssa.

Seuraavassa esimerkissä on ote klusterin luettelo. Voit määrittää sovelluksen tyyppi-määrityksen, etuliite "ApplicationTypeMaxPercentUnhealthyApplications-"-sovelluksen tyyppi nimen perässä parametrin nimi.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Sovelluksen kunto käytäntö
[Sovelluksen kuntokäytännön](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.aspx) kuvataan, miten laskennan tapahtuma-ja ali-tilan kooste on valmiiksi sovellusten ja niiden alisivustojen käyttöä rajoitetaan. Se voidaan määrittää sovelluksen luettelo, **ApplicationManifest.xml**, kirjoita sovelluspaketin. Jos ole käytäntöjä ei määritetä, palvelun kangasta oletetaan, että kohde on perusasemassa, jos se on varoitus- tai kuntotietojen kuntoraportti tai alle.
Määritettävät käytännöt ovat seuraavat:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Määrittää, onko, käsittele varoitus kunto raportoi virheinä kunto arvioinnin aikana. Oletus: false.

- [MaxPercentUnhealthyDeployedApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications.aspx). Määrittää käyttöön sovellukset, jotka voivat virheelliseksi, ennen kuin sovellus katsotaan virhe siedetty enimmäisprosenttiarvon. Tämä prosenttiosuus lasketaan jakamalla perusasemassa käyttöön sovellusten määrän päälle, sovellukset on tällä hetkellä otettu käyttöön klusterin solmujen määrän. Laskemisen pyöristää ylöspäin Hyväksy pieni solmujen määrän yksi virhe. Oletusarvoinen prosentti: nolla.

- [DefaultServiceTypeHealthPolicy](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy.aspx). Määrittää palvelun tyyppi kunto oletuskäytäntö, joka korvaa kunto oletuskäytäntö tyypeissä service-sovelluksessa.

- [ServiceTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap.aspx). Sisältää palvelun kuntokäytännöt kohti palvelutyyppi kartta. Käytännöt korvaa palvelun tyyppi kunto oletuskäytäntöjä määritetyn palvelun mistäkin. Esimerkiksi jos sovellus on tilattomien yhdyskäytävän palvelutyyppi ja tilallisten ohjelma palvelutyyppi, voit määrittää niiden arvioitavaksi kuntokäytännöt eri tavalla. Kun määrität käytännön kohti palvelutyyppi, voit käyttää eritellympiä hallinnan palvelun kunto.

### <a name="service-type-health-policy"></a>Palvelun tyyppi kunto käytäntö
[Palvelun tyyppi kunto käytäntö](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.aspx) määrittää laskeminen ja koostaa palvelut ja alisivustojen käyttöä rajoitetaan palveluista. Käytäntö sisältää:

- [MaxPercentUnhealthyPartitionsPerService](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice.aspx). Määrittää perusasemassa osioiden siedetty enimmäisprosenttiarvon, ennen kuin palvelu pidetään perusasemassa. Oletusarvoinen prosentti: nolla.

- [MaxPercentUnhealthyReplicasPerPartition](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition.aspx). Määrittää perusasemassa replikoiden siedetty enimmäisprosenttiarvon, ennen kuin osion pidetään perusasemassa. Oletusarvoinen prosentti: nolla.

- [MaxPercentUnhealthyServices](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices.aspx). Määrittää perusasemassa services siedetty enimmäisprosenttiarvon, ennen kuin sovellus pidetään perusasemassa. Oletusarvoinen prosentti: nolla.

Seuraavassa esimerkissä on ote sovellusluettelo:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Kuntotietojen arviointi
Käyttäjien ja automaattinen palvelujen laskea kunto minkä tahansa kohteen milloin tahansa. Arviointikohde yksikön kunto kunto kaupan koostefunktioista kaikki kunto raportoi kohteen ja arvioi kaikki sen alisivustojen käyttöä rajoitetaan (tarvittaessa). Kuntotietojen kooste algoritmin käyttää kuntokäytännöt, jotka määrittävät, miten voit arvioida kuntoraportit ja kootaan lapsen kunto hyötyä (tarvittaessa).

### <a name="health-report-aggregation"></a>Kuntotietojen raportin kooste
Yhden kohteen voi olla useita eri reporters (järjestelmäosia tai watchdogs) eri ominaisuuksien lähettämä kuntoraportit. Koosteen käyttää liittyvien terveys-käytäntöjä, erityisesti sovelluksen tai klusterin kuntokäytännön ConsiderWarningAsError jäsen. ConsiderWarningAsError määrittää, miten voit arvioida varoitukset.

Koostetun kuntotietojen käynnistämä yksikön *huonoin* kuntoraportit. Jos näkyvissä on vähintään yksi virhe kuntoraportti, koostetun kuntotietojen tarkistus on virhe.

![Kuntotietojen raportin virheraportin kooste.][2]

Kunto-kohteen, jossa on vähintään yksi virhe kuntoraportit arvioidaan virheeksi. Sama koskee päättyneen kuntoraportti sen kuntotietojen riippumatta.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Jos ei ole virheraporttien ja yksi tai useampi varoitus, koostetun kuntotietojen tarkistus on varoitus tai virhe ConsiderWarningAsError käytännön merkinnän mukaan.

![Kuntotietojen raportin kooste varoitus raportti ja ConsiderWarningAsError EPÄTOSI.][3]

Kuntotietojen raportin kooste varoitus raportti ja ConsiderWarningAsError arvoksi false (oletus).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Lapsen kunto kooste
Kohteen koostetun kuntotietojen kuvastaa lapsen kunto tilat (tarvittaessa). Salausalgoritmi yhdistäminen lapsen kunto hyötyä käyttää kuntokäytännöt sovellettavan kohteen tyypin mukaan.

![Lapsen kohteiden kunto kooste.][4]

Lapsen kooste mukaisesti terveyden käytännöt.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Kun kunto-kaupan laskenut kaikki lasten, niiden kunto-tilan perusteella määritetty enimmäisprosenttiarvon perusasemassa lasten kokoaa yhteen. Tämä prosenttiosuus otetaan käytäntö kohde ja ali-tyypin mukaan.

- Jos kaikki lasten on hyötyä OK, lapsen koostetun kuntotietojen on OK.

- Jos lasten on OK ja varoitus hyötyä lapsen koostetun kuntotietojen varoitus.

- Jos määritettynä on lasten virheen jäsenvaltioiden, jotka eivät tue suurin sallittu prosenttiosuus perusasemassa lasten kanssa, koostetun kuntotietojen tarkistus on virhe.

- Jos virheen jäsenvaltioiden kanssa lasten noudattaa perusasemassa lasten sallitun enimmäisprosenttiarvon, koostetun kuntotietojen tarkistus on varoitus.

## <a name="health-reporting"></a>Kuntotietojen raportointi
Järjestelmäosat ja järjestelmän kangasta sovellusten sisäinen/ulkoinen watchdogs raportoida vastaan palvelun kangasta kohteita. Reporters Varmista valvottu ne seurantaa ehtoihin perustuvia kunto *paikallisen* määritykset. Niitä ei tarvitse missä tahansa yleisen tilan tai koostetietoja kohde. Haluamasi toiminta on yksinkertainen reporters ja ei monimutkaisia organismit, jotka on tarkistettava, mitä tietoja haluat lähettää määrittääkseen monella eri tavalla.

Kuntotietojen tietojen lähettäminen kunto-kaupasta, reporter on kyseessä olevaan kohteen selvittäminen ja luominen kuntoraportti. Raportin voi lähettää kautta käyttämällä [FabricClient.HealthClient.ReportHealth](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient_members.aspx)API, PowerShellin kautta tai muiden kautta.

### <a name="health-reports"></a>Kuntoraportit
Kunkin klusterin kohteiden [kuntoraportit](https://msdn.microsoft.com/library/azure/system.fabric.health.healthreport.aspx) olla seuraavat tiedot:

- **SourceId**. Merkkijono, joka yksilöi reporter kunto tapahtuman.

- **Kohteen tunnus**. Näet, jossa raporttia käytetään kohteen. Se vaihtelee [kohteen, kirjoita](service-fabric-health-introduction.md#health-entities-and-hierarchy):

  - Klusterin. Ei mitään.

  - Solmu. Solmunimi (merkkijono).

  - Sovelluksen. Sovelluksen nimi (URI). Edustaa käyttöön klusterin sovelluksen esiintymän nimi.

  - Palvelu. Palvelun nimi (URI). Edustaa klusterin käyttöön palvelun esiintymän nimi.

  - Osio. Osion tunnus (GUID). Edustaa osion yksilöllinen.

  - Replikan. Tilallisten palvelun replikan tunnus tai tilattomien palvelun Esiintymätunnus (INT64).

  - DeployedApplication. Sovelluksen nimi (URI) ja Solmunimi (merkkijono).

  - DeployedServicePackage. Sovelluksen nimi (URI), Solmunimi (merkkijono) ja palvelun luettelon nimi (merkkijono).

- **Ominaisuus**. *Merkkijono* (ei kiinteä luettelointi), joka sallii reporter luokitella tietyn ominaisuuden kohteen kunto-tapahtuma. Esimerkiksi reporter A raportoida Node01 "tallennustilan"-ominaisuus ja reporter B kunto raportoida kuntotietojen Node01 "yhteyden"-ominaisuutta. Kunto-kaupan raportit julkaistaan käsitellään eri kunto tapahtumien Node01-kohteen.

- **Kuvaus**. Merkkijono, joka sallii reporter kunto tapahtumasta yksityiskohtaiset tiedot. **SourceId**, **ominaisuuden**ja **HealthState** täysin kuvataan raportin. Kuvaus Lisää näytettävän raportin tietoja. Tekstin helpompi ymmärtää kuntoraportti järjestelmänvalvojille ja käyttäjille.

- **HealthState**. [Luettelointi](service-fabric-health-introduction.md#health-states) , joka kuvaa raportin kuntotietojen tarkistus. Hyväksytyt arvot ovat OK, varoitus ja virhe.

- **TimeToLive**. Aikajakson, joka ilmaisee, miten kauan kuntoraportti on voimassa. Yhdistää **RemoveWhenExpired**, se sallii tietää, miten voit arvioida vanhentuneet tapahtumien kunto-kaupasta. Oletusarvoisesti arvo on äärettömän ja raportti on voimassa jatkuvasti.

- **RemoveWhenExpired**. Totuusarvo. Jos arvo on TOSI, vanhentuneiden kuntoraportti poistetaan automaattisesti kunto-kauppa ja raportin ei vaikuttaa kohteen kunto arviointi. Käytetään, kun raportti on kelvollinen määritetyn ajanjakson aikana vain ja reporter ei tarvitse erikseen valinta. Sitä käytetään myös poistaa raporttien kunto-kaupasta (esimerkiksi watchdog muutetaan ja lopettaa lähettämisen raporttien edellisen lähde- ja ominaisuutta). Se lähettää raportti, jossa on lyhyt TimeToLive sekä RemoveWhenExpired selvittää minkä tahansa edelliseen tilaan kunto-kaupasta. Jos arvo on EPÄTOSI, vanhentuneiden raportissa käsitellään virheen kunto arviointiin. Väärä arvo signaalit kunto säilöön, lähteen Ilmoita säännöllisesti tätä ominaisuutta. Jos näin ei ole, valitse käytettävä jotakin vikaa watchdog. Kunto watchdog kirjataan sivustokokoelmatyyppien tapahtuman virheeksi.

- **SequenceNumber**. Positiivinen kokonaisluku, joka on oltava koskaan lisääntyvien, se edustaa raporttien järjestystä. Sitä käytetään kunto säilön esiintyvien vanhentuneiden raportteja, jotka on vastaanotettu myöhässä verkon viivyttää tai muiden ongelmien vuoksi. Raportin on hylätty, jos järjestysnumero on pienempi tai yhtä suuri useimmin viimeksi käytetty numero saman kohteen, lähde ja ominaisuudet. Jos sitä ei ole määritetty, järjestysnumero luodaan automaattisesti. On tarpeen Vie järjestysnumero vain, kun raportoidaan tilan siirrot. Tässä tilanteessa lähde on muista, mitä se lähetettyjä raportteja ja säilyttää tiedot automaattisesti palauttamista varten.

Nämä neljä kappaletta tietojen--SourceId, kohteen tunnus tai ominaisuuden HealthState--varten tarvittavat jokaisen kuntoraportti. Merkkijonoa ei sallita aloittaa SourceId etuliite "**järjestelmän.**", joka on varattu järjestelmän raportit. Saman kohteen on vain yksi raportin samasta lähteestä ja ominaisuudet. Raportit, sama lähde- ja ominaisuus ohittaa toisiinsa, joko kunto asiakaspuolen (jos ne ovat erämuotoinen) tai kuntotietojen Tallenna sivu. Korvaa perustuu numeroita; uudempaan-raportit (suuremmat järjestysnumerot) korvaa vanhat raportit.

### <a name="health-events"></a>Kuntotietojen tapahtumat
Sisäisesti terveys-kaupan pitää [kunto tapahtumia](https://msdn.microsoft.com/library/azure/system.fabric.health.healthevent.aspx), jotka sisältävät kaikkien tietojen poistaminen raportteja ja metatietoja. Metatietojen sisältää raportin annettiin kunto asiakkaan aikaa ja sitä on muokattu palvelinpuolen aika. Kuntotietojen tapahtumat palautetaan [kunto](service-fabric-view-entities-aggregated-health.md#health-queries)kyselyjen.

Lisätty metatietojen sisältää:

- **SourceUtcTimestamp**. Aika raportin annettiin kunto-asiakasohjelma (UTC-ajasta).

- **LastModifiedUtcTimestamp**. Aika, raportti on viimeksi muokattu palvelinpuolen (UTC-ajasta).

- **IsExpired**. Merkintä, joka osoittaa, onko raportti on vanhentunut, kun kysely on suoritettu kunto säilön. Tapahtuman voi olla vanhentunut, vain, jos RemoveWhenExpired on EPÄTOSI. Muussa tapauksessa tapahtuman kysely ei palauttanut ja poistetaan-kaupasta.

- **LastOkTransitionAt**, **LastWarningTransitionAt** **LastErrorTransitionAt**. OK tai varoitus ja virhe siirtymät viimeksi. Nämä kentät antaa kuntotietojen historiatiedot tilan siirrot tapahtuman.

Tilan siirtymä-kenttiin voidaan entistä tehokkaammin ilmoituksia tai "historiallisten" kunto tapahtumatietoja. Ne käyttöön skenaarioiden seuraavasti:

- Ilmoitus, kun ominaisuus on osoitteessa varoitus ja virhe yli X minuutin. Tarkistuksen ehdot tietyn ajan vältetään ilmoitusten tilapäinen ehtojen perusteella. Esimerkiksi ilmoituksen, jos kuntotietojen tarkistus on tehty varoitus enintään viiteen minuuttiin voidaan muuntaa (HealthState == varoitus ja nyt - LastWarningTransitionTime > 5 minuuttia).

- Ilmoitusten vain-ehdot, jotka ovat muuttuneet viimeisen X minuutin. Jos raportti on jo virheen ennen määritetyllä aikavälillä, se voi ohittaa, koska se on jo suorittanut aiemmin.

- Jos ominaisuus on VAIHTO välillä varoitus ja virhe, selvitä, kuinka kauan aikaa on kulunut perusasemassa (eli ei OK). Esimerkiksi ilmoituksen, jos ominaisuus ei ole ollut kunnossa enintään viisi minuuttia voidaan muuntaa (HealthState! = Ok ja nyt - LastOkTransitionTime > 5 minuuttia).

## <a name="example-report-and-evaluate-application-health"></a>Esimerkki: Raportin ja sovelluksen kunto arvioiminen
Seuraava esimerkki lähettää kuntoraportti PowerShellin kautta sovelluksen **kangasta: / WordCount** lähteestä **MyWatchdog**. Kuntotietojen raportti sisältää kunto-ominaisuuden "käytettävyys" Virhe-kuntotietojen, jossa ääretön TimeToLive tietoja. Valitse kyselyn sovelluksen kuntotietojen, mitkä palauttaa koostetun kunto tilan virheet ja terveyteen tapahtumaluettelon raportoidun kunto-tapahtumat.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Kuntotietojen mallin käyttö
Kunto-mallin avulla pilvipalveluihin ja pohjana palvelun kangasta ympäristö, jos haluat skaalata, koska seuranta- ja kuntotietojen määrityksen, jotka on jaettu eri näyttöjen klusterin sisällä.
Muista järjestelmistä on yksi, keskitetty palvelu klusterin tasolla, kaikki *mahdollisesti* hyödyllisiä tietoja palveluiden lähettämän jäsentää. Tämän menetelmän haitallinen vaikutus niiden skaalattavuus. Se myös ei he voivat kerätä tunnistaminen ja mahdollisten ongelmien kuin lähellä pääkansion syy mahdollisimman hyvin tiettyihin tietoihin.

Kunto mallia käytetään raskaasti seurantaa ja vianmäärityksen, arvioinnissa klusterin ja sovelluksen terveyden ja valvottu päivityksiä varten. Muiden palveluiden avulla kunto tietojen automaattinen korjaus, muodosta klusterin kunto historia ja antaa ilmoituksia tiettyjen ehtojen perusteella.

## <a name="next-steps"></a>Seuraavat vaiheet
[Palvelun kangasta kuntoraporttien tarkasteleminen](service-fabric-view-entities-aggregated-health.md)

[Käytä järjestelmän kuntoraportit vianmääritys](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Raportin ja tarkista palvelun kunto](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Lisää mukautettu palvelun kangasta kuntoraportit](service-fabric-report-health.md)

[Valvo ja vianmäärityksen paikallisesti palvelut](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Palvelun kangasta sovelluksen päivitys](service-fabric-application-upgrade.md)
