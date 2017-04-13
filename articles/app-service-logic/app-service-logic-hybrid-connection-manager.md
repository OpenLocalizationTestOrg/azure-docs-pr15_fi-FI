<properties 
    pageTitle="Hybrid yhteyden hallinnan avulla | Microsoft Azure" 
    description="Asentaminen ja määrittäminen Hybrid Yhteyksienhallinnan ja muodosta yhteys paikalliseen yhdistimet logiikan-sovelluksissa" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>Yhteyden muodostaminen paikalliseen yhdistimet Hybrid yhteyden hallinnan avulla

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2014 – 12-01 – esikatselu rakenteen versio. Logiikan sovellusten yleiseen käyttöön (GA) käyttää yhdyskäytävän paikallisen yhteys. Lue lisää uusi [Yhdyskäytävä](app-service-logic-gateway-connection.md) - ja [Logiikka sovellusten GA](https://azure.microsoft.com/documentation/services/logic-apps/).

Logiikan sovellusten käyttää käyttämään paikallista järjestelmän Hybrid Yhteyksienhallinnan. Joidenkin yhdistimien muodostaa paikalliseen järjestelmään, kuten SQL Server, SAP, SharePoint ja niin edelleen. 

Hybrid yhteyden Manager (HCM) on napsautuksen-kerran asennusohjelman, joka on asennettu verkossa, palomuurin takana oleville IIS-palvelimessa. Käytä Azure palvelun Bus välitys, HCM todentaa paikalliseen järjestelmään Connectorilla Azure-tietokannassa. 

> [AZURE.NOTE] Hybrid Yhteyksienhallinnan vaaditaan vain, jos olet muodostamassa yhteyttä paikalliseen resurssin palomuurin takana. Jos olet muodostamassa ei paikalliseen järjestelmään, sinun ei tarvitse Hybrid Yhteyksienhallinnan.

Aluksi sinun on:

- Azure palvelun Bus välitys nimitilan SAS yhteysmerkkijono. Katso, voit selvittää, mitkä taso sisältää releitä [Palvelun Bus hinnoittelua](https://azure.microsoft.com/pricing/details/service-bus/) .
- Paikallisen järjestelmän sisäänkirjautumisongelmien tiedot, kuten käyttäjänimi ja salasana. Esimerkiksi jos olet muodostamassa paikallisen SQL Server-palvelimeen, sinun on kirjautuminen SQL Server-tili ja salasana.
- Paikallisen palvelimen tiedot, mukaan lukien portin numeroa ja palvelimen nimi. Esimerkiksi jos olet muodostamassa paikallisen SQL Server-palvelimeen, sinun on SQL-palvelimen nimen ja TCP-porttinumero.

## <a name="get-the-service-bus-connection-string"></a>Palvelun Bus tietoyhteyden yhteysmerkkijonon

Kopioi palvelun Bus pääkansio SAS yhteysmerkkijonon Azure-portaalissa. Tämän yhteysmerkkijonon muodostaa Azure yhdistimen paikalliseen järjestelmään. 

1. [Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)palvelun Bus nimitilan Valitse ja valitse **Yhteyden tietoja**:

    ![][SB_ConnectInfo]

2. Kopioi SAS yhteysmerkkijonon:

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>Asenna Hybrid Yhteyksienhallinnan

1. [Azure-portaalissa](http://go.microsoft.com/fwlink/p/?LinkID=525040)Valitse luomasi yhdistin. Voit avata sen valitsemalla **Selaa**, **API**-sovellukset ja valitse yhdistin tai API-sovelluksessa. 
<br/><br/>
**Hybrid yhteyden**ei **ole täydellinen**asennus:
<br/>
![][2] 

2. Valitse **Hybrid yhteys**. Aiemmin määritetty palvelun Bus yhteysmerkkijonon näkyy.
3. Kopioi **ensisijainen määritys-merkkijono**.
<br/>
![][PrimaryConfigString]

4. **Paikallisen Hybrid Yhteyksienhallinnan**voit ladata Hybrid yhteyden Manager tai asentaa sen suoraan portaalin. 
<br/><br/>
Asenna suoraan portaalin, siirry paikallisen IIS-palvelimeen, Selaa portaaliin ja valitse **Lataa ja määritä**.
<br/><br/>
Lataa Hybrid Yhteyksienhallinnan, siirry paikallisen IIS-palvelin ja siirry **ClickOnce sovelluksen** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application). Asennus käynnistyy automaattisesti, jotta voit suorittaa sen.

5. **Listener asetukset** -ikkunassa **Ensisijainen määritysten merkkijonon** liittämäsi aiemmin (vaiheessa 3) ja valitse **Asenna**.

Kun asennus on valmis, seuraavat näyttää:
<br/>
![][3] 

Kun selaat Connectorin uudelleen, hybrid yhteyden tila on nyt **yhdistetty**. Saatat joutua Sulje connector ja avaat sen uudelleen:
<br/>
![][4] 

> [AZURE.NOTE] Siirry toissijaiseen yhteysmerkkijonon asennusohjelman Hybrid yhteyden uudelleen ja kirjoita **Toissijainen määritys-merkkijono**.


## <a name="tcp-ports-and-security"></a>TCP-portit ja suojaus

Kun luot yhteyden hybrid, sivusto luodaan paikallisen paikallisen IIS-palvelimeen. IIS-palvelin voi olla DMZ. IIS-palvelimessa TCP Port (portti)-vaatimukset ovat seuraavat:

TCP-portti | Miksi
--- | ---
 | Ei ole saapuvien TCP-portit tarvitaan.
9350 - 9354 | Tiedonsiirto käytetään seuraavat portit. Palvelun Bus välitys hallinnan probes portin 9350 määrittämään TCP-yhteys on käytettävissä. Jos se on käytettävissä, valitse se olettaa, että port 9352 myös käytettävissä. Tietoliikennettä esitellään portin 9352. <br/><br/>Salli lähtevät yhteydet seuraavat portit.
5671 | Portti 9352 käytettäessä tietoliikennettä portin 5671 käytetään-ohjausobjektin kanava. <br/><br/>Salli lähtevät yhteydet tähän porttiin. 
80, 443 | Jos portit 9352 ja 5671 eivät ole käytettävissä, *Valitse* portit 80 ja 443 on varakyselyjen portit, joita käytetään tiedonsiirto ja ohjausobjektin kanava.<br/><br/>Salli lähtevät yhteydet seuraavat portit.
Paikallinen järjestelmä portti | Avaa järjestelmän käyttämä portti paikalliseen järjestelmään. Esimerkiksi SQL Server käyttää yleensä porttia 1433. Avaa tämä porttinumeroa.

[Palvelun Bus kanssa palomuurin takana isännöinnin](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>Vianmääritys

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>Paikallisen vianmääritys

1. Vahvista IIS-palvelimen IIS-web-rooli on asennettu ja IIS-palvelut on käynnistetty.
2. IIS-palvelimessa Vahvista Hybrid Yhteyksienhallinnan on asennettu ja käytössä:
 - IIS hallinta (inetmgr), ***MicrosoftAzureBizTalkHybridListener*** sivuston luettelossa pitäisi näkyä ja käytössä. 
 - Tämä sivusto käytetään ***HybridListenerAppPool*** , joka suoritetaan *NetworkService* valmiin paikallista tilinä. Tämä AppPool myös voidaan aloittaa.
3. IIS-palvelimessa Vahvista yhdistin on asennettu ja käytössä: 
 - Sivusto on luotu sitten yhdistimen. Esimerkiksi jos olet luonut SQL-yhdistin, ei ***MicrosoftSqlConnector_nnn*** -sivustossa. IIS hallinta (inetmgr), tarkista tämän sivuston on luettelossa ja käytön aloittaminen. 
 - Tämä sivusto käytetään omaa IIS-sovellussarjan nimeltä ***HybridAppPoolnnn***. Tämä AppPool suoritetaan *NetworkService* valmiin paikallista tilinä. Tämä sivusto ja AppPool sekä käynnistetään. 
 - Etsi paikallinen yhdistin. Esimerkiksi jos yhdistimen sivuston käyttää porttia 6569, siirry http://localhost:6569. Oletusarvo-asiakirjaan ei ole määritetty siten `HTTP Error 403.14 - Forbidden error` odotetaan.
4. Vahvista palomuurin, tässä aiheessa lueteltuja TCP-portit ovat avoinna.
5. Tarkista lähde- tai järjestelmä:
 - Jotkin paikallisen edellyttää muita riippuvuuden tiedostot. Esimerkiksi jos olet muodostamassa yhteyttä paikalliseen SAP-joitakin muita SAP-tiedostojen on oltava asennettuna IIS-palvelimessa.
 - Tarkista yhteys kirjautumistili järjestelmän. Esimerkiksi porttinumeroa, järjestelmä käyttää on oltava avattuna, kuten SQL Server-portin 1433. Azure-portaalissa kirjoittamasi kirjautumistili on oltava pääsyn.
6. Tarkista virheitä tapahtumalokit IIS-palvelimeen. 
7. Uudelleenjärjestäminen ja asenna se uudelleen Hybrid Yhteyksienhallinnan: 
 - IIS-Poista manuaalisesti connector-sivustossa ja sen sovellussarjan. 
 - Suorita Hybrid Yhteyksienhallinnan ja vahvista lisäät oikean **Ensisijainen määritysten merkkijonon** sitten yhdistimen.



### <a name="in-the-azure-classic-portal"></a>Azure perinteinen-portaalissa

1. Vahvista palvelun Bus nimitila on **aktiivinen** .
2. Kun luot yhdistimen, kirjoita palvelun Bus SAS yhteysmerkkijono. Älä kirjoita ACS yhteysmerkkijonon.


## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET

**KYSYMYS**: on kaksi Hybrid yhteyden valvojat. Mitä eroa on? 

**Vastaus**: ei [Hybrid yhteydet](../biztalk-services/integration-hybrid-connection-overview.md) -tekniikkaa, jota käytetään yhteyden muodostaminen paikalliseen myönteiseen Web Apps (aiemmalta nimeltään sivustoja) ja Mobile-sovellusten (aiemmalta nimeltään mobile palvelut) mukaan. Tämä Hybrid yhteyksien hallinta on omat [asetukset](../biztalk-services/integration-hybrid-connection-create-manage.md) ja käyttää Azure BizTalk-palvelun (taustalla). Se tukee vain TCP- ja HTTP-protokollaa.

Azure-sovelluksen palvelun yhdistimillä Meillä on myös Hybrid Yhteyksienhallinnan.  Onko tämä Hybrid Yhteyksienhallinnan *ei* Käytä Azure BizTalk Service (taustalla) ja tukee yli TCP- ja HTTP-protokollaa. Katso [yhdistimien ja API Sovellusluettelossa](app-service-logic-connectors-list.md).

Molemmat yhteys paikalliseen järjestelmään Bus Azure-palvelun avulla.

**KYSYMYS**: kun luoda mukautetun API-sovelluksen, Voinko käyttää sovelluksen palvelun Hybrid Yhteyksienhallinnan Muodosta yhteys paikalliseen? 

**Vastaus**: ei perinteinen mielessä. Voit käyttää valmiita yhdistin, Määritä sovellus palvelun Hybrid-Yhteyksienhallinnan muodostaa paikalliseen järjestelmään. Tämä yhdistin käyttäminen sitten mukautetun API-sovelluksen, mahdollisesti logiikan sovellusta käytettäessä. Ei voi tällä hetkellä kehittää tai Luo omia hybrid API-sovellus (kuten SQL-yhdistin tai tiedoston yhdistin).

Jos mukautetun API käytetään TCP- tai HTTP-portin, voit käyttää [Hybrid yhteydet](../biztalk-services/integration-hybrid-connection-overview.md) ja sen Hybrid Yhteyksienhallinnan. Tässä skenaariossa Azure-BizTalk palvelua käytetään. [Yhteyden muodostaminen paikalliseen SQL Server web-sovelluksen](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) avulla.  


## <a name="read-more"></a>Lisätietoja on artikkelissa

[Logiikan sovellusten valvonta](app-service-logic-monitor-your-logic-apps.md)<br/>
[Palvelun Bus hinnat](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
