
<properties
    pageTitle="Officen käytön Azure RemoteApp | Microsoft Azure" 
    description="Lue, miten Office ja Azure RemoteApp toimivat yhdessä"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-office-with-azure-remoteapp"></a>Officen käytön Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Sinulla on kaksi vaihtoehtoa isännöimiseen Azure RemoteApp Office-sovelluksissa: Office 365 ProPlus tai Office 2013 Professional Plus-kokeiluversio.

**Hei Tiesitkö, että uuden, paremmin, joka korvaa pian tämä artikkeli on? Tutustu [Office 365-tilauksen Azure RemoteApp käyttämisestä](remoteapp-officesubscription.md). Se kattaa kaikki tarvittavat tiedot, tarvitset Office 365: ssä + Azure RemoteApp avulla.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
Voit luoda käyttämällä Office 365 ProPlus suunnittelumallin kuva RemoteApp-sivustokokoelman. Tämän asetuksen avulla voit laajentaa Office 365-palvelun RemoteApp. Sinulla on oltava olemassa olevan tilauksen järjestelyn ja käyttäjien täytyy olla käyttöoikeus Office 365 ProPlus-palvelun, joko erillinen tai palvelun suunnitelmien Office 365: n kautta.

RemoteApp tukee Office 365: n jaetut tietokoneen aktivointi. Kun käyttöön jaetun tietokoneen aktivointi ja [Officen käyttöönottotyökalu](http://www.microsoft.com/download/details.aspx?id=36778) asennus-työkalun avulla, Office 365 ProPlus asentaa ilman aktivointia. Kun käyttäjä allekirjoittaa kokoelmaan, joka sisältää Office 365: ssä, Office tarkistaa, jos käyttäjä on valmisteltu for Office 365 ProPlus. Jos näin on, Office tilapäisesti Aktivoi Office 365 ProPlus - tämän aktivointi jatkuu saakka, että käyttäjät merkkejä palvelun ulos.

Voit käyttää Office 365: n jaetut tietokoneen aktivointi, sinun on [mukautetun mallin](remoteapp-create-custom-image.md) luominen ja asennettava Office 365 ProPlus, seuraamalla [näitä ohjeita](https://technet.microsoft.com/library/dn782858.aspx).

Voit hallita käyttäjien Office 365-käyttöoikeutta, [Office 365-hallintaportaalissa](https://portal.office365.com/). Lue lisätietoja [Office 365: n palvelusopimusten vaihtoehdot](http://technet.microsoft.com/library/office-365-plan-options.aspx).  


## <a name="office-2013-professional-plus-trial"></a>Office 2013 Professional Plus-kokeiluversio
30 päivän kokeiluversio RemoteApp-aikana voit Office 2013 Professional Plus (kokeiluversio) suunnittelumallin kuva RemoteApp-sivustokokoelman luomiseen. Voit määrittää käyttäjät kokeiluversion kokoelmaan Azure Active Directory-työ tilit-tai Microsoft-tilit. Lisää tilausta ei ole ei tarvita.

Tämä on hyvä tapa projektin renkaat ja hyvä feeling Officen RemoteApp. Tämä asetus on kuitenkin tarkoitettu arvioitavaksi ja testauksen vain. RemoteApp sivustokokoelmat luotu Office 2013 Professional Plus (kokeiluversio) suunnittelumallin kuva et voi olla transitioned tuotannon tilaan ja eivät ole käytettävissä kokeilujakson päättyessä.

## <a name="switching-from-trial-to-production"></a>Siirtyminen tuotannon kokeiluversio
30 päivän maksuton käynnistyessä muistiinpanon portaalin RemoteApp-osassa kertoo, kuinka pitkään olet poistunut-kokeiluversion ennen siirtymään maksettu tilille. Voit aktivoida tilisi ja vaihda tuotannon tilaan Huomautus linkin avulla.

Kun olet aktivoinut tilisi, tämä vaikuttaa kaikkien RemoteApp tililläsi kokoelmien.

- Kokoelmien, joissa on käytössä Windows Server 2012 R2: n tai Office 365 ProPlus sivustomallien kuvia saumaton siirtyminen tuotannon. Kaikki käyttäjätiedot ja asetuksia, kuten jatkuvaa istuntojen säilyvät.
- Jos olet ladannut mukautetun mallin kuvia, kuvat käyttämällä sivustokokoelmat myös siirtymä saumattomasti.
- Office 2013 Professional Plus (kokeiluversio) suunnittelumallin kuva on tarkoitettu vain arviointiin. Sivustokokoelmat käytön tämän suunnittelumallin kuva et voi olla transitioned tuotannon. Ne siirretään "käytöstä"-tilaan.


Jos ei siirtymä tuotannon tilaan kokeilujakson vanhenemisen mukainen, RemoteApp sivustokokoelmat eivät ole käytettävissä. Älä huoli - asetukset ja käyttäjien tiedot tallennetaan toisen 90 päivää, jotta voit edelleen aktivoida palvelua ja vaihda tuotannon tilaan menettämättä mitään tietoja.
