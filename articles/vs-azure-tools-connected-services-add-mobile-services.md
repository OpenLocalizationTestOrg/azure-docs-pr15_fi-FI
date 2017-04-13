<properties 
   pageTitle="Lisääminen Mobile-palveluita käyttämällä yhdistetyt palvelut-Visual Studiossa | Microsoft Azure"
   description="Lisää Mobile palvelut Visual Studio Lisää yhdistetyt palvelut-valintaikkunan avulla"
   services="visual-studio-online"
   documentationCenter="na"
   authors="mlhoop"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="12/16/2015"
   ms.author="mlearned" />

# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Lisääminen Mobile-palveluita käyttämällä Visual Studio yhdistetyt palvelut

Visual Studio 2015, jossa voit muodostaa yhteyden Azure Mobile-palveluihin **Lisää yhdistetty palvelu** -valintaikkunasta. Voit muodostaa minkä tahansa C# asiakas-sovelluksen, kaikki JavaScript-sovelluksen tai Office kaikissa ympäristöissä Cordova sovelluksen. Kun muodostat yhteyden, voit luoda ja käyttää tietoja, luoda mukautetun ohjelmointirajapinnan ja ajoitetuissa tai lisätä push-ilmoitusten tuki.  Yhdistetyt palvelut-toiminto lisää kaikki tarvittavat viittaukset ja yhteyden koodi. Voit myös hyödyntää valmiin tuen todennustavaksi, jossa on erilaisia Suositut identity-mallit, kuten Azure AD-Facebookiin, Twitteriin ja Microsoft Accounts.

## <a name="supported-project-types"></a>Tuetut projektityypit

>[AZURE.NOTE] Visual Studio 2015 Azure Mobile palvelujen lisääminen Windows Yleinen (Windows 10) projektien Lisää yhdistetyt palvelut-valintaikkunan ei tueta. Voit lisätä tarvittavat paketit NuGet paketin hallintaa käyttämällä projektin asentamalla Azure Mobile-palvelut.

Yhdistetyt palvelut-valintaikkunan avulla voit muodostaa yhteyden Azure Mobile-palveluihin projektin seuraavanlaisia.

- .NET Windows 8.1-kaupan, puhelinnumero ja Yleinen sovelluksen projektit

- JavaScript-Windows 8.1-kaupan, puhelinnumero ja Yleinen sovelluksen projektit

- Visual Studio Tools for Apache Cordova avulla luotuja projekteja


## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Yhteyden muodostaminen Azure Mobile-palveluihin Lisää yhdistetyt palvelut-valintaikkuna

1. Varmista, että sinulla on Azure-tili. Jos sinulla ei ole Azure-tili, voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Avaa **Lisää yhdistetyt palvelut** -valintaikkuna.
 - .NET-sovellusten projektin avaaminen Visual Studiossa, **viittaukset** -solmu pikavalikon avaaminen ratkaisunhallinnassa ja valitse sitten **Lisää yhteys palvelun**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - Apache Cordova sovelluksen projekteille projektin avaaminen Visual Studiossa, Avaa project-solmu pikavalikko ratkaisunhallinnassa ja valitse sitten **Lisää yhteys palvelun**.

1. **Lisää yhteys palvelun** -valintaikkunassa valitse **Azure Mobile-palveluihin**ja valitse sitten **Määritä** -painiketta. Sinua voidaan pyytää kirjautumaan Azure, jos et ole vielä tehnyt niin.

    ![Lisäämällä Azure mobiilipalvelu](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1. Jos käytössäsi on jokin, valitse aiemmin mobiilipalvelu **Azure Mobile-palvelut** -valintaikkunassa. Jos haluat luoda uuden Azure mobiilipalvelu, voit tehdä alla olevien ohjeiden mukaan. Muussa tapauksessa siirry seuraavaan vaiheeseen.

    Luo uusi mobiilipalvelu seuraavasti:
    1. Valitse valintaikkunan alaosassa **Luo Service **-linkki.
        ![Lisää uusi yhdistetyn mobiilipalvelu](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. **Luo mobiilipalvelun ominaisuudet** -valintaikkunassa voit valita JavaScript Taustajärjestelmä mobiilipalvelu tai .NET Taustajärjestelmä mobiilipalvelu **Runtime** avattavasta luettelosta. 
  
        ![Luominen mobile-palvelu](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        JavaScript-taustatietokannan-palvelu on yksinkertainen ja tehokas. Jos luot Taustajärjestelmä mobiilipalvelu JavaScript-palvelinpuolen JavaScript-koodia on tallennettu pilvipalveluun, mutta voit muokata palvelimen komentosarjojen palvelimen Explorer tai Azure hallinta-portaalin avulla. 

        .NET Taustajärjestelmä mobiilipalvelu avulla voit käyttää koko power ja verkko-Ohjelmointirajapinnan ja kohteen Framework joustavuutta. Jos luot .NET Taustajärjestelmä mobiilipalvelu, projektin on luonut puolestasi ja ratkaisu lisätään. 

    1. Valitse mobiilipalvelun haluamaasi **aluetta** ja kirjoita sitten palvelimen käyttäjänimi ja salasana.
 
    1. Kun olet lisännyt kaikki tarvittavat tiedot, valitse Luo mobiilipalvelun **Luo** -painike.
    2. Uusi mobiilipalvelu pitäisi näkyä Palveluluettelo **Azure Mobile-palvelut** -valintaikkunassa. Valitse uusi mobiilipalvelu luettelosta ja valitse sitten **Lisää** -painiketta palvelun lisääminen projektiin.
    

1. Tarkista, käytön aloittaminen-sivu, joka tulee näkyviin ja katso, miten projektin on muokattu. Aloittaminen-sivu tulee näkyviin selaimeen aina, kun olet lisännyt yhdistetyn palvelu. Voit tarkistaa ehdotettu seuraavat vaiheet ja koodiesimerkkejä tai mitä on tapahtunut sivulle mitä viittaukset on lisätty projektiin, ja kuinka koodi- ja tiedostojen on muokattu.

1. Käytä oppaan MALLIKOODEJA, kirjoittaa koodin käyttämään mobile-palvelu!

## <a name="how-your-project-is-modified"></a>Miten projektin on muokattu

Miten Visual Studio Muokkaa projektin määräytyy projektityyppi. Katso C# asiakassovellukset, [Mitä tapahtui – C# projektit](http://go.microsoft.com/fwlink/p/?LinkId=513119). Katso JavaScript-asiakassovellukset [tapahtumista – JavaScript-projektit](http://go.microsoft.com/fwlink/p/?LinkId=513120). Katso Cordova sovellukset [tapahtumista – Cordova projektit](http://go.microsoft.com/fwlink/p/?LinkId=513116).


##<a name="next-steps"></a>Seuraavat vaiheet

Kysy kysymyksiä ja ohjeita: 

 - [MSDN-keskustelupalsta: Azure Mobile-palvelujen](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [Azure Mobile palveluiden Microsoft Azure tiimin blogia](https://azure.microsoft.com/blog/topics/mobile/)

 - [Azure Mobile-palvelujen azure.microsoft.com etsiminen](https://azure.microsoft.com/services/mobile-services/)

 - [Azure Mobile-palvelujen azure.microsoft.com Documentation](https://azure.microsoft.com/documentation/services/mobile-services/)



