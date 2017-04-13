<properties
   pageTitle="Projektien päivittämisestä uusimpaan versioon Azure-Työkalut | Microsoft Azure"
   description="Opi päivittämään Azure Visual Studio-projektiksi Azure Työkalut nykyinen versio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Projektien päivittämisestä uusimpaan versioon Azure-Työkalut Visual Studio

## <a name="overview"></a>Yleiskatsaus

Kun olet asentanut Azure-työkalut (tai aiemman version, joka on uudempi kuin 1.6) nykyisessä versiossa, projekteissa, jotka on luotu käyttämällä Azure-työkaluja Vapauta ennen 1.6 (marraskuu 2011) päivitetään automaattisesti heti, kun tiedostoa avattaessa. Jos olet luonut projektien käyttämällä Työkalut 1,6 (marraskuu 2011)-versiossa ja on edelleen kyseistä versio asennettuna, voit avata projekteihin vanhempi versio ja päätät myöhemmin, haluatko päivittää ne.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Miten projektin muuttuu, kun päivität sen

Jos projektin päivitetään automaattisesti tai voit määrittää, että haluat päivittää sen, projektin on muokattu nykyisen versioita tiettyjä kokoonpanojen ja joitakin ominaisuuksia muutetaan myös, kuten tässä osassa kuvataan. Jos projektin edellyttää muita muutoksia työkaluja uudemman version kanssa, sinun on tehtävä muutokset manuaalisesti.

- Seuraavan koodin korostetut web-roolien ja työntekijä roolien app.config-tiedostoa päivitetään viittaamaan Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll uudempi versio.

- Microsoft.WindowsAzure.StorageClient.dll, Microsoft.WindowsAzure.Diagnostics.dll ja Microsoft.WindowsAzure.ServiceRuntime.dll kokoonpanon päivitetään uusia versioita.

- Julkaise-profiileista, jotka on tallennettu Azure projektitiedoston (.ccproj) siirtyvät erilliseen tiedostoon, jonka tunniste, .azurePubXml **Julkaise** alikansioon.

- Julkaise-profiilin joitakin ominaisuuksia päivitetään tukemaan uusista ja muuttuneista ominaisuuksista. **AllowUpgrade** korvataan **DeploymentReplacementMethod** , koska voit päivittää käyttöön pilvipalvelussa samanaikaisesti tai soluissa.

- Ominaisuuden **UseIISExpressByDefault** lisätään ja arvoksi false, niin, että verkkopalvelimeen, jota käytetään virheenkorjaus automaattisesti ei muuta Internet Information Services (IIS)-IIS Expressiin. IIS Express on oletusarvoinen verkkopalvelin projekteille, jotka on luotu työkaluja uudempia versioita.

- Jos Azure välimuistiin nykyisessä yhdessä tai useammassa projektin roolit, jotkin ominaisuudet palvelun määritys (.cscfg-tiedosto) ja palvelun määritys (.csdef-tiedosto) muuttuvat projektin päivityksen. Jos projektin käyttää Azure välimuistiin NuGet-paketti, projekti on päivitetty paketin uusin versio. Avataan seuraavan koodin korostetut ja varmista, että asiakkaan määrityksissä on ylläpitää oikein päivityksen aikana. Jos olet lisännyt Azure välimuistiin asiakkaan kokoonpanot viittaukset käyttämättä NuGet-paketti, ei päivitetä näistä kokoonpanoista; viittaukset uusia versioita on päivitettävä manuaalisesti.

>[AZURE.IMPORTANT] Viittaukset Azure laitekokonaisuuksiin F # projekteissa, on päivitettävä manuaalisesti niin, että ne viittaavat näiden kokoonpanojen uudempia versioita.

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Azure projektin päivittämisestä uusimpaan

1. Asenna Visual Studiossa, jota haluat käyttää päivitetyn projektin asennettuun nykyisen version Azure-työkalut ja avaa sitten projekti, jonka haluat päivittää. Jos projektissa on luotu Azure-Työkalut Vapauta ennen 1.6 (marraskuu 2011), projektin automaattisesti päivitetty uusimpaan versioon. Jos projektissa on luotu marraskuu 2011 ja vapauta ja kyseisen release on edelleen asennettuna, project avaa kyseisen versiossa.

1. Napsauta ratkaisunhallinnassa Avaa project-solmu pikavalikon, valitse **Ominaisuudet**ja valitse sitten tulevasta valintaikkunasta **sovellus** -välilehti.

    **Sovelluksen** -välilehdessä näkyy Työkalut-versio, joka liittyy projektin. Jos nykyinen versio Azure-Työkalut tulee näkyviin, projekti on jo päivitetty. Jos olet asentanut uudempaan versioon Työkalut välilehdessä näkyy kuin, **Päivitä** -painike tulee näkyviin.

1. Valitse projektin päivittämiseksi työkaluja nykyisen version **Päivitä** -painike.

1. Projektin luominen ja osoite, jotka aiheutuvat Ohjelmointirajapinnan muutosten virheitä. Lisätietoja uuden version koodin muokkaamisesta on tietyn Ohjelmointirajapinnan ohjeissa.
