<properties
   pageTitle="Emulaattorin Expressin avulla ja korjaa pilvipalvelussa paikallisessa tietokoneessa | Microsoft Azure"
   description="Emulaattorin Expressin avulla ja korjaa pilvipalvelussa paikallisessa tietokoneessa"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="using-emulator-express-to-run-and-debug-a-cloud-service-on-a-local-machine"></a>Emulaattorin Expressin avulla ja korjaa pilvipalvelussa paikallisessa tietokoneessa

Käyttämällä emulaattorin Express Testaa ja virheenkorjaus pilvipalveluun suorittamatta Visual Studio järjestelmänvalvojana. Voit määrittää projektin asetukset käyttämään emulaattorin Express tai koko emulaattorin pilvipalvelussa sijaitsevaan vaatimusten mukaan. Saat lisätietoja koko emulaattorin [Azure-sovelluksen Laske emulaattori suorittaminen](./storage/storage-use-emulator.md). Azure SDK 2.1 ensin sisältyvät emulaattorin Express ja Azure SDK 2.3 saavuttaman on oletusarvoinen emulaattori.

## <a name="using-emulator-express-in-the-visual-studio-ide"></a>Visual Studio IDE emulaattorin Expressin käyttäminen

Kun luot uuden projektin Azure SDK 2.3 tai uudempi versio, emulaattorin Express on jo valittu. Aiemmin projekteissa, jotka on luotu aiemmassa versiossa SDK Valitse emulaattorin Express seuraavasti.

### <a name="to-configure-a-project-to-use-emulator-express"></a>Voit määrittää projektin emulaattorin Expressin

1. Pikavalikon Azure projektin valitsemalla **Ominaisuudet**ja valitse **Web** -välilehti.

1. Valitse **Paikallinen kehittäminen palvelin**- **Käytä IIS Expressin asetukset** -painike. Emulaattorin Express ei ole yhteensopiva IIS-Web-palvelimen kanssa.

1. Valitse **emulaattorin**, **Käytä emulaattorin Express** -valintanappi.

    ![Emulaattorin Express](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## <a name="launching-emulator-express-at-a-command-prompt"></a>Avaamista komentokehotteeseen emulaattorin Express

Komentokehotteeseen voit käynnistää Azure Laske emulaattori, csrun.exe, express version /useemulatorexpress-asetuksen avulla.

## <a name="limitations"></a>Rajoitukset

Ennen kuin käytät emulaattorin Express, sinun olisi otettava huomioon joitakin rajoituksia:

- Emulaattorin Express ei ole yhteensopiva IIS-Web-palvelimen kanssa.

- Cloud-palvelussa voi olla useita rooleja, mutta kukin rooli on rajoitettu yhdessä paikassa.

- Et voi käyttää porttinumerot alle 1 000. Esimerkiksi jos käytät todennuspalvelu, joka käyttää tavallisesti portti alle 1 000, haluat ehkä muuttaa arvoksi porttinumero, joka on yli 1 000.

- Rajoitukset, jotka koskevat Azure Laske emulaattorin koskevat myös emulaattorin Express. Esimerkiksi ei voi olla enintään 50 roolin esiintymää käyttöönotto. Katso [Azure sovelluksen suorittaminen emulaattori suorittaminen](http://go.microsoft.com/fwlink/p/?LinkId=623050)

## <a name="next-steps"></a>Seuraavat vaiheet

[Virheenkorjaus pilvipalveluihin](https://msdn.microsoft.com/library/azure/ee405479.aspx)
