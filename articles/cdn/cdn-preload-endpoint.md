<properties
    pageTitle="Lataa valmiiksi varat Azure CDN päätepisteen | Microsoft Azure"
    description="Opi lataamaan CDN päätepisteen välimuistissa sisällön valmiiksi."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Lataa valmiiksi Azure CDN päätepisteen resurssit

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Oletusarvon mukaan varat ensin välimuistiin, kun niitä pyydetään. Tämä tarkoittaa, että kunkin alueen ensimmäinen pyyntö saattaa kestää kauemmin, koska reunapalvelimet ei ole sisällön välimuistiin ja toteuttaa lähettää pyynnön edelleen alkuperäisestä palvelimesta. Tätä ensimmäistä osumien viive ennen sisällön lataamista ei.

Lisäksi tarjoaa paremman käyttökokemuksen ladataan valmiiksi välimuistissa olevia varoja myös pienentää verkkoliikenteen alkuperäisestä palvelimesta.

> [AZURE.NOTE] Ladataan valmiiksi varat on hyötyä suuria tapahtumia tai sisältöä, joka on yhtä aikaa käytettävissä suuri määrä käyttäjiä, kuten uusien elokuva tai ohjelmistopäivitys.

Tässä opetusohjelmassa esitellään valmiiksi ladataan kaikissa Azure CDN reunan solmuissa välimuistissa olevaa sisältöä.

## <a name="walkthrough"></a>Vaiheittainen kuvaus

1. Siirry [Azure Portal](https://portal.azure.com)sisältävä haluat ladata etukäteen päätepisteen CDN profiilin.  Profiili-sivu avautuu.

2. Valitse luettelosta päätepiste.  Päätepisteen-sivu avautuu.

3. Valitse CDN päätepisteen-sivu napsauttamalla Lataa-painiketta.

    ![CDN päätepisteen sivu](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    Lataa-sivu avautuu.

    ![CDN kuormituksen sivu](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. Kunkin resurssi, jonka haluat ladata koko polku (esimerkiksi `/pictures/kitten.png`) **polku** -tekstiruutuun.

    > [AZURE.TIP] Lisää **polku** tekstiruutujen tulee näkyviin, kun kirjoitat tekstiä, jotta voit luoda useita kohteita.  Voit poistaa kohteita luettelosta napsauttamalla kolmea pistettä (…)-painiketta.
    >
    > Polkujen on oltava suhteellinen URL-osoite, joka sopii seuraava [säännöllinen lauseke](https://msdn.microsoft.com/library/az24scfc.aspx): `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Kukin resurssi on oltava oma polku.  Ei ole valmiiksi lastaus resurssien osalta yleismerkkien-toimintoa.

    ![Lataa-painike](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. Napsauttamalla **Lataa** -painiketta.

    ![Lataa-painike](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] On 10 kuormituksen pyynnöt minuutissa CDN profiilissa rajoitus.

## <a name="see-also"></a>Katso myös
- [Tyhjennä Azure CDN päätepisteen](cdn-purge-endpoint.md)
- [Azure CDN REST API-viittaus - Tyhjennä tai lataat valmiiksi päätepisteen](https://msdn.microsoft.com/library/mt634451.aspx)
