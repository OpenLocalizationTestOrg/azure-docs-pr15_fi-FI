<properties
    pageTitle="Vianmääritys Azure koneen Learning perinteinen WWW-palvelun Retraining | Microsoft Azure"
    description="Tunnistaa ja korjata yleisiä ongelmia levykiintiötietoja, kun ovat käsittää mallin Azure koneen Learning-ohjelman Web-palveluun."
    services="machine-learning"
    documentationCenter=""
    authors="VDonGlover"
   manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Azure koneen Learning perinteinen WWW-palvelun uudelleenkoulutusta vianmääritys

## <a name="retraining-overview"></a>Uudelleenkoulutuksen yleiskatsaus

Kun otat ennakoivan kokeen tulosmalli web-palvelu on staattinen malli. Uusien tietojen tullessa käytettävissä-tilaan tai Ohjelmointirajapinnan kuluttaja on omia tietoja, malli on oltava retrained. 

Katso vaiheista perinteinen verkkopalvelun uudelleenkoulutuksen prosessi, [Suuntaamista koneen Learning mallien ohjelmallisesti](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Uudelleenkoulutuksen prosessi

Kun haluat suuntaamista Web-palveluun, sinun on lisättävä joitakin muita laitteita:

* Web-palvelu koulutus kokeen käyttöön. Kokeen on oltava **Web-palvelu tulostus** -moduuli, joka on liitetty **Junassa malli** -moduulin tulos.  

    ![Liittää WWW-palvelun tulosteen junassa-malli.][image1]

* Tulosmalli Web-palvelu lisätään uusi päätepiste.  Voit lisätä ohjelmallisesti käyttämällä annettua otoksen Viitattu suuntaamista koneen Learning-malleissa ohjelmallisesti päätepisteen aiheen tai palvelun Azure perinteinen-portaalissa.

Voit sitten suuntaamista mallin koulutus verkkopalvelun API ohjesivu otoksen C# koodilla. Kun on lasketaan tulokset ja heidän kanssaan täyttyvät, voit päivittää näkyvissä WWW-palvelun avulla uusi päätepiste, jonka lisäsit pistemäärä koulutetun mallin.

Paikallaan kaikki kappaletta, jossa sinun on otettava suuntaamista mallin päävaiheet ovat seuraavat:

1.  Koulutus-verkkopalvelun kutsu: puhelu on, erän suorittamisen Service (BES), ei ole Pyydä vastaus Service (RRS). API Ohjesivun otoksen C# koodin avulla voit soittaa. 
2.  Arvojen etsiminen *BaseLocation*, *RelativeLocation*ja *SasBlobToken*: nämä arvot palauttaa tulosteesta koulutus-verkkopalvelun puhelu. 
      ![Näyttää tulosteen uudelleenkoulutuksen otoksen ja BaseLocation, RelativeLocation ja SasBlobToken arvot.][image6]
3.  Päivitä lisätty päätepisteen tulosmalli verkkopalvelusta uuden koulutetun mallin: annettu suuntaamista koneen Learning-malleissa ohjelmallisesti otoksen koodissa Päivitä koulutus verkkopalvelusta lisätään tulosmalli mallin juuri koulutetun mallia uusi päätepiste.

## <a name="common-obstacles"></a>Yleisiä esteistä

### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Tarkista, onko oikeaa KORJAUSTIEDOSTON URL-osoite

Käytät KORJAUSTIEDOSTON URL on oltava yhden liittyvän tulosmalli Web-palvelu lisätään uusi tulosmalli päätepiste. On useita tapoja KORJAUSTIEDOSTON URL-osoitteen:

**Vaihtoehto 1: ohjelmallisesti**

Voit hankkia oikeaa KORJAUSTIEDOSTON URL-Osoitteen seuraavasti:

1.  Suorita [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) otoksen koodi.
2.  Etsi *HelpLocation* AddEndpoint tulosteen, ja kopioi URL-osoite.

    ![HelpLocation addEndpoint otosten tulosteessa.][image2]

3.  Liitä URL-osoite selaimen, siirry sivulle, joka sisältää ohjelinkit Web-palveluun.
4.  Valitse Avaa korjaustiedoston ohjesivu **Päivityksen resurssin** -linkki.

**Vaihtoehto 2: Azure perinteinen-portaalin käyttäminen**

1.  Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com).
2.  Avaa tietokoneen Learning-välilehti. 
     ![Koneen kallistumassa-välilehti.][image4]
3.  Valitse työtilan nimi, valitse **Verkkopalveluihin**.
4.  Valitse tulosmalli verkkopalvelun käsitellään. (Jos muokkaat ei WWW-palvelun oletusnimi, se päättyy: [näkyvissä pistemäärä eksponentti.].)
5.  Valitse **Lisää päätepiste**.
6.  Kun päätepisteen on lisätty, valitse päätepisteen nimi. Valitse **Päivitä resurssin** Avaa Aikavyöhykkeiden Ohje-sivu.

>[AZURE.NOTE] Jos olet lisännyt päätepisteen sijaan ennakoivan verkkopalvelun koulutus Web-palveluun, saat seuraavan virheilmoituksen kun napsautat linkkiä **Päivitä resurssi** : ikävä Kyllä, mutta tämä toiminto ei ole tuettu tai käytettävissä tässä kontekstissa. Web-palvelu on resursseja ei voi päivittää. Yritämme korjata ongelman ja työskentelevät parantaminen tämän työnkulun.

![Uusi päätepisteen raporttinäkymät-ikkuna.][image3]

KORJAUSTIEDOSTON Ohje-sivu sisältää KORJAUSTIEDOSTON URL, sinun on käytettävä sekä annetaan esimerkkejä-koodin avulla voit kutsua sitä.

![Korjaustiedoston URL-osoite.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Tarkista, että olet päivittämässä oikea tulosmalli päätepiste

* Koulutus-Web-palvelu ei ole korjaustiedoston: korjaus-toiminto on suoritettava tulosmalli Web-palvelusta.
* WWW-palvelun oletusarvoinen päätepiste ei korjaustiedoston: korjaus-toiminto on suoritettava uusi tulosmalli Web palvelun päätepisteen, jonka lisäsit.

Voit tarkistaa mihin WWW-palvelun päätepiste on ohjesisältöä Azure perinteinen portaalin. 

>[AZURE.NOTE] Muista, olet lisäämässä päätepisteen ennakoivan Web-palvelu ei ole koulutus Web-palvelu. Jos olet ottanut oikein koulutusta ja ennakoivan verkkopalvelun, kaksi erillistä verkkopalvelut, luettelossa pitäisi näkyä. Ennakoivan verkkopalvelun olisi pääty "[ennakoivan eksponentti.]".

1.  Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com).
2.  Avaa tietokoneen Learning-välilehti. 
     ![Koneen learning työtila UI.][image4]
3.  Valitse työtilassa.
4.  Valitse **Web-palvelut**.
5.  Valitse ennakoivan Web-palvelu.
6.  Varmista, että Web-palvelu on lisätty uusi päätepiste.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Tarkista työtilan, jotta se on oikein alueen on web-palvelu

1.  Kirjautuminen [Azure perinteinen portal](https://manage.windowsazure.com).
2.  Valitse valikosta tietokoneen Learning.
      ![Koneen learning alue UI.][image4]
3.  Tarkista työtilan sijainti.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png