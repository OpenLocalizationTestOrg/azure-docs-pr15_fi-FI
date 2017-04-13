<properties
   pageTitle="Määrittäminen nimeltä todennuksen tunnistetiedot | Microsoft Azure"
   description="Tietoja siitä, miten voit tunnistetietoja, että Visual Studio voit tarkistamiseen pyynnöt Azure Azure sovelluksen Visual Studio valinnaiseksi tai valvoa aiemmin pilvipalvelussa... "
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="setting-up-named-authentication-credentials"></a>Nimetty todennuksen tunnistetiedot on määritetty

Voit julkaista sovelluksen Azure Visual Studio tai valvoa aiemmin pilvipalvelussa, sinun on määritettävä tunnistetiedot, jotka Visual Studio avulla voit todentaa Azure pyynnöt. On monessa Visual Studiossa, johon voit kirjautua sisään näitä tunnistetietoja. Esimerkiksi Server Explorerista, voit avata **Azure** -solmu pikavalikon ja valitse **Muodosta yhteys Azure**. Kun olet kirjautunut sisään, Tilaustiedot Azure tilisi on käytettävissä Visual Studiossa ja ei ole enää mitään muuta, sinun on suoritettava.

Azure Työkalut tukee myös vanhempia keino käyttöoikeudet, tilauksen käyttäminen (.publishsettings-tiedosto). Tässä ohjeaiheessa kerrotaan tätä menetelmää, jota tuetaan edelleen Azure SDK 2.2.

Azure-todennus vaaditaan seuraavat kohteet.

- Tilauksen tunnus

- Kelvollinen v3 X.509-varmenne

>[AZURE.NOTE] X.509 v3 varmenteen avaimen pituuden on oltava vähintään 2 048 bittiä. Azure Hylkää kaikki varmenne, joka ei täytä tämä vaatimus tai, joka ei ole kelvollinen.

Visual Studio käyttää tilaus-Tunnuksesi ja varmenteen tietojen tunnistetiedot. Tarvittavat tunnistetiedot viitataan tilauksen tiedostossa (.publishsettings-tiedosto), joka sisältää varmenteen julkinen avain. Tilauksen tiedostossa voi olla useita tilauksia tunnistetiedot.

Voit muokata **Uusi/Muokkaa tilauksen** -valintaikkunassa tilauksen tiedot kuvatulla tavalla tämän artikkelin.

Jos haluat luoda sertifikaatin, voit viitata [Luo ja lataa Azure hallinta varmenteen](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) ohjeita ja manuaalisesti Lataa sertifikaatti [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.NOTE] Nämä tunnistetiedot, jotka Visual Studio edellyttää hallittavan cloud Services-palvelut eivät ole samoja tunnistetietoja, joita tarvitaan todentaa pyynnön vastaan Azure liittyviä palveluja.

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Muokkaa tai Visual Studiossa todentamisen tunnistetiedoilla vieminen

Voit myös määrittäminen, muokata tai viedä todennus tunnistetiedot **Uusi tilaus** -valintaikkunassa, joka tulee näkyviin, jos teet jompikumpi seuraavista toimista:

- **Palvelimen Explorer**Avaa **Azure** -solmu pikavalikon, valitse **Tilausten hallinta**, valitse **Varmenteet** -välilehti ja valitse **Uusi** tai **Muokkaa** -painiketta.

- Kun julkaiset **Julkaista Azure-sovelluksen** ohjatusta Azure pilvipalvelussa, valitse **Hallitse** **Valitse tilaus** -luettelosta ja valitse varmenteet-välilehti ja valitse sitten **Uusi** tai **Muokkaa** -painiketta.

Seuraavassa oletetaan, että **Uusi tilaus** -valintaikkuna on avoinna.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Voit määrittää Visual Studiossa todennuksen tunnistetiedot

1. **Valitse varmenne** todennusta luettelon Valitse varmenne.

1. Voit **kopioida koko polku** -painiketta. Leikepöydälle kopioidun polku varmenteen (.cer-tiedosto).

    >[AZURE.IMPORTANT] Voit julkaista Azure sovelluksen Visual Studio täytyy ladata tämän todistuksen [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Voit ladata sertifikaatin [Azure perinteinen portaalissa](http://go.microsoft.com/fwlink/?LinkID=213885)seuraavasti:

    1. Valitse Azure Portal-linkki.

         [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885) avautuu.

    1. Kirjautuminen [Azure perinteinen portaaliin](http://go.microsoft.com/fwlink/?LinkID=213885)ja valitse sitten **Cloud Services** -painiketta.

    1. Valitse pilvipalvelussa kiinnostavaa ryhmää.

        Palvelu-sivu avautuu.

    1. Valitse **Varmenteet** -välilehdessä **Lataa** -painiketta.

    1. Liitä juuri luomasi .cer-tiedoston koko polku ja kirjoita salasana, jonka olet määrittänyt.
