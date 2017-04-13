<properties
    pageTitle="Azure diagnostiikka vianmääritys"
    description="Ongelmien vianmääritys käytettäessä Azure diagnostiikka Azure Cloud Services-näennäiskoneiden ja "
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Azure diagnostiikka vianmääritys
Vianmääritystietoja kiinnostavat Azure Diagnostiikan avulla. Lisätietoja Azure Diagnostiikka on artikkelissa [Azure diagnostiikka yleiskatsaus](azure-diagnostics.md#cloud-services).

## <a name="azure-diagnostics-is-not-starting"></a>Azure diagnostiikka ei käynnisty

Diagnostiikan koostuu kahdesta osasta: Vieras agent-laajennus ja seurantaa agentti. Voit tarkistaa lokitiedostot **DiagnosticsPluginLauncher.log** ja **DiagnosticsPlugin.log** lisätietoja siitä, miksi diagnostiikka ei käynnisty.  
  
Pilvipalvelussa-roolin Vieras agentti laajennus lokitiedostot on ohjeaiheessa: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

Valitse Azure Virtual Machine-Vierailijan agentti laajennus lokitiedostot sijaitsevat: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
Lokitiedostojen viimeinen rivi sisältää lopetuskoodi.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

Laajennus palauttaa seuraavia lopetuskoodeja:

Lopetuskoodi|Kuvaus
---|---
0|Onnistui.
luku -1|Yleinen virhe.
-2|Rcf-tiedoston lataaminen ei onnistu.<p>Tämä on sisäinen virhe, vain tapahtuu, jos Vierailijan agentti laajennuksen avain on manuaalisesti kutsua, virheellisesti AM.
– 3|Ei voi ladata diagnostiikka määritystiedostoa.<p><p>Ratkaisu: Tämä on saatu määritystiedoston kulkeva ei rakenteen tarkistaminen. Ratkaisu on antamaan rakenteen kanssa, jotka ovat määritystiedostoa.
-4|Seurannan agentti diagnostiikka esiintymässä on käytössä paikallinen resurssi-kansio.<p><p>Ratkaisu: Määritä eri arvon **LocalResourceDirectory**.
-6|Vieras agentti laajennuksen avain yritetään käynnistää diagnostiikka Virheellinen komentorivin kanssa.<p><p>Tämä on sisäinen virhe, vain tapahtuu, jos Vieras agentti laajennuksen avain on manuaalisesti kutsua, virheellisesti AM.
-10|Diagnostiikka-laajennus päättyi käsittelemättömän poikkeuksen vuoksi.
-11|Vieras-agentti ei voi luoda prosessin vastuussa avaamista ja seurantaa agentti seuranta.<p><p>Ratkaisu: Varmista, että riittävästi järjestelmäresursseja ovat käytettävissä käynnistää uusien prosessien.<p>
-101|Virheelliset argumentit soitettaessa diagnostiikka-laajennus.<p><p>Tämä on sisäinen virhe, vain tapahtuu, jos Vieras agentti laajennuksen avain on manuaalisesti kutsua, virheellisesti AM.
-102|Laajennus on itse alustus epäonnistui.<p><p>Ratkaisu: Varmista, että riittävästi järjestelmäresursseja ovat käytettävissä käynnistää uusien prosessien.
-103|Laajennus on itse alustus epäonnistui. Erityisesti ei voi luoda lokin objekti.<p><p>Ratkaisu: Varmista, että riittävästi järjestelmäresursseja ovat käytettävissä käynnistää uusien prosessien.
-104|Vieras-agentti myöntämä rcf tiedoston lataaminen ei onnistu.<p><p>Tämä on sisäinen virhe, vain tapahtuu, jos Vierailijan agentti laajennuksen avain on manuaalisesti kutsua, virheellisesti AM.
-105|Diagnostiikka-laajennus ei voi avata diagnostiikka-kokoonpanotiedosto.<p><p>Tämä on sisäinen virhe, vain tapahtuu, jos diagnostiikka-laajennus on manuaalisesti kutsua, virheellisesti AM.
-106|Diagnostiikka-kokoonpanotiedosto ei voi lukea.<p><p>Ratkaisu: Tämä on saatu määritystiedoston kulkeva ei rakenteen tarkistaminen. Ratkaisu on niin antamaan rakenteen kanssa, jotka ovat määritystiedostoa. Voit etsiä XML, joka on AM kansio- *%SystemDrive%\WindowsAzure\Config* diagnostiikka-tunniste. Avaa haluamasi XML-tiedosto ja Etsi **Microsoft.Azure.Diagnostics**ja sitten **xmlCfg** -kenttään. Tiedot salataan base64, joten sinun on [purkaa sen](http://www.bing.com/search?q=base64+decoder) nähdäksesi, joka on ladattu diagnostiikka XML.<p>
-107|Resurssin directory pass seurantaa Agent on virheellinen.<p><p>Tämä on sisäinen virhe, vain tapahtuu, jos seurantaa agentti on manuaalisesti kutsua, virheellisesti AM.</p>
-108    |Ei voi muuntaa seurantaa agent-kokoonpanotiedosto diagnostiikka-kokoonpanotiedosto.<p><p>Tämä on sisäinen virhe, vain tapahtuu, jos diagnostiikka-laajennus käynnistetään manuaalisesti virheellisen määritystiedoston kanssa.
-110|Yleinen diagnostiikka kokoonpanovirhe.<p><p>Tämä on sisäinen virhe, vain tapahtuu, jos diagnostiikka-laajennus käynnistetään manuaalisesti virheellisen määritystiedoston kanssa.
-111|Käynnistä seuranta-agentti ei onnistu.<p><p>Ratkaisu: Varmista, että riittävät järjestelmäresurssit ovat käytettävissä.
-112|Yleinen virhe


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnostiikan tiedot ovat ole kirjautunut Azuren tallennustilaan
Azure diagnostiikka tallentaa kaikki tiedot Azuren tallennustilaan.

Yleisimmät syy puuttuvat tapahtumatiedot on virheellisesti määritettyjä tallennustilan tilin tiedot.

Ratkaisu: Korjaa diagnostiikka-kokoonpanotiedosto ja asennettava uudelleen Diagnostiikka.
Jos ongelma jatkuu asennettuasi diagnostiikka-tunniste uudelleen sitten virheenkorjaus on ehkä edelleen katsomalla mitään seuranta agentti virheitä. Ennen tapahtumatietoja tuodaan tallennustilan tilin, se tallennetaan LocalResourceDirectory.

Cloud palvelun rooli LocalResourceDirectory on:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

Näennäiskoneiden LocalResourceDirectory on:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Jos LocalResourceDirectory-kansiossa ei ole tiedostoja, seuranta-agentti ei voi käynnistää. Syynä on yleensä virheellisen määritystiedoston, tapahtuma, joka on ilmoitettava CommandExecution.log.

Jos seuranta-agentti on onnistuneesti tapahtuman tietojen keräämisen näet kuhunkin tapahtumaan määritetty määritystiedostossa .tsf tiedostot. Seuranta-agentti kirjaa sen virheitä tiedoston MaEventTable.tsf. Voit tarkastaa tämän tiedoston sisällön .tsf-tiedoston muuntaminen Luetteloerottimella erotetut values(.csv) tiedoston tabel2csv-sovelluksen:

Cloud palvelun roolin:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

Cloud palvelun rooliin *%SYSTEMDRIVE%\Program* on yleensä D:

Virtual tietokoneessa:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

Edellä olevat komennot Luo loki tiedoston *maeventtable.csv*, jonka voit avata ja virheen viestien tarkistaminen.    


## <a name="diagnostics-data-tables-not-found"></a>Diagnostiikan tietojen taulukot ei löydy
Azure tallennustilan Azure diagnostiikka tietoja taulukot nimetään käyttämällä seuraava koodi:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Tässä on esimerkki:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

Joka tuottaa 4 taulukot:

Tapahtuman|Taulukkonimi
---|---
palvelun = "prov1" &lt;tapahtumatunnus = "1" /&gt;|WADEvent + MD5("prov1") + "1"
palvelun = "prov1" &lt;tapahtumatunnus = "2" eventDestination = "dest1" /&gt;|WADdest1
palvelun = "prov1" &lt;DefaultEvents /&gt;|WADDefault+MD5("prov1")
palvelun = "prov2" &lt;DefaultEvents eventDestination = "dest2" /&gt;|WADdest2
