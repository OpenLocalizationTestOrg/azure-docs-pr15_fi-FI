
<properties 
    pageTitle="Office 365-tilauksen käyttäminen Azure RemoteApp | Microsoft Azure"
    description="Lue käyttämisestä Office 365-tilauksen Azure RemoteApp jakaa Office-sovellukset."
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Office 365-tilauksen käyttäminen Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Tiesitkö, että voit käyttää olemassa olevan Office 365-tilauksen Azure RemoteApp jakaa pilven Office-sovelluksia? Lue lisätietoja Office 365: ssä + Azure RemoteApp asetukset, mukaan lukien linkkejä Office 365-aiheisten artikkelien, joiden avulla voit tehdä irti tilauksen.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Miten Office 365-tilien käyttäminen Azure RemoteApp?
Tutustu Peter henkilön kaikki tiedot uuteen artikkelista: [käyttämisestä Azure RemoteApp kanssa Office 365-käyttäjätilit](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Suorita Office-sovellusten Azure RemoteApp Office 365-tilauksen avulla?

Kyllä! Itse asiassa käyttäminen Office 365-tilauksesi on ainoa tapa tuoda Office-sovellukset Azure RemoteApp.

(Huomautus: Jos Azure RemoteApp-käyttöönoton toimitetaan mukaan isännöintipalvelu kumppani, ne voi antaa sinulle Office-käyttöoikeuksien perusteella [Tarjoajan käyttöoikeussopimus](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))


Tietoja Office 365-tilauksen hyvien vaihe on, että sen avulla voit saman käyttöoikeus käyttäminen useissa eri ympäristöissä ja Azure pilveen ympäristöissä. Kun käytät Office-sovellusten Azure RemoteApp ei tarvitse ostaa lisäkäyttöoikeuksia tai määrittää aiemmin käyttöoikeuksien määräten minkäänlaista. Sinulla on Office 365-tilauksen, joka sisältää [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx).

Office 365 ProPlus mahdollistaa [jaetun tietokoneen aktivointi](https://technet.microsoft.com/library/Dn782860.aspx) – tämän ominaisuuden avulla tilapäinen käyttäjän aktivoimisessa Office-virtual ja cloud-ympäristössä, kuten Azure RemoteApp (ja Etätyöpöytäpalvelut).

Mitä Office 365-Palvelupaketit sisältävät Office 365 ProPlus? Lisätietoa [palvelun käytettävyyttä kunkin suunnitelman](https://technet.microsoft.com/library/office-365-plan-options.aspx) taulukossa. Huomaa, että kaikki-Palvelupaketit sisältävät Office 365 ProPlus (esimerkiksi Office 365 Business-palvelupaketti). Jos palvelupakettisi ei tapahdu, harkitse päivittämistä palvelupakettiin, joka tekee (esimerkiksi Office 365 Education E3).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>OK, niin kuinka on oma Office 365 ProPlus käyttöoikeuksien Azure RemoteApp käyttämisestä?

Kunkin käyttäjän käyttöoikeutta Office 365 ProPlus-palvelun avulla yksittäisen käyttäjän Aktivoi Office-sovellusten enintään 5 tietokoneissa plus taulutietokoneisiin ja matkapuhelimiin. Kunkin aktivointi on rekisteröity käyttäjä, kunnes ne poistanut Office-laitteessa. (Käyttäjät voivat hallita [Office 365-portaalin](https://portal.office365.com/)laitteensa.)

Azure RemoteApp yksittäisen käyttäjän voi kirjautua usean tietokoneen samana päivänä Luulit sitä. Tämä johtuu siitä, että palvelun hallitsee ja Skaalaa resurssien pilvipalvelussa, kun käyttäjä näkee vain sovellukset ja ohjelmat, jotka olet jakanut automaattisesti. Katso artikkelista Office 365 ProPlus tarjoaa jaetun tietokoneen aktivoinnin tilan – Tämä tarkoittaa sitä, käyttäjän ei tarvitse tehdä mitään käyttöoikeuksien hallinta ja resursseja, yksittäiset tietokoneet ei lasketaan mukaan 5 tietokoneen Aktivointirajoitus.

Kun (järjestelmänvalvojat) voit määrittää Office 365 ProPlus käyttöoikeuksia käyttäjille, he voivat käyttää Office Omat laitteensa- ja Azure RemoteApp-sivustokokoelman kautta.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Mitä Office-sovellukset voivat käyttää Office 365: ssä ja Azure RemoteApp?

Voit käyttää Office 365-tilauksen aktivoiminen ja jakaa Office 2013: n Azure RemoteApp käyttöönotoissa. Microsoft ei tue muiden Azure RemoteApp Office-versioiden käyttö. Tämä sisältää Office 2003: n, Office 2007: n, Office 2010: n ja Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Mitä tietoja Visio Pro tai Project Pro?

Office 365 ProPlus kuva sisältyvät RemoteApp-tilauksesi sisältää Visio Pro ja Project Pro. Mutta et voi käyttää Office 365 ProPlus-tilauksen aktivoiminen kyseisissä ohjelmissa - laitteilla on oman käyttöoikeuden. Voit aktivoida [Office 365-portaalissa](https://portal.office365.com/). 

Sinun ei tarvitse käyttöoikeuden ohjelmat, jos et halua käyttää niitä. Aktivoi vain use - ja jättää muut ohjelmat. Voit tarkastella niitä silti kuvassa, mutta et voi käyttää niitä. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Miten aloitetaan Office 365: ssä ja Azure RemoteApp?

Nyt kun osaat tietoja Office 365-käyttöoikeuksien myöntämisestä, oletetaan, että pääset alkuun käyttämällä Azure RemoteApp - on hyvin helppoa:

Kun luot Azure RemoteApp-sivustokokoelman, käytä **Office 365 ProPlus-(edellyttää tilausta)** kuvaa.

![Office 365 Pro Plus Azure RemoteApp kuva](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


Tämä kuva on uusin versio Windows Server- ja Office 365 Proplusissa. Kun olet määrittänyt kokoelma (mukaan lukien julkaisun sovellukset) käyttäjien yksinkertaisesti Kirjaudu Azure RemoteApp (käyttämällä niiden RemoteApp-asiakas) ja anna minkä tahansa Office-sovellusten Office 365-tunnistetietoja. Käyttöoikeuksien toimitetaan automaattisesti kaikki ilman tai hallinta pakollinen.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Voinko luoda mukautetun kuvan Office 365 ProPlus-palvelun kanssa?

Voit luoda mukautetun kuvan kokoelma, joka sisältää Office 365 ProPlus. Vaihtoehtoja on 2 – Käytä annamme Azure valikoima tai voit luoda oman mukautetun kuvan ja asenna Office 365 ProPlus siellä.

### <a name="use-the-azure-gallery-image"></a>Käytä Azure valikoima

Helpoin tapa ottaa käyttöön kokoelman Office 365 ProPlus on [aloittaa Azure valikoiman kuvia](remoteapp-image-on-azurevm.md) Azure RemoteApp-tilaukseen sisältyy. Varmista, että valitset **esiasennettuna Windows Server Remote Desktop istunnon isäntään kanssa Office 365 ProPlus** -kuva. Asenna muista sovelluksista, jonka haluat, että kuva ja olet hyvä huomioida missä.

### <a name="use-a-custom-image"></a>Käytä mukautettua kuvaa

Voit luoda mukautetun kuvan – voit luoda [Azure AM](remoteapp-image-on-azurevm.md) tai [Luo paikallisesti kuva](remoteapp-create-custom-image.md) ja lataa se Azure. Kummassakin tapauksessa Varmista, että olet asentanut Office 365 ProPlus jaetun tietokoneen aktivointi solmun avulla. [Officen käyttöönottotyökalun](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) avulla ja noudata [näyttöön tulevia ohjeita](https://technet.microsoft.com/library/Dn782858.aspx) asennuksen.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Poista Automaattiset päivitykset käytöstä for Office 365 ProPlus mukautetun kuvan - tärkeää

Mukautetun kuvan käytetään Azure RemoteApp mallina, lisäresursseja lisäämisellä demand käyttäjien kasvaa. Voit estää viiveitä ja yhteysongelmien, käytöstä Automaattiset päivitykset Office kuvassa. Jos et ole, ja valitse jokaisen resurssin mallin avulla luotuja päivittyy automaattisesti, kun se käynnistetään. Käytä vakio Azure RemoteApp-prosessin mukautetun kuvan päivittämistä varten. Sen mukaan, Päivitä Office-sovellusten kerran-suunnittelumallin kuva ja anna sitten Azure RemoteApp huolehtia käytön päivitykset käyttäjille.

Käytöstä Automaattiset päivitykset, Lisää Officen käyttöönottotyökalun kokoonpanotiedosto seuraavasti:

        <Updates Enabled="FALSE" />

Nyt määritys-tiedostossa on oltava seuraavista riveistä:
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Miten kuvan voit päivittää Office 365 ProPlus-palvelun kanssa?

On useita syitä päivittää kuvaa kokoelmasta. Seuraavassa on todella:

- Uusimmat Windows-päivitykset 
- Saat uusimman Office 365 ProPlus sovelluksen päivitykset
- Mukautetun sovelluksen päivittäminen
- Kuva itse määritysten muiden asetusten muuttaminen

Lopusta loppuun-kohdasta kokoelmaa, jos haluat käyttää kuvaa, voit päivittää päivittämistä varten go [tähän](remoteapp-update.md). Mutta tietojen päivittämisestä kuva ja Office 365 ProPlus, lue seuraavat tiedot.

Sinulla on kaksi vaihtoehtoa: kuvan päivittäminen - korvata kuvan kokonaan uuden tai manuaalisesti päivittää olemassa olevan kuvan.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>Kuvan korvaaminen uusimman Azure valikoima + mukautusten lisääminen
Tällä asetuksella avulla Microsoft huolehtia Windows Server- ja Office 365 ProPlus-päivitykset. Sen sijaan, että päivitystä luodun kuvan, luo kokonaan uuden kuvan uusimman valikoima perusteella. Toista kaikki ohjeita kuin ennen kuin voit mukauttaa kuvan - mukautettujen sovellusten asentaminen Muokkaa Kuva määritysten jne.

Valikoiman kuvia päivitetään säännöllisesti, joten voit pidät helposti, tiedät, että Windows Server- ja Office 365 ProPlus-sovellukset ovat ajan tasalla. Trade-off on tarvitse käyttää mukautukset aina, kun saat uuden kuvan. Voit luoda automatisoida määrittäminen mukautukset.

### <a name="manually-update-your-existing-image"></a>Olemassa olevan kuvan manuaalinen päivittäminen

Kun tämä asetus käyttää vakio Windows-työkaluja päivitysten kuvan. Käytä for Office 365 ProPlus Officen käyttöönottotyökalun Lataa ja asenna uusimmat päivitykset tai Office 365 ProPlus-ratkaisussa.

> [AZURE.IMPORTANT] Muista – poistaminen käytöstä Office 365 ProPlus Automaattiset päivitykset.

Tarvitsetko lisätietoja Officen käyttöönottotyökalun avulla päivitykset?

- [Office 365-tuotteiden pika-asennuksen käyttöönottaminen Officen käyttöönottotyökalun avulla](https://technet.microsoft.com/library/JJ219423.aspx)
- [Käyttöönotto ja päivittäminen Office 365 ProPlus Officen käyttöönottotyökalun avulla](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (video)
- [Päivitä Office 365 ProPlus asetusten määrittäminen](https://technet.microsoft.com/library/dn761708.aspx)
