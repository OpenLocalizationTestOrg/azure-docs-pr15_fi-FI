<properties
   pageTitle="Hallitse sovellustesi Visual Studiossa | Microsoft Azure"
   description="Visual Studio avulla voit luoda, kehittää, pakata, ottaminen käyttöön ja virheenkorjaus palvelun kangasta sovelluksia ja palveluja."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="seanmck;mikhegn"/>

# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Visual Studiolla voit yksinkertaistaa kirjoittaminen ja palvelun kangasta sovellusten hallinta

Voit hallita Azure palvelun kangasta sovelluksia ja palveluja Visual Studio. Kun olet [kehittäminen ympäristön määritys](service-fabric-get-started.md), voit käyttää Visual Studio palvelun kangasta sovellusten luominen, Lisää palveluita tai paketin, Rekisteröi ja ottaa sovellusten paikallista kehittämistä-klusterin.

## <a name="deploy-your-service-fabric-application"></a>Palvelun kangasta sovelluksen käyttöönotto

Oletusarvon mukaan sovelluksen käyttöönotto yhdistetään yhdeksi yhden helppokäyttöisyys seuraavasti:

1. Sovelluspaketin luominen
2. Kuva-kaupan sovelluspaketin lataaminen
3. Sovelluksen tyyppi rekisteröiminen
4. Poistamista Palvelusovellusten esiintymät käynnissä
5. Uuden sovelluksen esiintymän luominen

Visual Studiossa painamalla **F5-näppäintä** myös sovelluksen käyttöönotto ja liittää virheenkorjaus kaikki sovelluksen esiintymät. Voit käyttää **Ctrl + F5** -sovelluksen ottaminen käyttöön ilman virheenkorjaus tai voit julkaista paikallisessa tai etätietokoneessa klusterin käyttämällä Julkaise profiilia. Lisätietoja on kohdassa [Julkaise sovelluksen remote klusterin Visual Studion avulla](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Sovelluksen virheenkorjaus-tilassa

Oletusarvon mukaan Visual Studio poistaa sovelluksen tyyppi esiintymät, kun lopetat virheenkorjaus tai (Jos olet asentanut sovelluksen liittämättä virheenkorjaus), kun sovellus käyttöön. Siinä tapauksessa kaikki sovelluksen tiedot poistetaan. Korjattaessa paikallisesti haluat säilyttää tiedot, jotka olet jo luonut Kun testataan sovelluksen uuden version haluat säilyttää sovelluksen käytön tai haluat myöhemmin virheenkorjaus istuntoon ja päivitä sovellus. Visual Studio palvelun kangasta työkalut tarjoavat ominaisuuden **Sovelluksen virheenkorjaus-tilassa**, mitkä ohjausobjekteista onko **F5** asennuksen poistaminen sovelluksen säilyttää käynnissä, kun virheenkorjausistunnon päättyy tai ottaa käyttöön sovelluksen tulevissa muistin istunnoissa, valitse päivitettävä sijaan poistetaan ja myymälät sovelluksen.

#### <a name="to-set-the-application-debug-mode-property"></a>Sovelluksen korjaaminen tila-ominaisuuden määrittäminen

1. Sovelluksen projektin hiiren kakkospainikkeella ja valitse **Ominaisuudet** (tai painamalla **F4** -näppäintä).
2. **Ominaisuudet** -ikkunassa ominaisuuden **Sovelluksen virheenkorjaus-tilassa** .

    ![Määritä sovelluksen virheenkorjaus tila-ominaisuus][debugmodeproperty]

Nämä ovat käytettävissä **Sovelluksen virheenkorjaus tila** -asetukset.

1. **Automaattisen päivityksen**: sovelluksen säilyy suoritetaan, kun virheenkorjaus-istunto päättyy. Seuraavan **F5** käsitellään käyttöönoton päivityksen käyttämällä lähetetä automaattinen tila päivittämään uudempaan sovellus nopeasti päivämäärä-merkkijonolla, joka on lisätty. Päivitysprosessin säilyttää edellisen virheenkorjaus-istunnossa kirjoittamasi tiedot.

2. **Säilytä sovelluksen**: pitää sovellusta klusterin virheenkorjausistunnon päättyessä. Seuraava **F5** -sovellus poistetaan ja äskettäin luodun sovellus otetaan käyttöön klusterin.

3. **Poista sovellus** aiheuttaa sovellus poistetaan, kun virheenkorjaus istunto päättyy.

Saat **Automaattisen päivityksen** tiedot säilytetään lisäämällä palvelun kangasta sovelluksen päivityksen ominaisuuksia, mutta se on virittää turvallisuutta sijaan suorituskyvyn parantamiseksi. Lisätietoja sovellusten ja miten voi suorittaa päivityksen reaali-ympäristössä Katso [palvelun kangasta sovelluksen päivittäminen](service-fabric-application-upgrade.md).

![Esimerkki päivämäärän, joka on lisätty uusi sovellusversio][preservedata]

>[AZURE.NOTE] Tämä ominaisuus ei ole versiota 1.1 palvelun kangasta Työkalut Visual Studio. Ennen 1.1 Käytä saavuttamiseksi sama ongelma **Säilyttää tiedot käynnistettäessä** -ominaisuus. "Säilytä sovelluksen"-vaihtoehto otettiin käyttöön palvelun kangasta Työkalut versio 1.2 for Visual Studio.

## <a name="add-a-service-to-your-service-fabric-application"></a>Lisää palvelu palvelun kangasta-sovellukseen

Voit lisätä uusia palveluita sovelluksen sen toimintojen laajentamiseksi.  Voit varmistaa, että palvelu sisältyy sovelluksen-paketti, Lisää palvelun kautta **Uuden... palvelun kangasta** -valikkovaihtoehto.

![Lisää uusi kangasta palvelu-sovellukseen][newservice]

Valitse palvelun kangasta projektityypin sovelluksen lisääminen ja palvelun nimi.  Katso avulla voit määrittää, mitkä palvelutyyppi käyttämään [framework palvelun valitseminen](service-fabric-choose-framework.md) .

![Valitse kangasta palvelun projektityypin sovelluksen lisääminen][addserviceproject]

Uusi palvelu lisätään ratkaisu ja aiemmin sovelluspaketin. Palvelun viittaukset ja palvelun oletusesiintymää lisätään sovelluksen luettelo. Palvelun luodaan ja seuraavan kerran, voit ottaa käyttöön sovelluksen käytön aloittaminen.

![Uusi palvelu lisätään sovelluksen-luettelo][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Palvelun kangasta sovelluksen pakkaaminen

Ottaa käyttöön sovelluksen ja palveluja klusteriin, tarvitset sovelluspaketin luominen.  Paketin järjestää sovelluksen luettelo, palvelun manifest(s) ja muut tarvittavat tiedostot on tietty asettelu.  Visual Studio määrittää ja hallinnoi paketin sovelluksen projektin kansiossa 'pkg' hakemistossa.  Luo tai päivittää sovelluspaketin **paketti** napsauttamalla **sovelluksen** pikavalikosta.  Haluat ehkä tehtävä tämä, jos sovellus otetaan käyttöön mukautettu PowerShell-komentosarjojen avulla.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Poistaa sovelluksia ja sovelluksen tyypit Cloud Resurssienhallinnassa

Voit suorittaa klusterin-hallintatoiminnot Visual Studion Cloud Resurssienhallinnassa, jotka voit käynnistää **Näytä** -valikosta. Voit esimerkiksi poistaa sovelluksia ja sovelluksen lajien paikallisessa tai etätietokoneessa klustereiden laajentamisen poistaminen.

![Sovelluksen poistaminen](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

>[AZURE.TIP] Katso tehokkaan klusterin hallintatoiminnot [havainnollistetaan usein yhteyttä klusterin kangasta Resurssienhallinnassa](service-fabric-visualizing-your-cluster.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

- [Palvelun kangasta sovelluksen malli](service-fabric-application-model.md)
- [Palvelun kangasta sovelluksen käyttöönotto](service-fabric-deploy-remove-applications.md)
- [Sovelluksen parametrit useita ympäristössä hallinta](service-fabric-manage-multiple-environment-app-configuration.md)
- [Palvelun kangasta sovelluksen virheenkorjaus](service-fabric-debugging-your-application.md)
- [Kesäolympialaisten visualisointi yhteyttä klusterin palvelun kangasta Resurssienhallinnan avulla](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]:./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
