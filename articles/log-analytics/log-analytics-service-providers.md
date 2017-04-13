<properties
    pageTitle="Kirjaudu Analytics ominaisuuksia for palveluntarjoajia | Microsoft Azure"
    description="Lokitiedoston Analytics avulla hallitaan palveluntarjoajat (MSPs) suurille yrityksille riippumaton ohjelmakoodi toimittajat ja palvelun palveluntarjoajien hallinta ja valvonta palvelinten asiakkaan paikallisen tai pilvi-infrastruktuuria."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="log-analytics-features-for-service-providers"></a>Kirjaudu for palveluntarjoajia Analytics-ominaisuudet

Lokitiedoston Analytics auttaa hallitun palveluntarjoajat (MSPs), suurille yrityksille tai (vertaiskäyttäjien) isännöintipalvelu palveluntarjoajien hallinta ja valvonta palvelinten asiakkaan paikallisen tai pilvi-infrastruktuuria. 

Suurissa organisaatioissa jakaminen paljon yhtäläisyyksiä palveluntarjoajien, erityisesti silloin, kun keskitetty IT-ryhmä, joka vastaa hallinnasta on useita eri liiketoimintayksiköitä IT. Yksinkertaisuuden tämä asiakirja käyttää termin- *palveluntarjoajan* mutta samat toiminnot on myös yritysten ja muiden asiakkaiden käytettävissä.

## <a name="cloud-solution-provider"></a>Cloud ratkaisun toimittajaan

Kumppanit ja palveluntarjoajat [Cloud ratkaisu Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) -ohjelman käyttävien Log Analytics on yksi Azure palvelujen CSP-tilauksessa. 

Log analysoinnissa seuraavia ominaisuuksia ovat käytettävissä *Cloud ratkaisun toimittajaan* tilaukset.

*Cloud ratkaisun toimittajaan* kuin voit tehdä seuraavaa:

+ Luo lokitiedoston Analytics-työtilat palvelutili (asiakkaan) tilaukseen.
+ Access-työtilat alihallinnat luoma. 
+ Lisätä ja poistaa käyttöoikeuden työtilaan käyttämällä Azure käyttäjien hallinta. Kun vuokraajan työtilassa OMS portaalissa käyttäjien hallinta sivun kohdassa asetuksia ei ole käytettävissä
  - Lokitiedoston Analytics ei tue Roolipohjainen käytön vielä - Anna käyttäjän `reader` Azure-portaalissa käyttöoikeudet mahdollistaa tekemällä määritysmuutoksia OMS-portaalissa

Kirjautua sisään vuokraajan tilauksen sinun on määritettävä vuokraajan tunnus. Vuokraajan tunniste on usein sähköpostiosoite, jolla kirjaudutaan sisään viimeinen osa.

+ Lisää OMS-portaalin `?tenant=contoso.com` -portaalin URL-osoite. Esimerkiksi`mms.microsoft.com/?tenant=contoso.com`
+ PowerShellin avulla `-Tenant contoso.com` parametrin käytettäessä `Add-AzureRmAccount` cmdlet-komento
+ Vuokraajan tunnus lisätään automaattisesti, kun käytät `OMS portal` linkki Azure-portaalista voit avata ja kirjaudu sisään valitun työtilan OMS-portaaliin

*Asiakkaan* Cloud ratkaisu-palvelun avulla voit

+ Lokitiedoston analytics-työtilan luominen CSP tilauksen
+ Access-työtilat CSP luoma
  -  Käytä `OMS portal` linkki Azure-portaalista voit avata ja kirjaudu sisään valitun työtilan OMS-portaaliin
+ Tarkastella ja käyttää käyttäjän hallintasivun asetukset-kohdassa OMS-portaalissa

>[AZURE.NOTE] Varmuuskopiointi ja palauttaminen ratkaisuja lokin analysoinnissa eivät voi muodostaa palautus palvelut-säilö ja ei voi määrittää CSP tilauksen.

## <a name="managing-multiple-customers-using-log-analytics"></a>Useiden asiakkaiden, jotka käyttävät Log Analytics hallinta 

On suositeltavaa Log Analytics-työtilan luominen kunkin asiakkaan hallitset. Lokitiedoston Analytics-työtilassa on:

+ Tiedot tallennetaan maantieteellisen sijainnin. 
+ Lisätietoja laskutuksesta rakeisuuden 
+ Tietoja eristystaso 
+ Yksilöivä määritys

Luomalla asiakasta kohti työtilan olet erottaa kunkin asiakkaan tietoja ja seurata myös kunkin asiakkaan käyttö.

Milloin ja miksi useita työtiloja kannattaa luoda? Lisätietoja on kuvattu [hallita niiden käyttöä Kirjaudu analytics] (log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Luominen ja määrittäminen asiakkaan työtilat voidaan automatisoida käyttämällä [PowerShell](log-analytics-powershell-workspace-configuration.md)- [Resurssienhallinta mallit](log-analytics-template-workspace-configuration.md)- tai käyttämällä [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

Resurssienhallinta mallien käyttäminen työtilan kokoonpano voit on perusmuodon määrityksistä, jotka voidaan luoda ja määrittää työtilat. Voit luottaa, että työtilat luodaan asiakkaille, ne on automaattisesti määritetty tarpeen. Kun päivität tarpeen, malli on päivitetty ja sitten uudelleen käyttöön aiemmin työtilat. Tämä prosessi varmistaa, että myös aiemmin työtilat täyttävät uudet standardit.    

Kun hallinta useita Log Analytics-työtilat, on suositeltavaa kunkin työtilan integraation aiemmin lippujärjestelmän / toiminnot-konsolin [ilmoitukset](log-analytics-alerts.md) -toiminnon avulla. Integroimalla aiemmin järjestelmien kanssa tukihenkilökunta jatkaa seuraa niiden tuttuja prosessit. Lokitiedoston Analytics tarkistaa jokaisen työtilan ilmoitusten ehdot vastaan säännöllisesti ja luo ilmoituksen, kun toiminto tarvita.

Johdon tason raportteja, jotka yhteenvetoja työtilat kautta voit käyttää lokin Analytics ja [PowerBI](log-analytics-powerbi.md)välinen integrointi. Jos haluat integroida toiseen raportoinnin järjestelmään, voit Etsi Ohjelmointirajapinnan (joko PowerShell tai [REST](log-analytics-log-search-api.md)) voit suorittaa kyselyjä ja viedä hakutulokset.

## <a name="next-steps"></a>Seuraavat vaiheet

+ Automatisoida luominen ja määrittäminen työtilat [Resurssienhallinta mallien](log-analytics-template-workspace-configuration.md) käyttäminen
+ [PowerShellin](log-analytics-powershell-workspace-configuration.md) työtilojen luonnin automatisoida 
+ Integroi aiemmin, jossa [ilmoitusten](log-analytics-alerts.md) avulla
+ Yhteenveto raportteja käyttämällä [PowerBI](log-analytics-powerbi.md)
