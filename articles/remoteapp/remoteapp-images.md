<properties
    pageTitle="Mikä on Azure RemoteApp sivustomallien kuvia? | Microsoft Azure"
    description="Lisätietoja Azure RemoteApp ohjelmistopakettia mallia kuvat."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Mikä on Azure RemoteApp sivustomallien kuvia?

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp-tilaukseesi sisältyy kolme sivustomallien kuvia:


- Windows Server 2012
- Microsoft Office 365 ProPlus (Office 365-tilauksen pakollinen)
- Microsoft Office 2013 Professional Plus (vain kokeiluversio)

> [AZURE.IMPORTANT]Azure RemoteApp-tilauksen antaa, voit käyttää ohjelmistoa kuvia, paitsi Office 365 ProPlus, jossa pyydetään tilattu ja Office 2013, jota ei voi käyttää tuotannon. Tämä tarkoittaa, että voit jakaa ohjelmia tai sivustomallien kuvia sovellusten käyttäjien kanssa. Jos luot kokoelma, joka käyttää Windows Server 2012 R2-kuvaa, voit julkaista System Center Endpoint Protection käyttäjät voivat käyttää RemoteApp kautta.
>
> Katso lisätietoja [RemoteApp käyttöoikeuksien myöntämistä tiedot](remoteapp-licensing.md) . Ja [Officen Azure RemoteApp](remoteapp-o365.md) -nimiseen Office-käyttöoikeudet tiedot.

Lisätietoja kutakin kuvaa on luettu.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("merkkiaine kuvan")
Tämä kuva perustuu Microsoft Windows Server 2012 R2 palvelinkeskuksen käyttöjärjestelmä ja on seuraavista rooleista ja Azure RemoteApp sivustomallien kuvia vaatimukset täyttävän asennetut ominaisuudet:


- .NET framework 4.5 3.5.1, 3.5
- Työpöydän kokemus
- Käsin ja käsin palvelut
- Media Foundation
- Työpöydän etäistunnon Host (isäntä)
- Windows PowerShellin 4.0
- Windows PowerShell ise:
- WoW64 tuki

Tämä kuva on myös on asennettu seuraavat sovellukset:

- Adobe Flash Player
- Microsoft Silverlight
- Microsoft System Center 2012 Endpoint Protection
- Microsoft Windows Media Player


## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (tilauksen pakollinen)
Office 365: n useimmin pyydettyjen sovellusten, jotta "Mukautettu" kuvan, voit käyttää luomaasi.

Tämä kuva on tiedostotunnistetta merkkiaine kuvan ja Microsoft Office 365 ProPlus-asennettu Windows Server 2012 R2-kuva on kuvattu osien lisäksi seuraavat osat:


- Access
- Excel
- Lync
- OneNote
- OneDrive for Business (Huomaa, että sync-agentti ei tueta käytettäväksi Azure RemoteApp)
- Outlookin
- PowerPointin
- Word
- Microsoft Officen tekstintarkistustyökalut

Kuva myös Visio Pro- ja Project Pro.

Ja seuraavat sovellukset, myös:

- SQL Native client
- ODBC-ohjain
- SQL Server tietojen louhintaa asiakas
- MasterDataServices asiakas
- Microsoft Publisherissa
- PowerQuery
- Powermapiin


Office 365 ProPlus-sovelluksen kaikki toiminnot ovat käytettävissä vain käyttäjät, joilla on Office 365 ProPlus-palvelupaketti. Lisätietoja Office 365: n tilauspaketeista, katso [Office 365: n palvelusopimusten vaihtoehdot](http://technet.microsoft.com/library/office-365-plan-options.aspx). Tarvitsetko lisätietoja? Tutustu [Office 365: ssä + RemoteApp](remoteapp-o365.md) -tiedot. Tutustu myös uusi artikkelista [Azure RemoteApp Office 365-tilauksen käyttäminen](remoteapp-officesubscription.md).

Huomaa, että haluat käyttöoikeuden Office 365 ProPlus, Visio Pro ja Project Pro erikseen - jokainen on oman käyttöoikeuden.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (vain kokeiluversio)
Maksuttoman kokeilujakson aikana voit testata palvelua ja Office 2013-kuva.

Tämä kuva on tiedostotunnistetta merkkiaine kuvan ja Microsoft Office 2013 Professional Plus asennettu Windows Server 2012 R2-kuva on kuvattu osien lisäksi seuraavat osat:


- Access
- Excel
- Lync
- OneNote
- OneDrive for Business (Huomaa, että sync-agentti ei tueta käytettäväksi Azure RemoteApp)
- Outlookin
- PowerPointin
- Projektin
- Visio
- Word
- Microsoft Officen tekstintarkistustyökalut

> [AZURE.IMPORTANT]**Juridiset tiedot:** Tämä kuva ei ole Microsoft Office-käyttöoikeus eikä *sitä voi käyttää tuotannon*. Kuva on tarkoitettu käytettäväksi vain Office 2013 Professional Plus. Jos haluat käyttää Office-sovelluksia Azure RemoteApp tuotannon, tarvitset Office 365 ProPlus kuvaa. Lisätietoja käyttöoikeuksien myöntämistä Office on artikkelissa [Azure RemoteApp käyttämällä Office 365](remoteapp-o365.md)
