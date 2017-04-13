<properties 
   pageTitle="Määrittäminen ja käyttäminen tallennustilan emulaattori Visual Studiossa | Microsoft Azure"
   description="Määrittäminen ja käyttäminen tallennustilan emulaattori Visual Studiossa"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Määrittäminen ja käyttäminen tallennustilan emulaattori Visual Studiossa

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Yleiskatsaus
Azure SDK-kehitysympäristö sisältää tallennustilan emulaattori-apuohjelma, jonka varmistaminen tallennustilan Blob, jonossa ja taulukon palvelut käytettävissä Azure paikallisen development käyttämääsi laitteeseen. Jos olet luominen pilvipalveluun, joka käyttää Azure liittyviä palveluja tai kirjoittaminen ulkoinen sovellus, joka kutsuu liittyviä palveluja, voit testata koodisi paikallisesti tallennustilan emulaattori vastaan. Microsoft Visual Studio Azure-Työkalut integroida tallennustilan emulaattori hallinnointi Visual Studio. Azure-työkaluja alusta tallennustilan emulaattorin tietokanta ensimmäisellä käyttökerralla, tallennustilan emulaattorin-palvelu käynnistyy, kun suoritat tai virheenkorjaus koodisi Visual Studio ja vain luku-käyttö tallennustilan emulaattorin tietoja Azure-tallennustilan Explorer kautta.

Lisätietoja tallennustilan emulointiohjelma, mukaan lukien järjestelmävaatimukset ja mukautetun määritysohjeet artikkelissa [Kehitys ja Testing Azure tallennustilan emulointikone](./storage/storage-use-emulator.md).

>[AZURE.NOTE] On joitakin eroja toimintojen tallennustilan emulaattorin simuloinnissa ja tallennustilaa Azure-palveluiden välillä. Katso tietoja tietyn eroista Azure SDK ohjeissa [erot välillä tallennustilan emulointikoneen ja Azure liittyviä palveluja](./storage/storage-use-emulator.md) .

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Tallennustilan emulaattori yhteysmerkkijonon määrittäminen

Käyttämään tallennustilan emulaattori roolin koodi haluat määrittää yhteysmerkkijonon, joka osoittaa tallennustilan emulaattori ja joka voidaan muuttaa myöhemmin siten, että Azure-tallennustilan tilin. Yhteysmerkkijonon on Kokoonpanoasetus, tehtäväsi voivat lukea suorituksen tallennustilan tiliin yhdistämistä varten. Lisätietoja yhteyden merkkijonot luomisesta on artikkelissa [Azure-sovelluksen määrittäminen](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

>[AZURE.NOTE] Voit palauttaa viittauksen tallennustilan emulaattorin tilin koodista **DevelopmentStorageAccount** -ominaisuuden avulla. Tämä menetelmä toimii oikein, jos haluat käyttää tallennustilan emulaattori koodista, mutta jos haluat julkaista sovelluksesi Azure, sinun on luotava yhteysmerkkijonoa Azure tallennustilan tiliäsi ja muokata koodin käyttämään kyseistä yhteysmerkkijonon, ennen kuin julkaiset. Jos olet siirtymässä tallennustilan emulaattorin-tilin ja Azure-tallennustilan tilin usein, yhteysmerkkijonon yksinkertaistamaan prosessia.

## <a name="initializing-and-running-the-storage-emulator"></a>Valmistellaan ja tallennustilaa emulaattori käynnissä

Voit määrittää, että kun suoritat tai virheenkorjaus palvelun Visual Studiossa, Visual Studio käynnistyy automaattisesti tallennustilan emulaattori. Napsauta ratkaisunhallinnassa Avaa pikavalikko **Azure** projektin ja valitse **Ominaisuudet**. **Kehitys** -välilehdessä **Aloita Azure tallennustilan emulaattorin** , valitse luettelosta **Tosi** (jos se ei ole vielä määritetty arvo).

Suorita tai virheenkorjaus Visual Studio palvelun ensimmäisen kerran tallennustilan emulaattori käynnistää alustusprosessin. Tämä prosessi varaa paikalliset portit tallennustilan emulaattori varten ja luo tallennustilan emulaattorin tietokannan. Kun valmiina, tämä toimenpide ei tarvitse suorittaa uudelleen, ellet tallennustilan emulaattorin tietokannan poistetaan.

>[AZURE.NOTE] Azure-Työkalut kesäkuussa 2012-versiossa alkaen tallennustilan emulaattori suoritetaan, oletusarvoisesti SQL Express LocalDB. Aikaisempien versioiden Azure-Työkalut-tallennustilan emulaattori suoritetaan oletusesiintymää SQL Express 2005 tai 2008, jossa on asennettava, ennen kuin voit asentaa Azure SDK-paketissa. Voit suorittaa tallennustilan emulaattori nimettyyn esiintymään SQL Express tai nimetyn tai Microsoft SQL Serverin oletusesiintymää vastaan. Jos haluat määrittää tallennustilan emulaattori suorittaa erillisen kuin oletusesiintymää vastaan, katso [Käytä Azure-tallennustilan emulaattorin kehittämisen ja Testing](./storage/storage-use-emulator.md).

Tallennustilan emulaattori sisältää käyttöliittymän paikallinen tallennus-palvelujen tilan tarkasteleminen ja voit aloittaa, lopettaa ja palauttaa ne. Kun emulaattorin tallennuspalvelu on alkanut, voit käyttöliittymän tai käynnistää tai pysäyttää palvelun napsauttamalla ilmaisinalueen kuvaketta Windowsin tehtäväpalkissa Microsoft Azure-emulaattorin varten.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Palvelimen Explorerissa tallennustilan emulaattorin tietojen tarkasteleminen

Azuren tallennustilaan solmu palvelimen Explorerin avulla voit tarkastella tietoja ja tallennustilaa käyttäjätilejä, kuten tallennustilan emulaattori tietojen Blob-objektien ja taulukon asetusten muuttaminen. Lisätietoja on kohdassa [selaaminen ja hallinta tallennustilan resurssien palvelimen Resurssienhallinnassa](https://msdn.microsoft.com/library/azure/ff683677.aspx) .
