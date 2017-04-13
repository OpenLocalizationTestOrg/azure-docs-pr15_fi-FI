<properties
    pageTitle="Azure julkishallinnon palvelut | Microsoft Azure"
    description="Lisätietoja Azure Government-tilauksen hallinta"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Hallinta ja yhteyden Azure Government-tilaukseen

Azure hallitus on yksilöllinen URL-osoitteet ja päätepisteet ympäristön hallintaan. On tärkeää oikean yhteydet avulla voit hallita ympäristösi portal tai PowerShellin kautta. Kun olet muodostanut yhteyden Azure Government-ympäristössä, Normaali toimintoja palvelun hallinta toimii, jos osa on otettu käyttöön.

## <a name="connecting-via-the-portal"></a>Yhteyden muodostaminen portaalin kautta
Portaalissa on ensisijainen tapa, jolla Useimmat käyttäjät muodostavat yhteyden Azure Government.  Jos haluat yhdistää, Selaa-portaaliin osoitteessa [https://portal.azure.us](https://portal.azure.us).  Azure-portaalin vanha versio käyttää [https://manage.windowsazure.us](https://manage.windowsazure.us)kautta.

Tilin voi luoda tilauksia muodostamalla yhteyden [https://account.windowsazure.us](https://account.windowsazure.us).

## <a name="connecting-via-powershell"></a>Yhteyden muodostaminen PowerShellin kautta

Käytätkö PowerShellin Azure hallittavan suuri project_pro_noversion komentosarjan tai käyttää ominaisuuksia, jotka eivät ole vielä käytettävissä, sinun on muodostettava Azure Government sijaan Azure julkisen Azure-portaalissa.  Jos olet käyttänyt PowerShellin Azure julkinen, se on lähinnä sama.  Azure Government erot ovat seuraavat:

+ Yhteyden tiliisi
+ Alueiden nimet

>[AZURE.NOTE] Jos et ole vielä käyttänyt PowerShell, katso [Johdanto PowerShellin Azure](../powershell-install-configure.md).

Onko muodostaa Azure Government ympäristön-parametrin määrittämällä PowerShellin Azure sinulla PowerShell käynnistyessä.  Parametrin varmistaa, että PowerShell muodostaa yhteyden oikea päätepisteet.  Päätepisteet sivustokokoelman määritetään, kun muodostat yhteyden tiliisi kirjautuminen.  Eri API tarvitaan ympäristön Vaihda eri versioita:

Yhteyden tyyppi | Komento
---|----
[Hallinta](https://msdn.microsoft.com/library/dn708504.aspx) -komennot | `Add-AzureAccount -Environment AzureUSGovernment`
[Resurssienhallinta](https://msdn.microsoft.com/library/mt125356.aspx) -komennot | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
[Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) -komennot | `Connect-MsolService -AzureEnvironment UsGovernment`
[V2 Azure Active Directory-komento](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

Voi myös käyttää ympäristön valitsinta käyttämällä uutta AzureStorageContext tallennustilan-tiliin yhdistettäessä ja määritä AzureUSGovernment.

### <a name="determining-region"></a>Sen selvittäminen alue

Kun yhteys on muodostettu, on yksi muita ero – alueiden kohdeluettelo-palvelun avulla.  Jokaisen Azure cloud on eri alueilla.  Näet ne käytettävyys-sivulle.  Alueen sijainti-parametrissa käytetään yleensä komennon.

On yksi todellisen.  Azure Government-alueilla on muotoiltu eri tavalla kuin yleisiä nimensä:

Yleinen nimi | Komento
---|----
Yhdysvaltain gov – Virginia | USGov Virginia
Yhdysvaltain gov – Iowa | USGov Iowa

>[AZURE.NOTE] Ei ole välilyöntiä Yhdysvallat – gov – käytettäessä sijainti-parametria.

Jos haluat tarkistaa Azure Government käytettävissä alueiden koskaan, voit suorittaa seuraavat komennot ja Tulosta nykyinen luettelo:

    Get-AzureLocation

Jos ole käytettävissä ympäristöissä Kiinnostaako Azure kautta, voit suorittaa:

    Get-AzureEnvironment

## <a name="next-steps"></a>Seuraavat vaiheet

Jos etsit tietoja, voit tarkistaa:

+ [GitHub PowerShell-tiedostoja](https://github.com/Azure/azure-powershell)
+ [Vaiheittaiset ohjeet muodostamisesta Resurssienhallinta](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [MSDN Azure PowerShell-tiedostoja](https://msdn.microsoft.com/library/mt619274.aspx)

Lisätietoa ja päivitykset tilaaminen [Microsoft Azure Government blogin] (https://blogs.msdn.microsoft.com/azuregov/)
