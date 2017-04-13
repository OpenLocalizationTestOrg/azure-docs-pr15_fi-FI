<properties
     pageTitle="Käyttämällä Azure CDN | Microsoft Azure"
     description="Tässä ohjeaiheessa esitellään ottamisesta käyttöön sisällön lähettämisen verkon (CDN) Azure varten. Opetusohjelman käydään läpi CDN uusi profiili ja päätepisteen luominen."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>Azure CDN käyttäminen  

Tässä artikkelissa käydään läpi Azure CDN ottaminen käyttöön luomalla uuden CDN profiilin ja päätepiste.

>[AZURE.IMPORTANT] Katso esittely CDN toimintaa, toimintojen luettelo sekä [CDN yleiskatsaus](./cdn-overview.md).

## <a name="create-a-new-cdn-profile"></a>Luo uusi profiili CDN

CDN profiili on kokoelma CDN päätepisteet.  Kunkin profiilin sisältää vähintään yhden CDN päätepisteet.  Haluat ehkä käyttää profiileja järjestämiseen CDN-päätepisteet internet-toimialueen, web-sovelluksen tai muun kriteerin perusteella.

> [AZURE.NOTE] Oletusarvon mukaan yhden Azure-tilauksen on rajoitettu kahdeksan CDN-profiileista. CDN kunkin profiilin on rajoitettu 10 CDN päätepisteet.
>
> CDN hinnat otetaan käyttöön CDN profiilin tasolla. Jos haluat käyttää Azure CDN hinnoittelu tasoa, sinun on useita CDN-profiileista.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Luo uusi CDN päätepiste

**Luo uusi CDN päätepiste**

1. Siirry [Azure Portal](https://portal.azure.com)CDN profiilin.  Voit ehkä on kiinnitetty sitä Raporttinäkymät-ikkunan edellisessä vaiheessa.  Jos sinulla ei ole, löydät sen valitsemalla **Selaa**ja valitse **CDN-profiileista**ja valitsemalla haluat lisätä oman päätepiste profiilissa.

    CDN profiili-sivu tulee näkyviin.

    ![CDN profiili][cdn-profile-settings]

2. Napsauta **Lisää päätepisteen** -painiketta.

    ![Lisää päätepisteen-painike][cdn-new-endpoint-button]

    **Lisää päätepisteen** -sivu tulee näkyviin.

    ![Lisää päätepisteen sivu][cdn-add-endpoint]

3. Anna tämän CDN päätepisteen **nimi** .  Tätä nimeä käytetään käyttää välimuistissa resurssien etsiminen toimialueen `<endpointname>.azureedge.net`.

4. Valitse **Origin tyyppi** avattavasta valikosta origin tyyppi.  Valitse Azure-tallennustilan tilin **tallennustilan** , Azure-pilvipalvelussa **pilvipalvelussa** , **Web App** Azure Web App-tai **Mukautettu origin** kaikki muut kaikkien käytettävissä web server origin (isännöidään Azure tai muualla).

    ![CDN origin tyyppi](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. Valitse tai kirjoita origin toimialueen **Origin hostname** avattavasta valikosta.  Avattavan luettelon sisältyviä määritetyn vaiheessa 4 käytettävissä alkuperän.  Jos olet valinnut *Mukautettu origin* oman **Origin Kirjoita**, voit kirjoittaa mukautetun origin toimialueeseen.

6. Kirjoita polku **Origin polku** -muokkausruutuun resurssien välimuistin haluat tai jättää tyhjäksi, jos haluat sallia välimuistin minkä tahansa resurssin vaiheessa 5 määrittämäsi toimialue.

7. Kirjoita **Origin toimialuenimi**haluat haluat lähettää sivupyynnön CDN toimialuenimi tai käytä oletusnimeä.

    > [AZURE.WARNING] Tietyntyyppiset alkuperän Azuren tallennustilaan ja online-sovellusten, kuten edellyttävät vastaamaan alkuperän toimialueen toimialuenimi. Ellei sinulla ole origin, joka edellyttää eri sen toimialueesta toimialuenimi, Jätä oletusarvo.

8. Määritä **protokolla** ja **Origin**, käyttää resurssien osoitteessa alkuperän portit ja protokollat.  Vähintään yksi protokolla (HTTP tai HTTPS) on oltava valittuna.
    
    > [AZURE.NOTE] **Origin portin** koskee vain mitä portti päätepisteen käyttää voit hakea tietoja alkuperän.  Päätepisteen itse vain ovat käytettävissä päätä asiakkaat oletusarvoiseen HTTP ja HTTPS-porttien (80 ja 443) **Origin portin**riippumatta.  
    >
    > **Azure-Akamai CDN** päätepisteet eivät salli alkuperistä koko TCP-porttialue.  Origin portit, jotka eivät ole sallittuja luettelo on artikkelissa [Azure CDN-Akamai sallitut Origin portit](https://msdn.microsoft.com/library/mt757337.aspx).  
    >
    > Käytettäessä CDN-sisällön liittämiseen HTTPS on seuraavat rajoitukset:
    > 
    > - Sinun on käytettävä SSL-varmenne CDN myöntämä. Kolmannen osapuolen varmenteet eivät ole tuettuja.
    > - Sinun on käytettävä antamalla CDN toimialueen (`<endpointname>.azureedge.net`) access HTTPS-sisältöön. HTTPS-tuki ei ole käytettävissä mukautettuja toimialuenimiä (CNAMEs), koska CDN ei tue mukautetun varmenteet tällä hetkellä.

9. Luo uusi päätepiste valitsemalla **Lisää** .

10. Kun päätepiste on luotu, se näkyy päätepisteet profiilin luettelo. Luettelonäkymä näyttää käyttämään välimuistissa olevaa sisältöä sekä origin toimialueen URL-osoite.

    ![CDN päätepiste][cdn-endpoint-success]

    > [AZURE.IMPORTANT] Päätepisteen heti eivät voi käyttää, kun kuluvaa aikaa rekisteröinnin kautta CDN leviäminen.  <b>Azure-Akamai CDN</b> profiilien välittäminen kestää yleensä yhden minuutin kuluessa.  <b>Azure CDN Verizon-</b> profiileista välittäminen yleensä kestää 90 minuutin kuluessa, mutta joissakin tapauksissa saattaa toimia hitaasti.
    >    
    > Käyttäjä yrittää käyttää CDN toimialuenimeä, ennen kuin päätepistemääritys on välitetty avautuu näyttöön tulee HTTP 404 vastauksen koodit.  Jos aikaa on kulunut useita tunteja, koska olet luonut päätepiste ja ole silti saanut 404 vastaukset, lue [vianmäärityksen CDN päätepisteet 404 tilat, joka palauttaa](cdn-troubleshoot-endpoint.md).


##<a name="see-also"></a>Katso myös
- [Pyynnöt ja kyselymerkkijonot välimuistiin toiminnan valvominen](cdn-query-string.md)
- [Voit yhdistää CDN sisältöä mukautetulla toimialueella](cdn-map-content-to-custom-domain.md)
- [Lataa valmiiksi Azure CDN päätepisteen resurssit](cdn-preload-endpoint.md)
- [Tyhjennä Azure CDN päätepisteen](cdn-purge-endpoint.md)
- [Vianmääritys CDN päätepisteet palauttaminen 404 tilat](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
