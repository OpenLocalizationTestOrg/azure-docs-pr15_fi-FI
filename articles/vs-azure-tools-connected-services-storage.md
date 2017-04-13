<properties 
   pageTitle="Azure-tallennustilan lisääminen käyttämällä yhdistetyt palvelut Visual Studiossa | Microsoft Azure"
   description="Azure-tallennustilan lisääminen sovelluksen Visual Studio Lisää yhdistetyt palvelut-valintaikkunan avulla"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Azure-tallennustilan lisääminen käyttämällä Visual Studio yhdistetyt palvelut

## <a name="overview"></a>Yleiskatsaus

Visual Studio 2015, jossa voit muodostaa minkä tahansa C# pilvipalvelussa, .NET Taustajärjestelmä mobiilipalvelu, ASP.NET-sivuston tai palvelu, ASP.NET-5-palvelun tai Azure WebJob palvelun Azure säilöön **Lisää yhdistetyt palvelut** -valintaikkunan. Yhdistetyn palvelun toimintoja Lisää kaikki tarvittavat viittaukset ja yhteyden koodi ja Muokkaa määritysten tiedostojen asianmukaisesti. Valintaikkunan myös siirryt asiakirjat, jossa kerrotaan, mitä seuraavat vaiheet ovat Blob-objektien tallennustilaan, olevien ja taulukot.

## <a name="supported-project-types"></a>Tuetut projektityypit

Yhdistetyt palvelut-valintaikkunan avulla voit muodostaa yhteyden Azuren tallennustilaan projektin seuraavanlaisia.

- ASP.NET Web-projektit

- ASP.NET 5 projektit

- .NET cloud palvelun Web rooli ja työntekijän rooli projektit

- .NET mobile-palvelujen projektit

- Azure WebJob projektit


## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Yhteyden muodostaminen Azuren tallennustilaan yhdistetyt palvelut-valintaikkunan avulla

1. Varmista, että sinulla on Azure-tili. Jos sinulla ei ole Azure-tili, voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](http://go.microsoft.com/fwlink/?LinkId=518146). Kun Azure-tili, voit luoda tallennustilan tilejä, Luo mobile-palvelujen ja Määritä Azure Active Directory.

1. Avaa projekti Visual Studiossa, **viittaukset** -solmu pikavalikon avaaminen ratkaisunhallinnassa ja valitse sitten **Lisää palvelun yhteydessä**.

    ![Yhdistetyn palvelun lisääminen](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. **Lisää yhteys palvelun** -valintaikkunassa valitse **Azuren tallennustilaan**ja valitse sitten **Määritä** -painiketta. Sinua voidaan pyytää kirjautumaan Azure, jos et ole vielä tehnyt niin.

    ![Yhdistetty palvelu-valintaikkunan - tallennustilan lisääminen](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1. **Azuren tallennustilaan** -valintaikkunassa valitse olemassa olevan tallennustilan tili ja valitse **Lisää**.

    Jos haluat luoda uuden tallennustilan tilin, siirry seuraavaan vaiheeseen. Muussa tapauksessa siirry vaiheeseen 6.

    ![Azuren tallennustila-valintaikkuna](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. Luo uusi tallennustilan seuraavasti: 

    1. Valitse **Luo uusi tallennustilan tili** -painike Azuren tallennustilaan-valintaikkunan alareunassa.

    1. Täytä **Luominen tallennustilan tili** -valintaikkunassa ja valitse sitten **Luo** -painiketta.
    
        ![Azuren tallennustila-valintaikkuna](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        Kun olet takaisin **Azuren tallennustilaan** -valintaikkunassa, uusi tallennustilan näkyy luettelossa.

    1. Valitse uusi tallennustilan luettelosta ja valitse **Lisää**.

1. Yhdistetty tallennuspalvelu näkyy kohdassa WebJob projektin palvelun viittaukset-solmu.

    ![Azure tallennustilan web työt Projectissa](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. Tarkista aloittaminen-sivu, joka tulee näkyviin, ja katso, miten projektin on muokattu. Aloittaminen-sivu tulee näkyviin selaimeen aina, kun olet lisännyt yhdistetyn palvelu. Voit tarkistaa ehdotettu seuraavat vaiheet ja koodiesimerkkejä tai mitä on tapahtunut sivulle mitä viittaukset on lisätty projektiin, ja kuinka koodi- ja tiedostojen on muokattu.

## <a name="how-your-project-is-modified"></a>Miten projektin on muokattu

Kun olet valmis valintaikkuna, Visual Studio viittaukset Lisää ja Muokkaa tiettyjä määritystiedostoja. Mitä muutoksia määräytyvät projektityyppi. 

 - ASP.NET-projektit-kohdassa [tapahtumista – ASP.NET projektit](http://go.microsoft.com/fwlink/p/?LinkId=513126). 
 - ASP.NET-5 projektit-kohdassa [Mitä tapahtui – ASP.NET 5 projektit](http://go.microsoft.com/fwlink/p/?LinkId=513124). 
 - Cloud service-projektit (web roolit ja työntekijä roolit)-kohdassa [Mitä tapahtui – pilvipalvelussa projektit](http://go.microsoft.com/fwlink/p/?LinkId=516965). 
 - Katso WebJob projektit- [tapahtumista - WebJob projektit](./storage/vs-storage-webjobs-what-happened.md).

## <a name="next-steps"></a>Seuraavat vaiheet

1. Käytön aloittaminen MALLIKOODEJA apuna, Luo tallennustilan, jota haluat tyyppi, ja kirjoita koodin tallennustilan tiliäsi kirjoittamista!

1. Kysy kysymyksiä ja ohjeita
     - [MSDN-keskustelupalsta: Azure-tallennustilan](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Azure-tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/)

     - [Tallennustilan azure.microsoft.com etsiminen](https://azure.microsoft.com/services/storage/)

     - [Tallennustilan azure.microsoft.com Documentation](https://azure.microsoft.com/documentation/services/storage/)

