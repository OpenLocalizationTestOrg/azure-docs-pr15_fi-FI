<properties
    pageTitle="Azure pinon palvelun sovelluksen teknisen ennakkoversion 1, ennen kuin aloitat | Microsoft Azure"
    description="Viimeistele ennen kuin otat Web Apps-sovellusten Azure pinon vaiheet"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>Ennen kuin alat Azure pinon Web Apps-sovellusten kanssa

> [AZURE.NOTE] Seuraavat tiedot koskevat vain Azure pinon TP1 ominaisuuksissa.

Sinun on muutamia kohteita Azure pinon Web Apps-sovellusten asentaminen:

- Otettiin [Azure pinon Technical Preview 1](azure-stack-run-powershell-script.md)
- Azure-pino järjestelmän pieni käyttöönottoa Azure pinon Web Apps-sovelluksista tarpeeksi tilaa.  Pakollinen tila on noin 20 gt: n levytilaa.
- [Palvelin, joka toimii SQL Server](#SQL-Server).

>[AZURE.NOTE] KAIKKI asetetaan-asiakasohjelman AM seuraavasti.

## <a name="before-you-deploy-azure-stack-web-apps"></a>Ennen kuin otat Azure pinon Web Apps-sovelluksista

Käyttöönotto resurssin palveluntarjoaja, sinun on suoritettava PowerShell integroitu komentosarjan Environment(ISE) järjestelmänvalvojana. Tästä syystä haluat sallia evästeet ja JavaScript Internet Explorer-profiili, jonka avulla Azure Active Directory kirjautuminen.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Internet Explorerin parannetun suojauksen poistaminen käytöstä

1.  Kirjaudu Azure pinon käsitteiden, (Käsitteiden) tietokoneeseen ****AzureStack/järjestelmänvalvojana ja avaa sitten **Serverin hallinta**.
2.  **Internet Explorerin parannetun suojauksen** käytöstä järjestelmänvalvojia ja käyttäjiä.
3.  Kirjaudu sisään järjestelmänvalvojana ClientVM.AzureStack.local virtuaalikoneen ja avaa sitten **Palvelimen hallinta**.
4.  **Internet Explorerin parannetun suojauksen** käytöstä järjestelmänvalvojia ja käyttäjiä.

## <a name="enable-cookies"></a>Evästeiden ottaminen käyttöön

1.  Valitse **Käynnistä**> **kaikki sovellukset**> **Windows apuohjelmat**. Napsauta **Internet Explorerin** > **Lisää** > **järjestelmänvalvojana**.
2.  Jos näyttöön tulee sanoma, valitse **suositeltavaa käyttää suojaus**ja valitse sitten **OK**.
3.  Valitse Internet Explorerissa **Työkalut** (hammaspyöräkuvake), > **Internet-asetukset** > **Tietosuoja** > **Lisäasetukset**.
4.  Valitse **Lisäasetukset**. Varmista, että **Hyväksy** molemmat valintaruudut ovat valittuina, ja valitse sitten **OK** ja **OK** uudelleen.
5.  Sulje Internet Explorer ja Käynnistä PowerShell ise: järjestelmänvalvojana.

## <a name="install-the-latest-version-of-azure-powershell"></a>PowerShellin Azure uusimman version asentaminen

1.  Kirjaudu Azure pinon Käsitteiden tietokoneen ****AzureStack/järjestelmänvalvojana.
2.  Kirjaudu sisään järjestelmänvalvojana ClientVM.AzureStack.local virtuaalikoneen Etätyöpöytäyhteys avulla.
3.  Avaa **Ohjauspaneeli** ja valitse **Poista ohjelman asennus**. 
4.  Napsauta **Microsoft Azure PowerShell - marraskuun 2015** hiiren kakkospainikkeella ja valitse **Poista**.
5.  Asennuksen jälkeen päättymistä, Käynnistä uudelleen ClientVM.AzureStack.local virtuaalikoneen
6.  Lataa ja asenna uusimmat [PowerShellin Azure](http://aka.ms/azstackpsh).


## <a name="sql-server"></a>SQL Server

Oletusarvon mukaan Azure pinon Web Apps-asennusohjelma on määritetty käyttämään, johon on asennettu sekä Azure pinon SQL resurssi-palvelu SQL Server. Kun olet asentanut resurssin SQL Server-palvelun (SQL-RP) Varmista huomioi tietokannan järjestelmänvalvojan käyttäjänimi ja salasana. Tarvitset molemmat Azure pinon Web Apps-asennuksen yhteydessä.
Huomautus: Voit myös on käyttöönotto ja suorittamalla SQL Serverin toisen palvelimen avulla. Voit käyttää SQL Server-esiintymän, kun suoritat Azure pinon Web Apps-asennuksen asetukset.
