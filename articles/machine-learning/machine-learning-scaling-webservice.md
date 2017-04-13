<properties
   pageTitle="WWW-palvelun skaalaus | Microsoft Azure"
   description="Opettele skaalata verkkopalvelun lisääntyvien samanaikainen ja lisätä uuden päätepisteet."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure konepohjaisten oppimistekniikoiden verkkopalvelut, operationalization, skaalaus-päätepisteen samanaikainen"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Web-palvelu skaalaus

>[AZURE.NOTE] Tässä ohjeaiheessa kerrotaan koskevat perinteinen koneen Learning-WWW-palvelun avulla. 

Oletusarvon mukaan kunkin julkaistun Web-palvelu on määritetty tukemaan 20 samanaikaiset pyynnöt ja voi olla yhtä suuret kuin 200 samanaikaiset pyynnöt. Kun Azure perinteinen-portaalissa on vielä Aseta arvoksi, Azure koneen Learning optimoi automaattisesti asetus parhaan suorituskyvyn WWW-palvelun tarjoamista varten ja portaalin arvo ohitetaan. 

Jos aiot Soita Ohjelmointirajapinnan kanssa kuin 200 Maks samanaikainen puhelut arvo suurempi kuormituksen tukee, voit luoda useita päätepisteet saman Web-palvelusta. Voit sitten satunnaisesti jakaa että latautuvat yli ne kaikki.

## <a name="add-new-endpoints-for-same-web-service"></a>Lisää uusi päätepisteet saman web-palvelu

Web-palvelu skaalaus on yleisiä tehtävä. Syitä, jos haluat skaalata ovat tukevat yli 200 samanaikaiset pyynnöt, Suurenna useita päätepisteet käytettävyydessä tai antaa eri päätepisteet WWW-palvelun. Voit suurentaa asteikon lisäämällä muita päätepisteet saman WWW-palvelun kautta [Azure perinteinen portal](https://manage.windowsazure.com/) tai [Azure koneen Learning verkkopalvelu](https://services.azureml.net/) -portaalin.

Lisätietoja uuden päätepisteet lisäämisestä on artikkelissa [Luominen päätepisteet](machine-learning-create-endpoint.md).

Ota huomioon, suuri samanaikainen laskeminen käyttämällä voi olla haittaa, jos olet kutsu Ohjelmointirajapinnan vastaavasti suuria arvolla. Saatat nähdä viive arviointi aikakatkaisu ja/tai piikkarit Jos asetat suhteellisen vähän kuormituksen Ohjelmointirajapinnan määritetty suuren kuormituksen aikana.

Synkronoitu ohjelmointirajapinnan käytetään yleensä tilanteissa, jossa pieni viive toivottuja. Viive tähän merkitsee sen kestää API yhden pyynnön suorittamiseen ja ei ota huomioon verkon minkä tahansa viiveitä. Oletetaan, että sinulla Ohjelmointirajapinnan kanssa 50 ms viive. Tarjoaman täysin käytettävissä olevan kapasiteetin rajoituksen viereiset suuri ja Maks samanaikainen puhelut = 20, sinun on soitettava Tämä API 20 * 1000 / 50 = 400 kertaa sekunnissa. Laajentaminen tämä edelleen Maks samanaikainen puhelut 200 avulla voit kutsua API 4000 ajat sekunnissa olettaen 50 ms viive.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
