<properties
    pageTitle="Azure CDN välimuistiin pyynnöt ja kyselymerkkijonot toiminnan valvominen | Microsoft Azure"
    description="Välimuistin ohjausobjektit, miten tiedostot ovat välimuistiin, kun ne sisältävät kyselymerkkijonoja Azure CDN-kyselymerkkijonon."
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings"></a>CDN-pyyntöjen kyselymerkkijonot välimuistiin toiminnan valvominen

> [AZURE.SELECTOR]
- [Vakio](cdn-query-string.md)
- [Azure CDN Premium-Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Yleiskatsaus

Kyselymerkkijonon välimuistiin ohjausobjektit, miten tiedostot ovat välimuistiin, kun ne sisältävät kyselymerkkijonoja.

> [AZURE.IMPORTANT] Vakio- ja Premium CDN tuotteet tarjoavat saman kyselymerkkijonon välimuistiin toimintoja, mutta käyttöliittymän eroaa.  Tässä asiakirjassa kerrotaan liittymän **Azure CDN vakio-Akamai** ja **Azure CDN vakio-Verizon**.  Kyselymerkkijonon välimuistiin **Azure CDN Premium-Verizon**kanssa, katso [hallinta välimuistiin toiminta CDN-pyyntöjen kyselymerkkijonoja - Premium](cdn-query-string-premium.md).

Kolmessa ovat käytettävissä:

- **Ohita kyselymerkkijonoja**: Tämä on oletusarvo.  CDN reuna-solmu välittää kyselymerkkijonon jostakin pyytäjän alkuperän ensimmäisen pyynnön ja välimuistin kohteen.  Kaikki seuraavat pyynnöt, että annetaan reuna-solmu-palvelimella olevat ohittaa kyselymerkkijonon, kunnes välimuistissa resurssi vanhenee.
- **Ohita välimuistiasetukset kyselyn merkkijonojen URL-osoite**: Tässä tilassa pyynnöt ja kyselymerkkijonoja ei tallenneta välimuistiin CDN reuna-solmu-palvelussa.  Reuna-solmu hakee kohteen suoraan alkuperän ja välittää sen pyytäjän sivupyynnön kanssa.
- **Välimuistin jokaisen yksilöllinen URL-osoite**: Tässä tilassa tulkitsee sivupyynnön kanssa kyselymerkkijonon yksilöllinen resurssi omassa välimuistin kanssa.  *Foo.ashx?q=bar* hakemus origin vastausta tapauksessa esimerkiksi välimuistiin reuna-solmu osoitteessa ja palauttaa saman kyselymerkkijono kanssa myöhemmin tallentaa.  Pyyntö *foo.ashx?q=somethingelse* välimuistin kuin erillisessä resurssi omassa ajan Live.

##<a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Kyselymerkkijonon välimuistiasetukset vakio CDN profiilien muuttaminen

1. Napsauta haluat hallita CDN päätepisteen CDN profiili-sivu.

    ![CDN profiilin sivu päätepisteet](./media/cdn-query-string/cdn-endpoints.png)

    CDN päätepisteen-sivu avautuu.

2. Valitse **Määritä** -painiketta.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-query-string/cdn-config-btn.png)

    CDN määritys-sivu avautuu.

3. Valitse asetus avattavasta valikosta **kyselymerkkijonon välimuistiin toiminta** .

    ![CDN kyselymerkkijonon välimuistiasetukset](./media/cdn-query-string/cdn-query-string.png)

4. Kun olet tehnyt valinnan, napsauta **Tallenna** -painiketta.

> [AZURE.IMPORTANT] Asetuksiin tehdyt muutokset ei välttämättä näy heti, kun kuluvaa aikaa rekisteröinnin kautta CDN leviäminen.  <b>Azure-Akamai CDN</b> profiilien välittäminen kestää yleensä yhden minuutin kuluessa.  <b>Azure CDN Verizon-</b> profiileista välittäminen yleensä kestää 90 minuutin kuluessa, mutta joissakin tapauksissa saattaa toimia hitaasti.
