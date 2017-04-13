<properties
    pageTitle="Web-sovelluksen Kloonaamalla Azure-portaalissa"
    description="Opettele Kloonaa Web Apps-sovellusten Azure-portaalissa uusi Web-sovelluksiin."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Azure App palvelun sovelluksen Kloonaamalla käyttämällä Azure Portal#

[Azure App palvelun](http://go.microsoft.com/fwlink/?LinkId=529714) verkkosovelluksissa kloonausohjelmistoja ominaisuuden avulla voit helposti Kloonaa aiemmin verkkosovelluksissa juuri luomasi sovellukseen toisella alueella tai samalla alueella. Tämä ottaa käyttöön asiakkaat voivat ottaa käyttöön useita sovelluksia eri alueiden nopeasti ja helposti.

Sovelluksen kloonaamalla ei tällä hetkellä tueta vain premium taso app palvelusopimusten vaihtoehdot. Uusi ominaisuus käyttää samat rajoitukset Web Apps varmuuskopiointi-ominaisuudeksi on artikkelissa [Azure-sovelluksen palvelun verkkosovellukseen varmuuskopioida](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>Olemassa olevan sovelluksen kloonaamalla ##

Web-sovelluksen on oltava käynnissä, jotta voit luoda Kloonaa web-sovelluksen **Premium** -tilassa.

1. [Azure-portaalissa](https://portal.azure.com/)Avaa web-sovelluksen sivu.
2. Valitse **Työkalut**. Valitse **Työkalut** -sivu **Kloonaa sovelluksen**.

    ![][1]

    > [AZURE.NOTE]
    > Jos web app ei ole **Premium** -tilassa, näyttöön tulee sanoma, sovelluksen kloonaamalla tuettujen tilojen. Tässä vaiheessa sinun on valita **Päivitä**.
    
3. Ole nimi uusi web app, resurssiryhmä ja sovelluksen palvelun suunnitteleminen **Kloonaa sovelluksen** sivu. Myös käyttäjä saa oikeuden Valitse, haluatko Kloonaa lähteen web app-asetukset useita vai ei.

    ![][2]

4. Valittuasi **Luo** platform alkaa toimimasta Kloonaa lähde-web-sovelluksen luominen.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Kloonaamalla olemassa olevan sovelluksen App Service-ympäristössä##

Asiakas on **Kloonaa sovelluksen** sivu voi valita sovelluksen sovellussarjan aiemmin App Service-ympäristössä.

## <a name="current-restrictions"></a>Nykyiset rajoitukset ##

Tämä toiminto ei esikatselussa, voit lisätä uusia ominaisuuksia ajan kuluessa Yritämme, seuraavassa luettelossa on nykyisen tuki tunnetut rajoituksista sovelluksen kloonaamalla Azure-portaalissa:

- Azure liikenteen hallinta-asetuksia ei kopioitu
- Automaattinen mittakaava-asetuksia ei kopioitu
- Aikatauluasetuksia ei kopioitu
- VNET asetuksia ei kopioitu
- Sovelluksen tietoja ei ole automaattisesti määritetty kohde web Appissa
- Helppo Auth asetuksia ei kopioitu
- Kudu tunniste ei kopioitu
- Vihje sääntöjä ei kopioitu
- Tietokannan sisältö ei kopioitu


### <a name="references"></a>Viittaukset ###
- [Web-sovelluksen Kloonaamalla PowerShellin avulla](app-service-web-app-cloning.md)
- [Azure-sovelluksen palvelun verkkosovellukseen varmuuskopiointi](web-sites-backup.md)
- [Sovelluksen Service-ympäristön luomiseen](app-service-web-how-to-create-an-app-service-environment.md)
- [Web-sovelluksen luominen sovelluksen Service-ympäristössä](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [Johdanto App Service-ympäristöön](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png