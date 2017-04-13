<properties
    pageTitle="Kuluttavat tietokoneen Learning verkkopalvelun web app-malli | Microsoft Azure"
    description="Kuluttavat Azure koneen Learning ennakoivan verkkopalvelun Azure Marketplacesta web app-mallin avulla."
    keywords="Web-palveluun, operationalization, REST API-koneen oppiminen"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="garye;raymondl"/>

# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Kuluttavat Azure koneen Learning WWW-palvelun web app-malli

>[AZURE.NOTE] Tässä ohjeaiheessa kerrotaan koskevat perinteinen WWW-palvelun avulla. 

Kun olet kehittänyt ennakoivan mallin ja käyttöön tietokoneen Learning Studiossa Azure web-palvelu tai työkaluilla, kuten R tai Python, voit käyttää operationalized mallin REST-Ohjelmointirajapinnan käyttäminen.

On useita tapoja tarjoaman REST-Ohjelmointirajapinnalla ja käyttää verkkopalvelua. Voit esimerkiksi kirjoittaa sovelluksen C#, R tai Python otoksen avulla luodaan, kun (käytettävissä koneen Learning Studiossa web service-koontinäytön API Ohjesivun) WWW-palvelun käyttöön. Tai voit käyttää Microsoft Excelin esimerkkitiedoston luonut puolestasi (myös käytettävissä WWW-palvelun koontinäyttö Studiossa).

Mutta nopein ja helpoin tapa käyttää web-palvelu on käytettävissä [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/)Web App-mallien avulla.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Azure Konepohjaisten Oppimistekniikoiden Web App-malleissa

Web app käytettävissä olevat mallit Azure Marketplacesta voit luoda mukautetun verkkosovelluksen tietokenttiä tietää web-palvelu syöttötiedot ja odotetut tulokset. Kaikki sinun on suoritettava on web app käyttöoikeuden antaminen web-palvelu ja tiedot ja malli hoitaa loput.

Kaksi mallit ovat käytettävissä:

- [Azure ML pyynnön vastaus palvelun Web App-malli](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Azure ML erä suorittamisen palvelun Web App-malli](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Jokainen malli tunnistus web-sivuna Azure ja luo otoksen ASP.NET-sovelluksen, WWW-palvelun API URI ja avaimen avulla. Vastaus palvelun (RRS)-malli luo verkkosovellukseen, jonka avulla voit lähettää yhden tietorivin WWW-palvelun avulla saat yhden tuloksen. Erän suorittamisen Service (BES)-malli luo verkkosovellukseen, jonka avulla voit lähettää monta riviä tietoja tuottaa useita tuloksia.

Mallien käyttäminen edellyttää ei coding. Sinun on annettava vain API URI ja avaimen avulla ja mallin muodostaa sovelluksen puolestasi.

## <a name="how-to-use-the-request-response-service-rrs-template"></a>Opi käyttämään vastaus palvelun (RRS)-malli

Kun olet ottanut käyttöön web-palvelu, voit noudattaa käyttämään RRS web app-malli, kuten seuraavassa kaaviossa on esitetty ohjeita.

![Prosessi, jos haluat käyttää RRS web-malli][image1]

1. Koneen Learning Studiossa Avaa **Web Services** -välilehti ja avaa web-palvelu, jota haluat käyttää. Kopioi avaimen kohdasta **Ohjelmointirajapinnan avain** ja tallenna se.

    ![Ohjelmointirajapinnan avain][image3]

2. Avaa **PYYNNÖN ja VASTAUKSEN** API Ohje-sivu. Ohjesivun kohdassa **pyytää**yläreunassa kopioi **Pyytää URI** -arvo ja tallenna se. Tämä arvo näyttää tältä:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![Pyynnön URI][image4]

3. Siirry [Azure portal](https://portal.azure.com), **Kirjaudu sisään**, valitse **Uusi**, Etsi ja valitse **Azure ML vastaus palvelun Web App**ja valitse sitten **Luo**. 

    - Anna koodiin yksilöivä nimi. Web-sovelluksen URL-osoite on saman niminen perään `.azurewebsites.net.` esimerkiksi`http://carprediction.azurewebsites.net.`

    - Valitse Azure tilaus ja palvelut, joiden web-palvelu on käynnissä.

    - Valitse **Luo**.

    ![Web-sovelluksen luominen][image5]

4. Kun Azure on lopettanut web-sovelluksen ottaminen käyttöön, valitsemalla Valitse Azure web app asetukset-sivun **URL-Osoitteen** tai URL-Osoitteen kirjoittaminen selaimessa. Esimerkiksi`http://carprediction.azurewebsites.net.`

5. Web-sovelluksen ensimmäisen käynnistyksen yhteydessä se kysyy **API URL-osoite** ja **Ohjelmointirajapinnan avain**.
Kirjoita aiemmin tallentamasi:
    - **Pyydä URI** **API kirjaa** URL-osoitteen API Ohje-sivu
    - Web-palvelu koontinäytössä **Ohjelmointirajapinnan avain** **Ohjelmointirajapinnan avain** .

    Valitse **Lähetä**.

    ![Kirjoita viestin URI ja Ohjelmointirajapinnan avain][image6]

6. Web-sovelluksen näyttää sen **Web App-määritys** -sivu, jossa nykyisen WWW-Palveluasetukset. Tässä voit tehdä muutoksia web App-sovelluksessa käytettävien asetukset.

    > [AZURE.NOTE] Nämä asetukset vain muuttaminen ne web-sovelluksen. WWW-palvelun oletusasetukset ei muutu. Esimerkiksi jos haluat muuttaa **kuvaus** tähän se ei muuta näytetään web palvelun Raporttinäkymät-ikkunan koneen Learning Studiossa kuvaus.

    Kun olet valmis, valitse **Tallenna muutokset**ja valitse sitten **Siirry aloitussivulle**.

7. Voit kirjoittaa kotisivulla arvot lähettäminen web-palveluun, valitse **Lähetä**ja palautetaan tuloksen.

Jos haluat palata **määritys** -sivulla, siirry `setting.aspx` web-sovelluksen sivulla. Esimerkki: `http://carprediction.azurewebsites.net/setting.aspx.` pyydetään antamaan Ohjelmointirajapinnan avain uudelleen - sivulle ja Päivitä asetuksia, jotka tarvitset.

Voit lopettaa, Käynnistä uudelleen tai poistaa Azure-portaalissa, kuten web-sovelluksen web-sovelluksen. Kun se suoritetaan voit Siirry home verkko-osoite ja kirjoita uudet arvot.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Opi käyttämään erä suorittamisen Service (BES)-malli

Voit käyttää BES web app-mallin samalla tavalla kuin RRS malliin, sillä erotuksella, että web-sovellusta, joka on luotu Salli useita tietorivejä lähettää ja vastaanottaa useita tuloksia.

Erän suorittamisen verkkopalvelun tulokset tallennetaan Azure säilytykseen; syöttöarvojen voi tulla Azure tallennustilan tai paikallinen tiedosto.
Niin sinun on Azure säilytykseen tuloksille palauttama web-sovelluksen ja sinun on opastaminen kenttään annettavat tiedot.

![Prosessi, jos haluat käyttää BES web-malli][image2]

1. Samalla tavalla kuin RRS-mallin BES web-sovelluksen luominen lukuun ottamatta:
    - Pyydä **Pyytää URI** **ERÄKÄSITTELY** API Ohjesivun WWW-palvelun.
    - Siirry [Azure ML erä suorittamisen palvelun Web App malli](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) Avaa BES Azure Marketplace-malli ja valitse **Luo verkkosovellus**.

2. Voit määrittää kohtaa, johon haluat tallentaa tulosten kirjoittamalla web app-aloitussivulla kohde säilö tiedot. Määritä myös paikassa, jossa web-sovelluksen voit käyttää syöttöarvojen paikallisen tiedoston tai Azure säilytykseen.
Valitse **Lähetä**.

    ![Tiedot.][image7]

Web-sovelluksen näkyy sivu, jossa tilan.
Kun työ on valmis sinun annettava Azure-blob-säiliö tulokset sijainti. Voit myös halutessasi lataamalla tulokset paikallisen tiedoston.

## <a name="for-more-information"></a>Lisätietoja

Lisätietoja...

- luot koneen learning kokeen Konepohjaisten Oppimistekniikoiden Studiossa, katso [oman ensimmäisessä kokeessa Azure Konepohjaisten Oppimistekniikoiden Studiossa luominen](machine-learning-create-experiment.md)

- koneen learning ottamisesta kokeilla web-palvelu on artikkelissa [Azure Konepohjaisten Oppimistekniikoiden WWW-palvelun käyttöönotto](machine-learning-publish-a-machine-learning-web-service.md)

- muita tapoja käyttää web-palveluun, katso, [miten tarjoaman Azure koneen Learning web-palvelu](machine-learning-consume-web-services.md)


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png
