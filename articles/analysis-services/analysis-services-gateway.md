<properties
   pageTitle="Paikallisen tietojen yhdyskäytävän | Microsoft Azure"
   description="Paikallisen--yhdyskäytävä on tarpeen, jos Analysis Services-palvelimen Azure-tietokannassa muodostaa yhteyden paikallisen tietolähteisiin."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="on-premises-data-gateway"></a>Paikallisen tietojen yhdyskäytävän

Paikallisen tietojen yhdyskäytävän toimii sillaksi tarjoavat tietojen suojaaminen siirtäminen paikalliseen tietolähteet ja pilveen Azure Analysis Services-palvelimen välillä.

Verkossa olevaan tietokoneeseen on asennettu yhdyskäytävä. Yhden yhdyskäytävän on oltava asennettuna kunkin Azure Analysis Services-palvelimen Azure-tilauksesi on. Esimerkiksi jos sinulla on kaksi palvelinten Azure-tilaukseesi, muodosta yhteys paikalliseen tietolähteisiin, yhdyskäytävä on oltava asennettuna kaksi eri tietokoneissa verkossa.

## <a name="requirements"></a>Vaatimukset

**Vähimmäisvaatimukset:**

- .NET 4.5 framework
- 64-bittinen Windows 7 ja Windows Server 2008 R2 (tai uudempi)

**Suositus:**

- 8 core Suoritin
- 8 Gigatavun muistia
- 64-bittinen versio Windows 2012 R2: n (tai uudempi)

**Tärkeät seikat:**

- Yhdyskäytävää ei voi asentaa toimialueen ohjauskoneen.

- Vain yhden yhdyskäytävän voidaan asentaa samassa tietokoneessa.

- Asenna tietokoneeseen, jossa on säilyy, ja siirry lepotilaan yhdyskäytävän. Jos tietokoneessa ei ole, Azure Analysis Services-palvelimen pysty muodostamaan yhteyttä paikalliseen tietolähteet tietojen päivittäminen.

- Älä asenna yhdyskäytävän tietokoneen langatonta yhteydessä verkkoon. Suorituskyky voi heiketä.

- Muuttaa SMTP-palvelimen nimi, joka on jo määritetty yhdyskäytävän, joudut Asenna ja määritä uusi yhdyskäytävä.

- Joissakin tapauksissa muodostamisesta tietolähteiden alkuperäisen tarjoajien, kuten SQL Server Native Client (SQLNCLI11) Taulukkomalleissa voi palauttaa virheen. Lisätietoja on aiheessa [tietolähteen yhteys](analysis-services-datasource.md).

## <a name="supported-on-premises-data-sources"></a>Tuetut paikallisen tietolähteet
Esikatselun yhdyskäytävän tukee Azure Analysis Services-palvelimen ja paikallisen tietolähteiden väliset yhteydet:

- SQL Server
- SQL-tietovarasto
- ‑SOVELLUKSET
- Oracle
- Teradata


## <a name="download"></a>Lataa
 [Lataa yhdyskäytävän](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>Asentaminen ja määrittäminen

1. Suorittamalla asennusohjelma.

2. Asennuksen sijainnin valitseminen ja hyväksy käyttöoikeussopimus.

3. Azure kirjautuminen.

4. Määritä Azure Analysis-palvelimen nimen selvittäminen. Voit määrittää vain yhden palvelimen yhdyskäytävän kohden. Valitse **Määritä** ja on hyvä huomioida missä.

    ![azure kirjautuminen](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>Toiminta
Yhdyskäytävän suoritetaan palveluna Windows- **paikallisen tietojen yhdyskäytävän**organisaation verkossa olevaan tietokoneeseen. Asenna Azure Analysis Servicesin kanssa käytettäväksi yhdyskäytävän perustuu samassa yhdyskäytävässä käyttää muita Power BI-palveluita varten, mutta eroja siitä, miten se on määritetty.

![Toiminta](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Kyselyiden ja tietojen kulun työ tältä:

1.  Kyselyssä on luonut pilvipalvelussa paikallisen tietolähteen salattuja tunnuksilla. Se lähetetään sitten käsittelemään yhdyskäytävän jonon.

2.  Yhdyskäytävän pilvipalvelussa analysoi kyselyn, ja Vie pyynnön [Azure palvelun Bus](https://azure.microsoft.com/documentation/services/service-bus/).

3.  Paikallisen tietojen yhdyskäytävän tekee kyselyn Azure palvelun Bus varten pyyntöihin.

4.  Yhdyskäytävän saa kyselyn, purkaa tunnistetietojen ja yhdistää tietolähteiden tunnistetiedot kanssa.

5.  Yhdyskäytävän lähettää kyselyn suorittamista varten tietolähteeseen.

6.  Tulokset lähetetään tietolähteestä, takaisin yhdyskäytävän ja aina pilvipalvelussa sijaitsevaan.

## <a name="windows-service-account"></a>Windows-palvelun tili

Paikallisen tietojen yhdyskäytävä on määritetty käyttämään Windows-palvelun kirjautumisen tunnistetiedon *NT SERVICE\PBIEgwService* . Oletusarvon mukaan se on oikealla puolella kirjautuminen palveluna; tietokone, oletko asentamassa yhdyskäytävän kontekstissa. Tämä tunnistetiedon ei ole sama tili, jota käytetään paikallisen tietolähteisiin yhdistäminen tai Azure-tili.  

Jos kohtaat ongelmia välityspalvelimen todennus vuoksi kanssa, haluat ehkä muuttaa Windows-palvelutili toimialuekäyttäjä tai hallitun palvelutilin.

## <a name="ports"></a>Portit

Yhdyskäytävän Luo Azure palvelun Bus lähtevä yhteys. Lähtevien porttiin yhteydessä: TCP 443 (oletusarvo), 5671, 5672, 9350 9354 kautta.  Yhdyskäytävää ei edellytä saapuvien porttien.

Suositeltavaa on whitelist tietojen alueesi palomuurissa IP-osoitteet. Voit ladata [Microsoft Azure palvelinkeskuksen IP-luettelosta](https://www.microsoft.com/download/details.aspx?id=41653). Tämä luettelo päivitetään viikoittain.

> [AZURE.NOTE]  IP-osoitteiden Azure palvelinkeskuksen IP-luettelossa ovat CIDR merkintätapaa. Esimerkiksi 10.0.0.0/24 ei tarkoita 10.0.0.0 10.0.0.24 kautta. Lisätietoja [CIDR merkintätapaa](http://whatismyipaddress.com/cidr).

Seuraavassa taulukossa on yhdyskäytävän käyttämä täydelliset toimialuenimet.

|Toimialueiden nimet|Lähtevien portti|Kuvaus|
|---|---|---|
|*. powerbi.com|80|HTTP käyttää ladattaessa asennusohjelma.|
|*. powerbi.com|443|HTTPS|
|*. analysis.windows.net|443|HTTPS|
|*. login.windows.net|443|HTTPS|
|*. servicebus.windows.net|5671 5672|Lisäasetukset sanomajonojärjestelmä Protocol (AMQP)|
|*. servicebus.windows.net|443, 9350 9354|Kuuntelijoita-palvelun Bus välitys TCP (edellyttää 443 käytönvalvonta suojaustunnuksen saamiseksi)-protokollaa|
|*. frontend.clouddatahub.net|443|HTTPS|
|*. core.windows.net|443|HTTPS|
|Login.microsoftonline.com|443|HTTPS|
|*. msftncsi.com|443|Testaa internet-yhteys, jos yhdyskäytävä ei ole käytettävissä Power BI-palvelun avulla.|
|*.microsoftonline p.com|443|Käyttää todennustavaksi asetuksista riippuen.|


### <a name="forcing-https-communication-with-azure-service-bus"></a>Pakottaa HTTPS Azuren palvelun Bus tietoliikenteen

Voit pakottaa yhdyskäytävän Azure-palvelu Bus käyttämällä suoraa TCP; sijaan HTTPS viestintään Näin voit vähentää huomattavasti vähentää suorituskykyä. Voit joutua muokkaamaan *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* -tiedosto. Muuttaa arvon `AutoDetect` , `Https`. Tämä tiedosto sijaitsee oletusarvoisesti osoitteessa *C:\Program Files\On paikallisten tietojen yhdyskäytävä*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Vianmääritys
Näytä lisäasetukset voidaan yhdistää Azure Analysis Services-ympäristöön tietolähteisiin paikallisen tietojen yhdyskäytävä on samassa yhdyskäytävässä käyttämisestä Power BI.

Jos sinulla on ongelmia, kun asennuksesta ja määrityksestä yhdyskäytävän, muista Katso [vianmääritys Power BI-yhdyskäytävä](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Jos ongelma on mukana, on palomuurin tai välityspalvelimen-kohdissa.

Jos arvelet, että olet encountering välityspalvelimen seurantakohteita yhdyskäytävää, jossa on artikkelissa [Power BI-yhdyskäytävien määrittäminen välityspalvelimen asetukset](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md).

## <a name="next-steps"></a>Seuraavat vaiheet
- [Analysis Services hallinta](analysis-services-manage.md)
- [Tietojen noutaminen Azure Analysis Servicesistä](analysis-services-connect.md)
