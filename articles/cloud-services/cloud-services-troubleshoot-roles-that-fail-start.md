<properties
   pageTitle="Vianmääritys rooleihin, jotka eivät käynnisty | Microsoft Azure"
   description="Seuraavassa on muutamia yleisiä syitä, miksi pilvipalvelussa rooli saattaa epäonnistua, Aloita. Näiden ongelmien selvittämiseen, ratkaisujen myös toimitetaan."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Pilvipalvelussa roolit, joilla on käynnisty vianmääritys

Seuraavassa on joitakin yleisiä ongelmia ja ratkaisujen liittyvät Azure pilvipalveluihin rooleihin, jotka eivät käynnisty.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Puuttuvat dll tai riippuvuudet

Puuttuvat dll tai laitekokonaisuuksien voi johtua ei vastaa roolit ja roolit, jotka ovat toiminnan **alustaminen onnistuu**, **varattu**ja **lopettaminen** hyötyä välillä.

Oireita puuttuu dll tai laitekokonaisuuksien voi olla:

- Rooli esiintymää on toiminnan **alustaminen onnistuu**, **varattu**ja **lopettaminen** hyötyä kautta.
- Roolin esiintymän siirtyvät **valmiina** , mutta jos siirryt web-sovelluksen, sivulle ei tule näkyviin.

On useita suositeltavat tavat tutkiminen ongelmat.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Web-roolin puuttuvat DLL-ongelmien vianmääritys

Kun siirryt sivustoon, jossa on otettu käyttöön sivuston rooli ja selain näyttää seuraavankaltaiselta palvelinvirhe, se voi ilmaista DLL puuttuu.

![Palvelinvirhe "/" sovelluksen.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Ongelmien vianmääritys poistamalla käytöstä mukautetut virheet

Lisätietoja virhetietojen voit tarkastella web.config voit määrittää mukautetun virhetilan käytöstä web-roolin määrittäminen ja mallirakenteeseen palvelu.

Voit tarkastella lisätietoja virheitä käyttämättä etätyöpöydän seuraavasti:

1. Avaa Microsoft Visual Studio ratkaisu.

2. **Ratkaisunhallinnassa**seuraavan koodin korostetut Etsi ja avaa se.

3. Etsi system.web-osassa seuraavan koodin korostetut, ja lisää seuraava rivi:

    ```xml
    <customErrors mode="Off" />
    ```

4. Tallenna tiedosto.

5. Pakata ja ota palvelu uudelleen.

Palvelu on otettava käyttöön uudelleen, kun näet virhesanoman puuttuu kokoonpanon tai DLL nimi.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Tarkastele virheen etäyhteyden ongelmien vianmääritys

Voit käyttää rooli ja tarkastella lisätietoja virhetietojen etäyhteyden etätyöpöydän kautta. Seuraavien vaiheiden avulla voit tarkastella virheitä Etätyöpöydän avulla:

1. Varmista, että Azure SDK 1.3 tai uudempi versio on asennettu.

2. Ratkaisun Visual Studiossa käyttöönoton aikana valitsemalla "Määrittäminen työpöydän etäyhteyksien...". Lisätietoja Etätyöpöytä-yhteyden määrittämisestä on artikkelissa [Käyttämällä etätyöpöydän Azure roolien](../vs-azure-tools-remote-desktop-roles.md).

3. Valitse Microsoft Azure perinteinen-portaaliin, kun esiintymä näkyy tila on **Valmis**,-osasta jokin rooli esiintymät.

4. Valitse valintanauhan **Etäkäyttöpalvelimen** alueen **Yhdistä** -kuvake.

5. Kirjaudu virtuaalikoneen käyttöoikeuksilla, joka on määritetty etätyöpöydän määritysten aikana.

6. Avaa komentorivi-ikkuna.

7. Kirjoita `IPconfig`.

8. Huomautus: n IPV4-osoite-arvoa.

9. Avaa Internet Explorer.

10. Kirjoita osoite- ja web-sovelluksen nimi. Esimerkiksi `http://<IPV4 Address>/default.aspx`.

Siirtyminen sivustossa palauttavat tarkempia virheilmoituksia:

* Palvelinvirhe "/" sovelluksen.

* Kuvaus: Käsittelemättömän poikkeuksen vuoksi, joka on tapahtunut nykyisen web-pyynnön suorituksen aikana. Lue lisätietoja virheestä ja missä se luotu koodi pinon jäljitys.

* Poikkeuksen tiedot: System.IO.FIleNotFoundException: ei voitu ladata tiedoston tai kokoonpanon ' Microsoft.WindowsAzure.StorageClient, versio = 1.1.0.0, Culture = nolla, PublicKeyToken = 31bf856ad364e35' tai johonkin sen riippuvuudet. Määritettyä tiedostoa ei löydy.

Esimerkki:

!["/" Sovelluksen eksplisiittinen palvelinvirhe](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Käyttämällä Laske-emulaattorin ongelmien vianmääritys

Voit käyttää Microsoft Azure Laske emulaattorin ja puuttuvien riippuvuudet ja web.config virheiden vianmääritys.

Saat parhaat tulokset tällä menetelmällä vianmäärityksen sinun on käytettävä tietokoneessa tai virtual tietokoneeseen, jossa on Puhdista Windows-asennuksen. Voit simuloida parhaiten Azure ympäristössä, käytä Windows Server 2008 R2 x64.

1. Asenna [Azure SDK](https://azure.microsoft.com/downloads/)erillisversion.

2. Muodosta kehitystä, tietokoneen cloud palvelun projektin.

3. Siirry Resurssienhallinnassa cloud palvelun projektin bin\debug-kansio.

4. Kopioi .csx-kansio ja .cscfg tiedosto tietokoneeseen, jota käytät korjaa ongelmat.

5. SIIVOA tietokoneessa, Avaa Azure SDK komentorivi-ikkuna ja kirjoita `csrun.exe /devstore:start`.

6. Kirjoita komentoriville seuraava komento `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.

7. Rooli käynnistyessä, näet yksityiskohtaiset virhetiedot Internet Explorerissa. Voit käyttää myös Windowsin vianmääritystyökalut edelleen vianmäärityksessä.

## <a name="diagnose-issues-by-using-intellitrace"></a>Ongelmien vianmääritys IntelliTrace avulla

Työntekijän ja web-roolit, jotka käyttävät .NET Framework 4 voit käyttää [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), joka on käytettävissä [Microsoft Visual Studio Ultimate](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs).

Käyttöönotto palvelu käytössä IntelliTrace seuraavasti:

1. Varmista, että Azure SDK 1.3 tai uudempi versio on asennettu.

2. Ratkaisun käyttöönotto Visual Studiossa. Käyttöönoton aikana **Käyttöön IntelliTrace .NET 4 roolien** valintaruutu.

3. Kun esiintymä käynnistyy, Avaa **Palvelimen Explorer**.

4. Laajenna **Azure\\pilvipalveluihin** solmu ja Etsi käyttöönotto.

5. Laajenna käyttöönoton, kunnes näet roolin esiintymät. Napsauta hiiren kakkospainikkeella jonkin esiintymät.

6. Valitse **Näytä IntelliTrace lokitiedot**. Avaa **IntelliTrace yhteenveto** .

7. Etsi yhteenvedon poikkeukset-osa. Jos poikkeukset-kohdassa lukee **Poikkeuksen tiedot**.

8. Laajenna **Poikkeuksen tiedot** ja etsiä **System.IO.FileNotFoundException** virheitä seuraavankaltaiselta:

![Poikkeuksen tiedot, puuttuvan tiedoston tai kokoonpanon](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Puuttuvat dll-kirjastoista ja kokoonpanot

Puuttuvat DLL ja kokoonpanon virheiden korjaamiseksi seuraavasti:

1. Avaa ratkaisun Visual Studiossa.

2. **Ratkaisunhallinnassa** **viittaukset** -kansion avaaminen

3. Valitse-ohjelman virhe kokoonpanon.

4. Valitse **Ominaisuudet** -ruudussa Etsi **Paikallinen kopio-ominaisuus** ja aseta arvoksi **True**.

5. Ota cloud-palvelu uudelleen.

Kun olet varmistanut, että kaikki virheet on korjattu, voit ottaa palvelun tarkistamatta **Käyttöön IntelliTrace .NET 4 roolien** -valintaruutu.

## <a name="next-steps"></a>Seuraavat vaiheet

Näytä lisää [artikkeleita vianmääritys](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pilvipalveluihin.

Cloud palvelun roolin ongelmien vianmääritys Azure PaaS diagnostiikka tietojen avulla, kohdassa [Kevin Williamson blogin sarja](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
