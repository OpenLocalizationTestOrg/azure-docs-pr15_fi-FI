<properties 
    pageTitle="Azure palvelun Web sovellukset Java sovelluksen lisääminen" 
    description="Tässä opetusohjelmassa näytetään, miten voit lisätä sivulle tai sovelluksen Azure palvelun Web sovellukset, jotka on jo määritetty käyttämään Java-esiintymään." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Azure palvelun Web sovellukset Java sovelluksen lisääminen

[Azure-sovelluksen palvelun][] Java web app on alustaa suorittimessa, [Luo Java verkkosovellukseen Azure-sovelluksen käytössä](web-sites-java-get-started.md), kun voit ladata sovelluksen sijoittamalla oman SODAN **webapps** -kansiossa.

Siirtyminen **webapps** -kansion polku vaihtelee Web Apps-esiintymän määrittämisestä.

- Jos määrität web Appin avulla Azure Marketplace- **webapps** -kansion polku on lomakkeen **d:\home\site\wwwroot\bin\application\_server\webapps**, jossa **sovelluksen\_palvelimen** on sovelluspalvelin voimassa for Web Apps-esiintymän nimi. 
- Jos määrität koodiin Azure määrityksen Käyttöliittymän avulla, **webapps** -kansion polku on lomakkeen **d:\home\site\wwwroot\webapps**. 

Huomaa, että voit käyttää tietolähteen ohjausobjektin ladata sovelluksen tai web-sivuille, mukaan lukien [Jatkuva integrointi skenaarioita](app-service-continuous-deployment.md). FTP on myös ladataan sovelluksen tai web-sivujen haluamasi vaihtoehto.

Huomautus Tomcat web Apps-sovellukset: kun lisättyjen SODAN tiedoston **webapps** -kansioon, Tomcat sovelluspalvelin havaitsee, että olet lisännyt ja automaattisesti ladata sen. Huomaa, että jos kopioit pääkansion tiedostot (muut kuin SOTA-tiedostoja), sovelluspalvelin on käynnistettävä uudelleen, ennen kuin kyseiset tiedostot käytetään. Autoload-toiminto käytössä Azure Tomcat Java web Apps-sovellukset perustuu uusi SODAN tiedosto lisätään, tai uusia tiedostoja tai kansioita, jotka on lisätty **webapps** -kansioon. 

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [Java Developer Center](/develop/java/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure sovelluksen-palvelu]: http://go.microsoft.com/fwlink/?LinkId=529714
