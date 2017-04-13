<properties 
   pageTitle=".NET 2.9 Azure SDK julkaisutiedot" 
   description=".NET 2.9 Azure SDK julkaisutiedot" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-29-release-notes"></a>.NET 2.9 Azure SDK julkaisutiedot

##<a name="overview"></a>Yleiskatsaus

Tämä asiakirja sisältää julkaisutiedot Azure SDK .NET 2.9 versioon. 

Lisätietoja päivityksistä tässä versiossa on artikkelissa [Azure SDK 2.9 ilmoitus viestin](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Azure SDK 2.9 Visual Studio 2015 päivityksen 2 ja Visual Studio "15" esikatselu
 
Päivitys sisältää seuraavat virheenkorjauksia:

- Jossa merkkijonon "Tuntematon tyyppi" näkyy muodossa koodin gen-kansion nimi ja/tai nimitilan nimi liittyvät VIE API asiakkaan luonti-ongelma kohteet poistetaan luotu koodiksi.
- Ongelman liittyvät ajoitettu WebJobs, jossa todennustiedot on viallinen välittämisen valmistelu ajoitus.

Päivitys sisältää seuraavat uusi ominaisuus:

- Tuki toissijainen App Services-palvelujen valmistelu App palvelu-valintaikkunan "Palvelut"-välilehti. 

##<a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure Data järvi Tools for Visual Studio 2015 päivitys 2
 
Tämä päivityksiä ovat esimerkiksi seuraavat:

- Visual Studio **Azure järvi Datatyökalut** yhdistetään nyt Azure SDK .NET-versiossa. Työkalu asennetaan automaattisesti, kun asennat Azure SDK-paketissa. 

    Työkalu päivitetään usein, valitse [tähän](http://aka.ms/datalaketool) päivitykset.

- **Palvelimen Explorerilla** voi nyt voit tarkastella kaikkia ja luoda U-SQL-metatiedot jotkin kohteet. Lisätietoja on artikkelissa [Tämä](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blogiin.


##<a name="hdinsight-tools"></a>HDInsight-Työkalut 

Visual Studio **HDInsight Työkalut** tukee nyt HDInsight versio 3.3, mukaan lukien Tez kaavioiden näyttäminen ja korjaa kieltä.


##<a name="azure-resource-manager"></a>Azure Resurssienhallinta 

Tässä versiossa Lisää [KeyVault](../resource-manager-keyvault-parameter.md) tuen ARM malleille.

##<a name="see-also"></a>Katso myös

[Azure SDK 2.9 ilmoitus viestiin](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)
