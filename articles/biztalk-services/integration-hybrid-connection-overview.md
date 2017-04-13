<properties
    pageTitle="Hybrid Tietoyhteyksien yleiskatsaus | Microsoft Azure"
    description="Lisätietoja Hybrid yhteydet, suojaus, TCP-portit ja tuettujen määrityksiä. MAB-WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>Hybrid Tietoyhteyksien yleiskatsaus
Johdanto Hybrid yhteydet luettelo tuetuista määritykset ja luettelo tarvittavat TCP-portit.


## <a name="what-is-a-hybrid-connection"></a>Hybrid yhteyden ominaisuudet

Hybrid yhteydet ovat Azure BizTalk palvelujen ominaisuus. Hybrid yhteydet on helppoa ja kätevä tapa Azure App palvelussa (aiemmin sivustoja) Web Apps-ominaisuus ja Azure App palvelussa (aiemmin Mobile Services) mobiilisovellukset-toiminto muodostaa paikallisen resurssien palomuurin takana.

![Hybrid yhteydet][HCImage]

Hybrid yhteydet etuja:

- Web Apps-sovellusten ja Mobile-sovellukset voivat käyttää aiemmin paikalliset tiedot ja palveluiden turvallisesti.
- Useita Web Apps-sovellusten tai Mobile-sovellukset voivat jakaa Hybrid yhteyden käyttämään paikallista resurssia.
- Verkon käytön myös tarvitaan mahdollisimman vähän TCP-portit.
- Sovellusten Hybrid yhteydet käyttää vain, jotka on julkaistu tietyn paikallisen resurssin Hybrid-yhteyden kautta.
- Voit muodostaa yhteyden mitä tahansa paikallista resurssia, joka käyttää staattinen porttinumeroa, kuten SQL Server, MySQL, HTTP-verkko-ohjelmointirajapinnan ja useimmat mukautetun verkkopalvelut.

    > [AZURE.NOTE] TCP-pohjaisia palveluja, jotka käyttävät dynaamiset portit (esimerkiksi FTP Passiivinen tila tai laajennettu Passiivinen tila) on tällä hetkellä tueta. LDAP myös ei tueta. LDAP käyttää staattinen porttinumeroa, mutta sitä voi käyttää myös UDP. Tämän vuoksi sitä ei tueta.

- Voidaan käyttää kaikkien kehysten tukemat Web Apps (.NET, PHP, Java, Python, Node.js) ja Mobile-sovellusten (Node.js, .NET).
- Web-sovellusten ja Mobile-sovellusten voi käyttää paikallisen resursseja täsmälleen samalla tavalla ikään kuin paikalliseen verkkoon kirjauduttaessa sijaitsee verkossa tai Mobile-sovelluksesta. Esimerkiksi sama yhteyden merkkijono, jota käytetään paikallisen voi käyttää myös Azure.


Hybrid yhteydet tarjoavat myös ohjausobjekti ja käytä hybrid-sovellukset, myös yritysresurssit näkyvyys yrityksen Järjestelmänvalvojat:

- Käyttäminen ryhmäkäytäntöasetukset järjestelmänvalvojat voivat määrittää myös resurssit, jotka voivat käyttää hybrid sovellukset ja sitä on sallivat Hybrid verkossa.
- Tapahtuma- ja valvonta lokit yrityksen verkossa on näkyvyys Hybrid yhteydet Käytä resursseja.


## <a name="example-scenarios"></a>Esimerkkejä eri tilanteista

Hybrid yhteyksiä tukevat seuraavia framework ja sovelluksen yhdistelmiä:

- .NET framework access SQL Server-tietokantaan
- .NET framework pääsy WebClient-palvelun HTTP/HTTPS
- SQL Server-MySQL PHP pääsy
- SQL Server, MySQL ja Oracle Java käyttö
- Java HTTP/HTTPS-palveluiden käyttäminen

Kun käyttäminen Hybrid yhteydet paikallisen SQL Server-, toimi seuraavasti:

- SQL Express nimeltä esiintymät on määritettävä käyttämään staattista portit. Oletusarvon mukaan SQL Express nimi esiintymät Käytä dynaamiset portit.
- SQL Express oletus esiintymiä käytetään staattinen portti, mutta TCP on otettava käyttöön. Oletusarvon mukaan TCP ei ole käytössä.
- Käytettäessä Clustering tai ryhmien käytettävyys `MultiSubnetFailover=true` tilaa ei tueta.
- `ApplicationIntent=ReadOnly` Ei tällä hetkellä tueta.
- SQL-todennusta voidaan käyttää lopusta loppuun todennusmenetelmän Azure sovelluksen ja paikallisen SQL server tukevat.


## <a name="security-and-ports"></a>Tietoturva ja portit

Hybrid yhteydet käyttää jaettu Access allekirjoitus (SAS) luvan suojaamiseen Hybrid yhteyden Azure sovellukset ja paikallisen Hybrid Yhteyksienhallinnan yhteydet. Erillisen yhteyden näppäimet luodaan ja paikallisen Hybrid Yhteyksienhallinnan. Yhteyden painettavat näppäimet on koottu ja kumottu itsenäisesti.

Hybrid yhteydet avulla sovellukset ja paikallisen Hybrid Yhteyksienhallinnan näppäimet saumattomasti ja turvallisesti jakautumisen.

Katso [Hybrid tietoyhteyksien luominen ja hallitseminen](integration-hybrid-connection-create-manage.md).

*Sovelluksen todennus on erillinen Hybrid yhteys*. Kaikki tarvittavat todennusmenetelmän voidaan. Todennus-menetelmä määräytyy lopusta loppuun-todennus menetelmiä tuettu Azure pilven ja paikallisen osien välillä. Esimerkiksi Azure sovelluksen noutaa paikallisen SQL Serveriä. Tässä skenaariossa SQL-todennus voi olla todennusmenetelmän, joka on tuettu lopusta loppuun.

#### <a name="tcp-ports"></a>TCP-portit
Hybrid yhteydet tarvitsevat vain lähtevän TCP- tai HTTP-yhteyden yksityinen verkosta. Sinun ei tarvitse avata minkä tahansa palomuurin porttien tai muuttaa verkon ympäröivän kokoonpano, jotta kaikki saapuvat connectivity mukaan verkostoon.

Hybrid yhteydet käyttää TCP-portit:

Port (portti) | Miksi sitä tarvitaan
--- | ---
9350 - 9354 | Tiedonsiirto käytetään seuraavat portit. Palvelun Bus välitys hallinnan probes portin 9350 määrittämään TCP-yhteys on käytettävissä. Jos se on käytettävissä, valitse se olettaa, että port 9352 myös käytettävissä. Tietoliikennettä esitellään portin 9352. <br/><br/>Salli lähtevät yhteydet seuraavat portit.
5671 | Portti 9352 käytettäessä tietoliikennettä portin 5671 käytetään ohjausobjektin kanava. <br/><br/>Salli lähtevät yhteydet tähän porttiin.
80, 443 | Porttien käytetään Azure joidenkin tietojen pyynnöt. Jos portit 9352 ja 5671 eivät ole käytettävissä, *Valitse* portit 80 ja 443 ovat myös varakyselyjen portit, joita käytetään tiedonsiirto ja ohjausobjektin kanava.<br/><br/>Salli lähtevät yhteydet seuraavat portit. <br/><br/>**Huomautus** On ei ole suositeltavaa käyttää näiden varakyselyjen portit sijasta TCP-portit. HTTP/WebSocket käytetään tietojen kanavien sijaan alkuperäisen TCP-protokolla. Saattaa johtaa alemman suorituskykyä.



## <a name="next-steps"></a>Seuraavat vaiheet

[Hybrid tietoyhteyksien luominen ja hallitseminen](integration-hybrid-connection-create-manage.md)<br/>
[Azure verkkosovelluksissa yhdistäminen paikallisen resurssi](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Yhteyden muodostaminen paikalliseen SQL Server Azure web app-sovelluksessa](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>Katso myös

[REST API BizTalk palveluiden Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk Services: versiot kaavion](biztalk-editions-feature-chart.md)<br/>
[Luo BizTalk palvelu Azure-portaalissa](biztalk-provision-services.md)<br/>
[BizTalk Services: Raporttinäkymät-ikkunan, näyttö ja skaalaus-välilehdet](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
