<properties
   pageTitle="Azure projektin luominen Visual Studiossa | Microsoft Azure"
   description="Visual Studiossa Azure projektin luominen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="creating-an-azure-project-with-visual-studio"></a>Visual Studiossa Azure projektin luominen

Visual Studio Azure-työkalut tarjoavat malli, jonka avulla voit luoda Azure pilvipalveluun. Työkalujen avulla voit määrittää, virheenkorjaus ja ottaa käyttöön Azure pilvipalvelussa.

Azure cloud-palvelun ratkaisu sisältää seuraavanlaisia projekteja:

- **Azure projektin**

    Azure projektissa on roolin projekteihin yhteydet ratkaisussa. Se myös palvelun määritys ja palvelun asetustiedostot. Palvelun määritystiedoston määrittää runtime asetusten mukaan lukien roolit ovat pakollisia, päätepisteet ja virtuaalikoneen kokoa. Palvelun määritystiedoston määrittää, kuinka monta esiintymää roolin määrittäminen ja roolille asetusten arvot. Lisätietoja näistä asetuksista on artikkelissa [Toimintaohje: roolien määrittäminen Azure-pilvipalvelussa Visual Studiossa](vs-azure-tools-configure-roles-for-cloud-service.md).

- **Web-roolin projekti**

    Työntekijän rooli suorittaa taustalla. Työntekijän rooli viestiä liittyviä palveluja ja muut Internet-pohjaisten palvelujen kanssa. Työntekijän rooli voi olla jokin muu luku HTTP, HTTPS tai TCP päätepisteistä.

    - **ASP.NET Web-rooli**-etsimisen web-edusta ASP.NET-sovellukseen
    - **ASP.NET-MVC5 Web-rooli**
    - **ASP.NET-MVC4 Web-rooli**
    - **ASP.NET-MVC3 Web-rooli**
    - **WCF-palvelun Web-rooli**-etsimisen WCF-palvelu
    - **Silverlight sovelluksen Web liiketoimintasäännön** (edellyttää Visual Studio 2012)

- **Välimuistin työntekijän rooli**

    Rooli, joka on erillinen välimuisti-sovellukseen.

- **Työntekijän roolin palvelun Bus jonon kanssa**

    Palvelun bus jono, joka sisältää viestin queuing toiminnot viestintä työntekijän prosessin avulla. Katso lisätietoja, [miten voit käyttää palvelun Bus olevien](http://go.microsoft.com/fwlink/?LinkId=260560).

## <a name="to-create-an-azure-cloud-service-project-in-visual-studio"></a>Visual Studio Azure cloud-palvelun projektin luominen

1. Käynnistä Microsoft Visual Studio järjestelmänvalvojana.

1. Valitse valikkorivillä **Tiedosto**, **Uusi** **Projekti**.

1. **Projektityypit** , valitse ruudun **Cloud** Visual C#- tai Visual Basic-projektin mallia solmujen.

1. Valitse **Mallit** -ruudussa **Azure pilvipalvelussa**.

1. Määritä haluamasi avulla voit tehdä projektin .NET Framework-version.

1. Kirjoita nimi ja sijainti projektin ja ratkaisu nimi. Valitse **OK** -painiketta.

1. Valitse **Uusi Azure-projekti** -valintaikkunasta rooleihin, jotka haluat lisätä ja valita oikean nuolipainikkeen lisääminen ratkaisu. Voit lisätä niin monta roolit kuin tarvitset.

1. Uudelleennimeäminen rooli, jonka olet lisännyt projektiin, osoita roolin **Azure uusi projekti** -valintaikkunassa, ja oikealla puolella rooli valitsemalla **Nimeä uudelleen** -kuvake. Voit nimetä rooli ratkaisu sisällä, kun se on lisätty.
