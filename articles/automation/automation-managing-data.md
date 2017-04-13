<properties 
   pageTitle="Azure automaatio tietojen hallinta | Microsoft Azure"
   description="Tässä artikkelissa on useita aiheita hallintaan Azure automaatio-ympäristössä.  Sisältää tällä hetkellä tietojen säilytys ja Azure automaatio-palauttaminen Azure automaatio varmuuskopioiminen."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Azure automaatio tietojen hallinta

Tässä artikkelissa on useita aiheita hallintaan Azure automaatio-ympäristössä.

## <a name="data-retention"></a>Tietojen säilytys

Kun poistat Azure automaatio resurssin, se säilyttää valvonta tarkoituksiin 90 päivää ennen poistetaan pysyvästi.  Ei voi tarkastella tai käyttää resurssin tänä aikana.  Käytäntö koskee myös resurssit, jotka kuuluvat automaatio-tiliä, jolla on poistettu.

Azure automaatio poistaa automaattisesti ja poistaa pysyvästi 90 päivää vanhemmat työt.

Seuraavassa taulukossa on yhteenveto eri resursseja säilytyskäytäntö.

|Tietoja|Käytäntö|
|:---|:---|
|Tilit|Kun tili on poistettu käyttäjän 90 päivää poistetaan pysyvästi.|
|Resurssit|Poistetaan pysyvästi 90 päivää kohteen käyttäjän poistamisen jälkeen, tai tiliä, joka sisältää kohteen poistetaan käyttäjän 90 päivän.|
|Moduulit|Poistetaan pysyvästi 90 päivää moduulin käyttäjän poistamisen jälkeen, tai tiliä, joka sisältää moduulin poistetaan käyttäjän 90 päivän.|
|Runbooks|Poistetaan pysyvästi 90 päivää, kun resurssi poistetaan käyttäjän tai 90 päivää, joka sisältää resurssi poistetaan käyttäjän tilin jälkeen.|
|Projektit|Poistetun ja pysyvästi poistetut 90 päivän jälkeen viimeksi muokataan. Tämä voi johtua, kun työ on valmis, pysäytetään tai keskeytetään.|
|Solmun käyttömahdollisuudet/MOF-tiedostot| Vanha solmu määritykset poistetaan pysyvästi 90 päivän jälkeen luodaan uusi solmu-määritys.|
|DSC solmujen| 90 päivän kuluessa solmu ei ole rekisteröity automaatio-tililtä käyttämällä Windows PowerShellin Azure portal tai [Poista AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) cmdlet-komento poistetaan pysyvästi. Solmujen poistetaan myös pysyvästi 90 päivää, joka sisältää solmu poistetaan käyttäjän tilin jälkeen. |
|Solmu-raportit| Poistetaan pysyvästi 90 päivän kuluessa solmun luodaan uusi raportti|

Säilytyskäytännön koskee kaikkia käyttäjiä ja tällä hetkellä ei voi mukauttaa.

## <a name="backing-up-azure-automation"></a>Azure automaatio varmuuskopioiminen

Kun poistat Microsoft Azure-automaatio-tiliä, kaikki objektit tilin poistetaan mukaan lukien runbooks, moduulit, määritykset, asetukset, työt ja resurssit. Objekteja ei voi palauttaa, kun tili on poistettu.  Voit varmuuskopioida ennen poistamista automaatio-tilisi sisällön seuraavat tiedot. 

### <a name="runbooks"></a>Runbooks

Voit viedä oman runbooks komentosarja-tiedostojen käyttäminen Windows PowerShellin Azure hallinta-portaalin tai [Hae AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) cmdlet-komento.  Nämä komentosarjatiedostot voi tuoda toinen automaatio-tili kuten edellä [luominen](https://msdn.microsoft.com/library/dn643637.aspx)tai tuominen Runbookin.


### <a name="integration-modules"></a>Integrointi moduulit

Integrointimoduuleja ei voi viedä Azure automaatio.  Varmista, että ne ovat käytettävissä ulkopuolella automaatio-tili.

### <a name="assets"></a>Resurssit

Ei voi viedä Azure automaatio [resurssit](https://msdn.microsoft.com/library/dn939988.aspx) .  Azure hallinta-portaalin on Huomautus muuttujat, tunnistetiedot, varmenteet, yhteydet ja aikatauluja tietoja.  Valitse manuaalisesti luotava kalusteet, joiden runbooks tuoda sen toiseen automaatio avulla.

Voit noutaa tietoja salaamaton varat ja joko tallentaa myöhempää käyttöä varten tai vastaa resurssien luominen toisen automaatio-tilin [Azure cmdlet-komennot](https://msdn.microsoft.com/library/dn690262.aspx) .

Ei voi hakea salattuja muuttujat tai cmdlets-komennoista tunnistetiedot salasana-kenttään arvo.  Jos et tiedä nämä arvot, valitse voit noutaa ne runbookin, [Hae AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) ja [Hae AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) tehtävien avulla.

Ei voi viedä Azure automaatio varmenteet.  Varmista, että kaikki varmenteet ovat käytettävissä Azure ulkopuolella.

### <a name="dsc-configurations"></a>DSC käyttömahdollisuudet

Voit viedä komentosarja-tiedostojen käyttäminen Windows PowerShellin Azure hallinta-portaalin tai [Vie AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) cmdlet-komennon avulla perusasetukset. Määritysten voidaan tuoda ja käyttää toista automaatio-tiliä.


##<a name="geo-replication-in-azure-automation"></a>Azure automaatio GEO-replikointi

GEO-replikoinnin-standardia Azure automaatio-tilejä, varmuuskopioi tiedot eri maantieteellisen redundanssin. Voit valita ensisijainen alue, kun määrittänyt tilin ja sitten toissijainen alue on määritetty automaattisesti. Toissijainen ensisijainen alueen kopioituja tietoja päivitetään jatkuvasti Jos tietojen menettämisen.  

Seuraavassa taulukossa on käytettävissä ensisijainen ja toissijainen alueen parit.

|Ensisijainen            |Toissijainen
| ---------------   |----------------
|Etelä keskitetyn USA   |Pohjois-keskitetyn USA
|Yhdysvaltojen Itä 2          |Keskitetyn USA
|Länsi Europe        |Pohjois-Eurooppa
|Etelä Itä-Aasian    |Itä-Aasian
|Japani Itä         |Japani Länsi

Ensisijainen aluetiedot menetetään epätodennäköistä tapauksessa Microsoft yrittää palauttaa sen. Jos ensisijainen tietoja ei voi palauttaa, geo automaattisesti suorittaa ja tarvitseville asiakkaille ilmoitetaan tästä tilauksen kautta.

