<properties 
    pageTitle="Käytä ReportViewer sivuston | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan muodostaminen Microsoft Azure-sivuston kanssa Visual Studio ReportViewer-ohjausobjektia, jossa Microsoft Azure-Virtual-Machine tallennettuun raporttiin."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Azure-Web-sivuston ReportViewer käyttäminen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Voit luoda Microsoft Azure-sivuston kanssa Visual Studio ReportViewer-ohjausobjektia, jossa Microsoft Azure-Virtual-Machine tallennettuun raporttiin. ReportViewer-ohjausobjekti on Web-sovelluksen luominen ASP.NET-Web-sovelluksen mallin avulla.

>[AZURE.IMPORTANT] ASP.NET-MVC verkkosovelluksen mallit eivät tue ReportViewer-ohjausobjektin.

Voi liittää ReportViewer Microsoft Azure-Web-sivuston, sinun täytyy suorittaa seuraavat tehtävät.

- **Lisää** Asennuspaketin ASENNELMIEN

- **Asetusten määrittäminen** Todennus- ja

- **Julkaise** Azure ASP.NET verkkosovelluksen

## <a name="prerequisites"></a>Edellytykset

Tarkista [SQL Server-liiketoimintatietojen Azuren näennäiskoneiden](virtual-machines-windows-classic-ps-sql-bi.md)"Yleiset suositus ja parhaita käytäntöjä"-kohdassa.

>[AZURE.NOTE] ReportViewer ohjausobjektit toimitetaan, jossa Visual Studiossa, Standard Edition tai uudempi versio. Jos käytössäsi on Web Developer Express Edition-on asennettava [MICROSOFT raportin tarkastelu 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) ReportViewer runtime-ominaisuuksia.
>
>Microsoft Azure ei tueta ReportViewer määritetty paikallisen käsittely-tilassa.

Tutustu [Reporting Services-raporttien katseluohjelman ohjausobjekti ja virtuaalikoneen Microsoft Azure-pohjaiset raportin palvelinten](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)laadinta.

## <a name="adding-assemblies-to-the-deployment-package"></a>Kokoonpanojen lisääminen asennuspaketin

Jos isännöit oman ASP.NET sovelluksen paikallinen, ReportViewer kokoonpanon on yleensä asennettu suoraan IIS-palvelimen kokoonpanovälimuistiin (GAC) Visual Studio asennuksen aikana ja niitä voi käyttää suoraan soveltamalla. Kuitenkin kun isännöit ASP.NET-sovelluksen pilveen, Microsoft Azure ei salli mitään voidaan asentaa GAC, joten varmista ReportViewer kokoonpanon ovat käytettävissä paikallisesti sovelluksen. Voit tehdä tämän lisäämällä viittaukset niihin projektin ja määrittää niitä kopioidaan paikallisesti.

Etäkäyttö tilassa ReportViewer-ohjausobjektin käyttää seuraavia kokoonpanon:

- **Microsoft.ReportViewer.WebForms.dll**: sisältää ReportViewer-koodi, jota haluat käyttää ReportViewer-sivulle. Viittauksen tämän kokoonpanon lisätään projektiin, kun pudotat projektin ReportViewer ohjausobjektin ASP.NET-sivulle.

- **Microsoft.ReportViewer.Common.dll**: sisältää luokista, joita ReportViewer-ohjausobjektin suorituksen aikana. Se ei automaattisesti lisätä projektiin.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Jos haluat lisätä Microsoft.ReportViewer.Common viittaus

- Projektin **viittaukset** solmu hiiren kakkospainikkeella ja valitse **Lisää viittaus**, valitse kokoonpanon .NET-välilehti ja valitse **OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Varmista kokoonpanon paikallisesti helppokäyttöisyys ASP.NET-sovelluksessa

1. Valitse **viittaukset** -kansiossa Microsoft.ReportViewer.Common kokoonpanon niin, että sen ominaisuuksia näkyvät ominaisuudet-ruudussa.

1. Valitse ominaisuudet-ruudussa määrittämällä arvoksi True **Paikallisen kopion** .

1. Toista vaiheet 1 ja 2 Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>ReportViewer kielipaketin hankkiminen

1. Asenna tarvittavat Microsoft raportin tarkastelu 2012 Runtime redistributable paketti [Microsoft Download](http://go.microsoft.com/fwlink/?LinkId=317386)Centeristä.

1. Valitse avattavasta luettelosta haluamasi kieli ja sivun uudelleenohjataan vastaavan center lataussivulle.

1. Valitse Aloita ReportViewerLP.exe lataaminen **Lataa** .

1. Kun olet ladannut ReportViewerLP.exe, valitse **Suorita** asentaminen välittömästi, ja valitse **Tallenna** , tallenna se tietokoneeseen. Jos valitset **Tallenna**, muista kansio, johon tallennat tiedoston nimi.

1. Etsi kansio, johon tallensit tiedoston. Napsauta ReportViewerLP.exe hiiren kakkospainikkeella, valitse **Suorita järjestelmänvalvojana**ja valitse sitten **Kyllä**.

1. Kun olet suorittanut ReportViewerLP.exe, näet c:\windows\assembly on resurssin **Microsoft.ReportViewer.Webforms.Resources** ja **Microsoft.ReportViewer.Common.Resources**tiedostot.

### <a name="to-configure-for-localized-reportviewer-control"></a>Voit määrittää lokalisoitu ReportViewer hallinta

1. Lataa ja asenna Microsoft raportin tarkastelu 2012 Runtime redistributable paketti edellä määritetyn ohjeita noudattamalla.

1. Luo <language> projekti ja kopioi kansioon liittyvän resurssin kokoonpanon tiedostot siellä. Kopioidaan resurssin kokoonpano-tiedostot ovat: **Microsoft.ReportViewer.Webforms.Resources.dll** ja **Microsoft.ReportViewer.Common.Resources.dll**. Kokoonpanon resurssitiedostojen ja valitse Ominaisuudet-ruudussa Määritä **Kopioi tulosteen kansioon** **aina kopioida"**.

1. Voit määrittää Culture & UICulture web-projekti. Katso lisätietoja siitä, miten voit määrittää kulttuuri ja Käyttöliittymän kieliasetus ASP.NET Web-sivun [Toimintaohje: kulttuuri ja Käyttöliittymän kieliasetus määrittäminen ASP.NET Web-sivun globalisointi](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Todennus- ja määrittäminen

ReportViewer tarvitaan myönnät käyttöoikeudet raporttipalvelimen todentamismenetelmä ja tunnistetiedot on oltava oikeus raporttipalvelin haluat käyttöraportteja. Lisätietoja käyttöoikeuksien dokumentissa [Reporting Services-raporttien katseluohjelman ohjausobjekti ja Microsoft Azure virtuaalikoneen perusteella raportti-palvelimiin](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>Julkaise Azure ASP.NET verkkosovelluksen

Katso ohjeet ASP.NET-Web-sovelluksen Azure julkaiseminen [miten: siirtäminen ja julkaista Azure verkkosovelluksen Visual Studio](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) ja [Web Apps -sovellusten ja ASP.NET käytön aloittaminen](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Lisää Azure käyttöönoton tai lisätä Azure Cloud palvelun projektiin-komento ei näy pikavalikon Napsauta ratkaisunhallinnassa, jos joudut ehkä muuttamaan kohde framework projektin .NET Framework 4.
>
>Kaksi komentoja on käytettävä samoja toimintoja. Yhden tai muita komento tulee näkyviin pikavalikon sen mukaan, mikä Microsoft Azure SDK-versio asennettuna.

## <a name="resources"></a>Resurssit

[Microsoft-raportit](http://go.microsoft.com/fwlink/?LinkId=205399)

[Azuren näennäiskoneiden SQL Server-liiketoimintatietojen](virtual-machines-windows-classic-ps-sql-bi.md)

[Luo Azure AM alkuperäistilassa raporttipalvelimen PowerShellin avulla](virtual-machines-windows-classic-ps-sql-report.md)

[Reporting Services-raporttien katseluohjelman ohjausobjekti ja Microsoft Azure virtuaalikoneen perusteella raportti palvelimet](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
