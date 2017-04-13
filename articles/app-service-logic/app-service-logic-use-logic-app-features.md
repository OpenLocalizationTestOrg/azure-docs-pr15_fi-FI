<properties 
    pageTitle="Logiikan App toiminnoilla | Microsoft Azure" 
    description="Opettele käyttämään logiikan sovellusten lisäominaisuuksia." 
    authors="stepsic-microsoft-com" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/28/2016"
    ms.author="stepsic"/> 
    
# <a name="use-logic-apps-features"></a>Logiikan ominaisuuksien käyttäminen

[Edelliseen aiheeseen](app-service-logic-create-a-logic-app.md)olet luonut ensimmäisen logiikan sovelluksen. Nyt on kerrotaan, kuinka voit luoda lisätietoja prosessin avulla Services logiikan sovellukset. Tässä ohjeaiheessa esitellään uusi logiikan sovellukset-käsitteestä:

- Ehdollinen logiikka, joka suorittaa toiminnon vain tietyn ehdon täyttyessä.
- Voit muokata aiemmin logiikan sovelluksen koodinäkymän.
- Työnkulun aloittaminen asetukset.

Ennen kuin suoritat tämän ohjeaiheen, [Luo uuden logiikan sovelluksen](app-service-logic-create-a-logic-app.md)vaiheet on suoritettava. [Azure portal]logiikan sovelluksen selaamalla ja valitse yhteenvedon logiikan sovelluksen kuvauksen muokkaaminen **Käynnistimet ja toiminnot** .

## <a name="reference-material"></a>Viittaus materiaali

Saattaa olla hyödyllistä seuraavat asiakirjat:

- [Hallinta ja runtime REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) - esimerkiksi käynnistää logiikan sovellusten suoraan ohjeet
- [Viittaus language](https://msdn.microsoft.com/library/azure/mt643789.aspx) - kaikki tuetut Funktiot ja lausekkeiden ohjeaiheiden luettelo
- [Käynnistin ja toiminto](https://msdn.microsoft.com/library/azure/mt643939.aspx) - erityyppiset toiminnot ja ne käyttää syötteiden
- [Sovelluksen palvelun yleiskatsaus](../app-service/app-service-value-prop-what-is.md) - kuvaus avulla voit ratkaista tietoja osista

## <a name="adding-conditional-logic"></a>Lisätty ehdollinen logiikka

Vaikka alkuperäisen työnkulku toimii, on joitakin alueet, jotka voidaan parantaa. 


### <a name="conditional"></a>Ehdollinen
Logiikan sovellus voi johtaa voit käytön sähköpostit paljon. Seuraavat vaiheet Lisää logiikka varmistaaksesi, että saat sähköpostin vain, kun joku on useita seuraajat peräisin tweetin. 

1. Napsauttamalla plusmerkkiä ja Etsi toiminnon *Hae käyttäjän* Twitter.

2. Välittää **Tweeted mukaan** -kentässä käynnistimen Twitter-käyttäjän tietoja.

    ![Hae käyttäjän](./media/app-service-logic-use-logic-app-features/getuser.png)

3. Valitse plus uudelleen, mutta tällä hetkellä valitsemalla **Lisää ehto**

4. Valitse ensimmäiseen ruutuun **...** **Hae käyttäjän** etsiminen **seuraajat count** -kentän alapuolella.

5. Valitse avattavasta valikosta **suurempi kuin**

6. Kirjoita toiseen ruutuun haluat myöntää käyttäjille seuraajat määrä.

    ![Ehdollinen](./media/app-service-logic-use-logic-app-features/conditional.png)

7.  Vetäminen ja pudottaminen Sähköposti-ruudun **Jos Kyllä** -ruutuun. Tämä tarkoittaa saat sähköpostit vain seuraaja Laske täyttyessä.

## <a name="repeating-over-a-list-with-foreach"></a>Toistuva forEach sisältävän luettelon kautta

ForEach silmukan määrittää matriisin toiminnon toistaminen päälle. Jos se ei ole matriisin työnkulku epäonnistuu. Esimerkkinä, jos sinulla on action1, joka siirtää viestit matriisin ja haluat lähettää viesti, voit sisällyttää forEach tietosuojatiedoissa toiminnon ominaisuudet: forEach:"@action('action1').outputs.messages"
 

## <a name="using-the-code-view-to-edit-a-logic-app"></a>Koodi-näkymässä avulla muokataan logiikan-sovellus

Lisäksi suunnittelija voit muokata suoraan koodi, joka määrittää logiikan-sovelluksessa seuraavasti. 

1. Napsauta komentopalkin **Koodinäkymän** -painiketta. 

    Tämä avaa koko-editorin, jolla on vain muokata määritys.

    ![Koodinäkymän](./media/app-service-logic-use-logic-app-features/codeview.png)

    Käyttämällä tekstieditori, voit kopioida ja liittää haluamasi määrän toimintoja saman logiikan sovelluksen tai logiikan-sovelluksesta toiseen. Voit myös helposti lisätä tai poistaa koko osien määrityksestä ja voit myös jakaa määritelmät muiden kanssa.

2. Kun olet tehnyt muutokset koodinäkymän, napsauttamalla **Tallenna**. 

### <a name="parameters"></a>Parametrit
On joitakin ominaisuuksia logiikan sovellukset, jotka voidaan käyttää vain koodi-näkymässä. Esimerkki nämä on parametrit. Parametrien avulla on helppo käyttää uudelleen arvot koko logiikan sovelluksen. Esimerkiksi jos sinulla on sähköpostiosoite, jota haluat käyttää eri toiminnoista, sinun täytyy määritellä se parametrina.

Seuraavat päivittää aiemmin logiikan sovelluksen parametreja käytetään kyselytermin.

1. Koodinäkymän, Etsi `parameters : {}` objekti ja aihe-objektin lisääminen:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
    
2. Siirry `twitterconnector` toiminnon, Etsi arvon kysely ja korvata `#@{parameters('topic')}`.
    **Ketjutettu** -funktion avulla voi myös liittyä yhteen vähintään kaksi merkkijonoja, esimerkiksi: `@concat('#',parameters('topic'))` on sama kuin edellä mainituista. 
 
Parametrit ovat erinomainen tapa tuoda esiin arvot, jotka ovat usein muuttuu todennäköisesti. Ne ovat erityisen hyödyllisiä, kun haluat ohittaa eri ympäristöissä parametrit. Lisätietoja siitä, miten voit ohittaa parametrit-ympäristöön on Microsoftin [REST API-ohjeissa](https://msdn.microsoft.com/library/mt643787.aspx).

Nyt, kun valitset **Tallenna**, tunnissa saat uuden tweets, joissa on enemmän kuin 5 retweets toimitettu kansio nimeltä **tweets** että dropbox.

Lisätietoja logiikan sovelluksen määritykset, katso [tekijän logiikan sovelluksen määritykset](app-service-logic-author-definitions.md).

## <a name="starting-a-logic-app-workflow"></a>Logiikan app työnkulun aloittaminen
On useita erilaisia vaihtoehtoja määritetty logiikan sovelluksessa työnkulun aloittamista. Työnkulun voi aina käynnistää tarvittaessa [Azure portal].

### <a name="recurrence-triggers"></a>Toistuminen Käynnistimet
Toistuminen käynnistin suoritetaan, jotka määrität välein. Kun käynnistin ehdollinen logiikka, käynnistin määrittää, onko työnkulku on suoritettava. Käynnistimen ilmaisee suorittaa palauttamalla `200` tilakoodi. Kun Suorita ei tarvita, se palauttaa `202` tilakoodi.

### <a name="callback-using-rest-apis"></a>REST API käyttämällä takaisinsoitto
Palvelujen soittaa logiikan-sovelluksen päätepisteen työnkulun aloittaminen. Lisätietoja on kohdassa [logiikan sovellusten kutsuttava päätepisteet nimellä](app-service-logic-connector-http.md) . Alkavan, millaisia logiikan app pyydettäessä, napsauta komentopalkin **Suorita** -painiketta. 

<!-- Shared links -->
[Azure portal]: https://portal.azure.com 