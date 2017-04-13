<properties 
   pageTitle="Azure Active Directory lisääminen käyttämällä yhdistetyt palvelut-Visual Studiossa | Microsoft Azure"
   description="Lisää Azure Active Directory Visual Studio Lisää yhdistetyt palvelut-valintaikkunan avulla"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="active-directory"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Azure Active Directory lisääminen käyttämällä yhdistetyt palvelut-Visual Studio 

##<a name="overview"></a>Yleiskatsaus
Azure Active Directory (Azure AD) avulla voit tukea Single Sign-On (SSO) ASP.NET MVC verkkosovellukset tai AD todennus verkko-Ohjelmointirajapinnan Services-palveluissa. Azure AD-todennuksen että käyttäjät voivat käyttää niiden tilejä Azure AD muodostaa web-sovellusten. Verkko-Ohjelmointirajapinnan kanssa Azure AD-todennus on hyötyä sisällyttää parannettu tietojen suojauksen, kun paljastaa Ohjelmointirajapinnan WWW-sovelluksesta. Azure AD-kanssa ei tarvitse erillistä todennusta-järjestelmän Oma tili- ja hallinnan hallintaan.

## <a name="supported-project-types"></a>Tuetut projektityypit

Voit muodostaa yhteyden Azure AD-projektin seuraavanlaisia yhdistetyt palvelut-valintaikkunassa.

- ASP.NET-MVC projektit

- ASP.NET-verkko-Ohjelmointirajapinnan projektit


### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Yhteyden muodostaminen Azure AD yhdistetyt palvelut-valintaikkunan avulla

1. Varmista, että sinulla on Azure-tili. Jos sinulla ei ole Azure-tili, voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Avaa pikavalikko **viittaukset** solmun projektin Visual Studiossa, ja valitse **Lisää yhdistetyt palvelut**.
1. Valitse **Azure AD-todennus** ja valitse sitten **Määritä**.

    ![Valitse Lisää Azure AD-todennus](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. **Määritä Azure AD-todennus**ensimmäisellä sivulla Tarkista **määrittäminen kertakirjautumisen Azure AD avulla**.

    Jos projektissa on määritetty toisen todennuksen määrittäminen, ohjattu toiminto varoittaa, että jatkuvaa käytöstä Edellinen kokoonpano.

    ![Määritä Azure AD ohjatussa](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  Valitse toimialue toisella sivulla **toimialue** avattavasta luettelosta. Toimialueiden luettelo on kaikkien käytettävissä toimialueiden luettelossa Tiliasetukset-valintaikkunassa tilin. Vaihtoehtoisesti voit kirjoittaa toimialuenimi Jos et löydä etsimääsi, kuten mydomain.onmicrosoft.com yksi. Voit valita asetus, jos haluat luoda uuden Azure AD-sovelluksen tai asetuksia Azure AD-sovelluksesta. 

    ![Määritä Azure AD ohjatussa](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. Ohjatun toiminnon kolmannella sivulla Varmista, että **lukea kansion tietoja** on valittuna. Ohjattu toiminto täyttää **asiakkaan salaisuus**. 

    ![Määritä Azure AD ohjatussa](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Valitse **Valmis** -painiketta. Valintaikkunan Lisää tarvittavat määritykset koodi ja viittausten käyttöön projektin Azure AD-todennusta varten. Näet [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)AD-toimialueen.

1. Tarkista aloittaminen-sivu, joka tulee näkyviin selaimeen käsittelevässä seuraavat vaiheet ja mitä on tapahtunut-sivulta, miten projektin on muokattu. Jos haluat tarkistaa, että kaikki työskenteli, Avaa jokin muokattu määritys-tiedostot ja varmista, että mainitusta mitä on tapahtunut asetukset ovat käytettävissä. Esimerkiksi tärkeimmät web.config ASP.NET MVC projektissa on lisätty asetuksia:

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Miten projektin on muokattu

Kun suoritat ohjatun toiminnon, Visual Studio Lisää Azure AD ja liittyvän projektin viittauksia. Määritysten ja koodin tiedostot projektin myös muokata Lisää Azure AD-tuki. Tiettyjä muutoksia, joka tekee Visual Studio määräytyvät projektityyppi. Saat tietoja siitä, miten ASP.NET MVC projektien muokataan, [Mitä on tapahtunut – MVC projektit](http://go.microsoft.com/fwlink/p/?LinkID=513809). Saat verkko-Ohjelmointirajapinnan projektit- [tapahtumista – Web-Ohjelmointirajapinnan projektit](http://go.microsoft.com/fwlink/p/?LinkId=513810).

##<a name="next-steps"></a>Seuraavat vaiheet

Kysy kysymyksiä ja ohjeita.

 - [MSDN-keskustelupalsta: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Azure AD-asiakirjat](https://azure.microsoft.com/documentation/services/active-directory/)

 - [Blogimerkintä: Azure AD esittely](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

