
<properties 
   pageTitle="Azure SDK .NET 2,8 julkaisutiedot" 
   description="Azure SDK .NET 2,8 julkaisutiedot" 
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
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>.NET 2,8, 2.8.1 ja 2.8.2 Azure SDK

##<a name="overview"></a>Yleiskatsaus
 
Tässä artikkelissa on julkaisutiedot (joka sisältää tunnettuja ongelmia ja uusimmat muutokset) Azure SDK-paketissa, .NET-2,8 2.8.1 ja 2.8.2 julkaisut. 

Katso luettelo kaikista uusista ominaisuuksista ja päivitykset tässä yhteydessä tehtiin [Azure SDK 2,8 2013: n Visual Studio ja Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) -ilmoitus. 

##  <a name="azure-sdk-for-net-28"></a>.NET 2,8 Azure SDK

### <a name="download-azure-sdk-for-net-28"></a>Lataa Azure SDK .NET 2,8

[Azure SDK .NET 2,8 for Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure .NET 2,8 Visual Studio 2013: n SDK](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>.NET 4.5.2 tuki 

####<a name="known-issues"></a>Tunnetut ongelmat

Azure .NET SDK 2,8 avulla voit luoda .NET 4.5.2 pilvipalvelussa paketit. Kuitenkin .NET 4.5.2 framework ei asenneta Vieras OS kuvia, kunnes tammikuussa 2016 Vieras OS Vapauta oletusarvoiseen. Ennen 4.5.2 olevat .NET framework ovat käytettävissä erillinen Vieras OS julkaisuversio – marraskuun 2015 02 kautta. Katso seurataan kuvan julkaistaan [Azure Vieras OS versioista ja SDK yhteensopivuuden matriisi](../cloud-services/cloud-services-guestos-update-matrix.md) -sivu.  Kun on julkaistu marraskuussa 2015 02 kuva voit käyttää kuvan päivittämällä pilvipalvelussa määritys-tiedosto (.cscfg)-tiedosto. Tiedoston Määritä palvelun ServiceConfiguration elementin osVersion-määritteen merkkijonoon "WA-Vieras-OS-4.26_201511-02". Jos haluat osallistua käyttää kuvassa sitten Vieras OS Automaattiset päivitykset eivät ole enää käytettävissä. Pääset Automaattiset päivitykset osVersion on määritettävä "*" ja .NET 4.5.2 vain ne ovat käytettävissä tammikuussa 2016 Automaattiset päivitykset.

###<a name="azure-data-factory"></a>Azure Data Factory

####<a name="known-issues"></a>Tunnetut ongelmat 

Aikana **Factory tietomalli** projektin luontia, joihin mallitiedot azure power liittymän komentosarja saattaa epäonnistua, jos azure power shell-versio on asennettu tietokoneeseen on 0.9.8 jälkeen.

Jotta voit luoda tällaista projektin, sinun on asennettava [azure power shell-versio 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).


### <a name="azure-resource-manager-tools"></a>Azure Resurssienhallinta-Työkalut 

####<a name="breaking-changes"></a>Tärkeimmät muutokset

PowerShell-komentosarjaa myöntämä Azure resurssiryhmä-projekti on päivitetty tässä versiossa uusi Azure PowerShellin cmdlet-komennot 1.0-version käyttöä varten.  Tämä uusi komentosarja ei toimi Visual Studiossa ennen 2,8 SDK versiota käytettäessä.  

Komentosarjojen SDK aiemmissa versioissa luotujen projektien ei suoriteta Visual Studion 2,8 SDK käytettäessä.  Kaikki komentosarjat säilyvät ulkopuolella Visual Studio Azure PowerShellin cmdlet-komennot oikean version käyttöä varten.  

2,8 SDK edellyttää Azure PowerShellin cmdlet-komennot 1.0-versiota.  Kaikki muut SDK versiot edellyttävät Azure PowerShellin cmdlet-komennot 0.9.8 versiota.  Katso lisätietoja [tämän](http://go.microsoft.com/fwlink/?LinkID=623011) blogin.

###<a name="web-tools-extensions"></a>Web-työkalut-laajennukset

####<a name="known-issues"></a>Tunnetut ongelmat

Seuraavia tunnettuja osoitetaan seuraavassa versiossa.

- Sovelluksen palvelun liittyvät Cloud ja palvelimen Explorer liikkeen tuotannon ympäristössä (esimerkiksi Azure Kiinan tai Azure pinon asiakkaat) eivät toimi. Asiakkaiden sellaisiin alueilla Julkaise profiilin lataaminen Azure-portaalista kokoustyötilaa julkaisun mahdollisuuden. Uuteen versioon korjaa eleitä, kuten "Liitä virheenkorjaus" ja "Näytä Streaming lokit" Azure kiina ja pinon asiakkaille. 
- Asiakkaiden voi tulla virheitä App palvelun luominen, kun sovellus havainnollistamisen esiintymän, jossa ne ovat käyttöönotto on alueen kuin Yhdysvaltojen Itä aikana. Näissä tilanteissa luominen sovelluksen-palvelun portaalissa ja julkaise-profiilin lataaminen kokoustyötilaa julkaisun skenaarioita. 

###<a name="azure-hdinsight-tools"></a>Azure Hdinsightiin Työkalut

####<a name="new-updates"></a>Uudet päivitykset

- Voit suorittaa kyselyn rakenne klusterin kautta HiveServer2 lähes ei katseltavan kanssa sekä tarkastella työn kirjaa reaaliaikainen.
- Uusi rakenne tehtävän suorittaminen näkymän avulla voit tarkastella työtäsi tarkempaa, enemmän ja tunnistaa mahdolliset ongelmat.

Lisätietoja on artikkelissa [Azure SDK 2,8 2013: n Visual Studio ja Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>.NET 2.8.1 Azure SDK

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013: n ja Visual Studio 2015 tunnetut ongelmat
 
1. Käynnistettävät WebJob julkaisee paikkojen näkyy Näytä ja virheen ja ei määrittäminen aikataulun, mutta se tulee siirtää WebJob Azure. Asiakkaat, jotka ovat ajoitettu työn voit käyttää Azure-portaalissa WebJob ajoituksen määrittäminen. 
2. Python asiakkaiden saattaa ilmetä virheenkorjaus ongelmia. Palvelun ryhmän juoksevan, korjaa tämän ominaisuuden, mutta jos asiakkaiden koskee, kerro Microsoft keskustelupalstoilla tai ilmoitus blogista tai vapauttaa muistiinpanojen kommentit-kohtaan. 
3. Asiakkaiden tietyillä alueilla (kuten Etelä Intia) ilmenee, sovelluksen palvelun valmisteluvirheitä. Tämä on portaalin johdonmukaisia ja ongelman kohtaavat asiakkaat avulla geo näiden alueiden julkaiseminen access pyytää Azure portaalin. Kun nämä alueita käyttämällä käyttöoikeuksien pyytäminen ne Azure portaalin valmistelu pitäisi toimia. 

##<a name="azure-sdk-for-net-282"></a>.NET 2.8.2 Azure SDK

2.8.2 asennusta seuraavan seuraavat ongelma voi ilmetä Työkalut-asiakkaille.         

- Jos käytät Windows 10: ssä ja ei ole asennettu Internet Explorer, näkyviin voi tulla "Internet Explorerin ei löydy"-virhe.
Voit ratkaista ongelman asentamalla Internet Explorerin Lisää tai poista Windowsin osia-valintaikkunan.

Jos huomaat ongelman, voit raportoida asiasta Send smile-toiminnon avulla.

Lisätietoja on artikkelissa [tämän](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) viestin.
##<a name="other-updates"></a>Muut päivitykset

Katso muita päivityksiä [Azure SDK 2,8 ilmoitus viestin](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

##<a name="also-see"></a>Katso myös

[Azure SDK 2,8 ilmoitus viestiin](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Tuki- ja käytöstä poistaminen lisätietoja Azure SDK .NET ja ohjelmointirajapinnan](https://msdn.microsoft.com/library/azure/dn479282.aspx)

