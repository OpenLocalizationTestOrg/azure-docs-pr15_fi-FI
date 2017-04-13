<properties 
    pageTitle="Voit luoda ja hallita Hybrid yhteydet | Microsoft Azure" 
    description="Opettele hybrid yhteyden luominen, hallinta yhteys ja asenna Hybrid Yhteyksienhallinnan. MAB-WABS" 
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
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Hybrid tietoyhteyksien luominen ja hallitseminen


## <a name="overview-of-the-steps"></a>Yleiset ohjeet
1. Hybrid yhteyden kirjoittamalla **isäntänimi** tai **FQDN** paikallisen resurssin yksityinen verkossa.
2. Linkki Azure verkkosovelluksissa tai Azure mobiilisovellukset Hybrid yhteys.
3. Asenna Hybrid Yhteyksienhallinnan paikallisen resurssin ja tietyn Hybrid yhteyden. Azure-portaalissa on yhdellä napsautuksella kokemus, asenna ja Yhdistä.
4. Hallitse Hybrid yhteydet ja niiden yhteys avaimet.

Tässä artikkelissa on lueteltu seuraavasti. 

> [AZURE.IMPORTANT] On mahdollista määrittää Hybrid yhteyden endpoint IP-osoitteeseen. Jos käytät IP-osoite, voivat tai saattaa ulotu paikallisen resurssin asiakkaan mukaan. Hybrid yhteyden määräytyy asiakkaan tekevät DNS-haku. Useimmissa tapauksissa __asiakkaan__ on sovelluksen-koodin. Jos asiakas ei suorita DNS-haku (se ei ole yrittää ratkaista IP-osoitetta, jos sen toimialuenimi (x.x.x.x)), valitse ei lähetetä Hybrid-yhteyden kautta.
>
> Esimerkiksi (pseudocode) Määritä **10.4.5.6** paikallisen isännän:
> 
> **Toimii seuraavassa tapauksessa:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Seuraava tilanne ei toimi:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Hybrid yhteyden luominen

Hybrid yhteyden voi luoda Azure-portaalissa Web Apps-sovellusten avulla **tai** BizTalk-palveluiden avulla. 

**Hybrid yhteyksien Web Apps-sovellusten luomiseen**, on artikkelissa [Yhteyden Azure Web Apps edelleen paikallisen resurssiin](../app-service-web/web-sites-hybrid-connection-get-started.md). Voit myös asentaa web Appista, joka on suositeltavampaa Hybrid yhteyden Manager (HCM). 

**Voit luoda Hybrid yhteydet BizTalk Services-palveluissa**:

1. Kirjautuminen [Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Valitse vasemmassa siirtymisruudussa **BizTalk** palvelut, ja valitse sitten BizTalk-palvelussa. 

    Jos sinulla ei ole olemassa olevan BizTalk-palvelun, voit [Luo BizTalk palvelu](biztalk-provision-services.md).
3. Valitse **Hybrid yhteydet** -välilehdessä:  
![Hybrid yhteydet-välilehti][HybridConnectionTab]

4. Valitse **Luo Hybrid yhteys** tai valitse **Lisää** -painike tehtäväpalkissa. Anna seuraavat tiedot:

    Ominaisuus | Kuvaus
--- | ---
Nimi | Hybrid yhteyden nimi on oltava yksilöllinen, eikä niitä voi sama nimi kuin BizTalk-palvelun. Voit kirjoittaa haluamasi nimen, mutta ole täsmällinen tarkoitustaan kanssa. Esimerkkejä:<br/><br/>Palkanlaskennan*SQL Server*<br/>SupplyList*SharepointServer*<br/>Asiakkaiden*OracleServer*
Isäntänimi | Kirjoita täydellinen isäntänimi vain isäntänimi tai paikallisen resurssin IPv4-osoite. Esimerkkejä:<br/><br/>mySQLServer<br/>*mySQLServer*. *Toimialueen*. Corporation*yourCompany*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *yourCompany*.com<br/>10.100.10.10<br/><br/>Jos käytät IPv4-osoite, Huomaa, että asiakasohjelman tai sovelluksen koodi ei ratkaise IP-osoite. Katso tärkeä huomautus: tämän ohjeaiheen yläreunassa.
Port (portti) | Kirjoita paikallisen resurssin porttinumero. Esimerkiksi jos käytät Web Apps-sovelluksista, kirjoita portti 80 tai porttiin 443. Jos käytät SQL Server, kirjoita porttia 1433.

5. Valitse Viimeistele määritykset valintamerkkiä. 

#### <a name="additional"></a>Lisätietoja

- Voit luoda useita Hybrid yhteyksiä. Katso [BizTalk Services: versiot kaavion](biztalk-editions-feature-chart.md) yhteyksien määrä. 
- Kukin Hybrid yhteys luodaan yhteysmerkkijonon kahdet: sovelluksen nuolinäppäimiä, lähetys- ja paikallisen näppäimet, jotka KUUNTELEVAT. Kunkin on ensisijainen ja toissijainen avain. 


## <a name="LinkWebSite"></a>Linkki Azure App palvelun Web App-sovelluksen tai mobiilisovelluksessa

Linkin verkkosovellukseen tai Mobile-sovelluksen Azure-sovelluksen palvelun Hybrid-yhteyden, valitse Hybrid yhteydet-sivu **Käytä aiemmin luotua Hybrid-yhteyttä** . Katso [Access paikallisen resurssien hybrid yhteyksien Azure sovelluksen-palvelun avulla](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Asenna Hybrid Yhteyksienhallinnan paikallisen

Kun Hybrid-yhteys on luotu, asenna Hybrid Yhteyksienhallinnan paikallisen resurssin. Voit ladata Azure verkkosovelluksissa tai BizTalk-palvelusta. BizTalk Services vaiheet: 

1. Kirjautuminen [Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Valitse vasemmassa siirtymisruudussa **BizTalk** palvelut, ja valitse sitten BizTalk-palvelussa. 
3. Valitse **Hybrid yhteydet** -välilehdessä:  
![Hybrid yhteydet-välilehti][HybridConnectionTab]
4. Valitse tehtäväpalkissa **Paikallisen asetukset**:  
![Paikallisen asetukset][HCOnPremSetup]
5. Valitse **Asenna ja määritä** suorittamaan tai Lataa Hybrid Yhteyksienhallinnan paikalliseen järjestelmään. 
6. Valitse Käynnistä asennus valintamerkkiä. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Lisätietoja
- Seuraavat käyttöjärjestelmät voidaan asentaa Hybrid Yhteyksienhallinnan:

    - Windows Server 2008 R2 (.NET Framework 4.5 + ja Windows Management Framework 4.0 + pakollinen)
    - Windows Server 2012 (Windows Management Framework 4.0 + pakollinen)
    - Windows Server 2012 R2


- Kun olet asentanut Hybrid Yhteyksienhallinnan, tapahtuu seuraavaa: 

    - Azure isännöimät Hybrid yhteyden on automaattisesti määritetty ensisijainen sovelluksen yhteysmerkkijonon. 
    - Edelleen paikallisen resurssin on automaattisesti määritetty käyttämään ensisijainen edelleen paikallisen yhteysmerkkijonon.

- Hybrid Yhteyksienhallinnan on käytettävä kelvollinen paikallisen yhteysmerkkijonon luvan. Azure verkkosovelluksissa tai Mobile-sovellusten on käytettävä hakemuksen yhteysmerkkijonon luvan.
- Hybrid yhteydet skaalata asentamalla toiseen käynnissä Hybrid Yhteyksienhallinnan toisessa palvelimessa. Määrittää paikallisen kuuntelua käytettävä sama osoite ensimmäisen paikallisen kuuntelua. Tässä tilanteessa liikenne aktiivinen paikallisen kuuntelijoita välillä on satunnaisesti hajautettu (PYÖRISTÄ-funktiota Mikko). 


## <a name="ManageHybridConnection"></a>Hybrid yhteyksien hallinta
Voit hallita Hybrid yhteyksiä seuraavasti:

- Azure-portaalin käyttäminen ja siirry BizTalk-palvelussa. 
- Käytä [REST API](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopioi tai luo Hybrid yhteyden merkkijonot

1. Kirjautuminen [Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Valitse vasemmassa siirtymisruudussa **BizTalk** palvelut, ja valitse sitten BizTalk-palvelussa. 
3. Valitse **Hybrid yhteydet** -välilehdessä:  
![Hybrid yhteydet-välilehti][HybridConnectionTab]
4. Valitse Hybrid-yhteys. Valitse **Hallitse yhteyden**tehtäväpalkissa:  
![Asetusten hallinta][HCManageConnection]

    **Hallitse yhteyden** luettelo sovelluksen ja paikallisen yhteyden merkkijonoja. Voit kopioida yhteyden merkkijonoja tai käyttää yhteysmerkkijonon pikanäppäin uudelleen. 

    **Jos valitset Luo**, Jaetut pikanäppäin, käyttää yhteysmerkkijonon muutetaan. Toimi seuraavasti:
    - Valitse **Synkronoi näppäimet** Azure perinteinen portal Azure-sovelluksessa.
    - Suorita **Paikallisen asennusohjelma**uudelleen. Kun suoritat paikallisen-asennus uudelleen, paikallisen resurssin automaattisesti määritetty käyttämään päivitetyt ensisijainen yhteysmerkkijonon.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Ryhmäkäytännön avulla voit hallita Hybrid yhteyden käyttämät paikallisen resurssit

1. Lataa [Hybrid Yhteyksienhallinnan järjestelmänvalvojan malleja](http://www.microsoft.com/download/details.aspx?id=42963).
2. Pura tiedostot.
3. Käyttöön tietokoneessa, jossa Muokkaa Ryhmäkäytäntö toimi seuraavasti:  

    - Kopioi. ADMX tiedostot *%WINROOT%\PolicyDefinitions* -kansioon.
    - Kopioi. Kuolleisuusraja tiedostot *%WINROOT%\PolicyDefinitions\en-us* -kansioon.

Kopioimisen, voit muuttaa käytännön ryhmäkäytännön editori.




## <a name="next"></a>Seuraava

[Azure verkkosovelluksissa yhdistäminen paikallisen resurssi](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Yhteyden muodostaminen paikalliseen SQL Server Azure Web Apps-sovelluksista](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Hybrid Tietoyhteyksien yleiskatsaus](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Katso myös

[Microsoft Azure BizTalk palveluiden REST-Ohjelmointirajapinnalla](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: Versiot kaavio](biztalk-editions-feature-chart.md)  
[Luo BizTalk palvelu Azure perinteinen-portaalissa](biztalk-provision-services.md)  
[BizTalk Services: Raporttinäkymät-ikkunan, näyttö ja skaalaus-välilehdet](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
