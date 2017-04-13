<properties
    pageTitle="Hallinta Azure CDN Premium-välimuistin toiminta kyselymerkkijonoja-pyyntöjen Verizon | Microsoft Azure"
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings---premium"></a>CDN-pyyntöjen kyselymerkkijonoja - Premium välimuistiin toiminnan valvominen

> [AZURE.SELECTOR]
- [Vakio](cdn-query-string.md)
- [Azure CDN Premium-Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Yleiskatsaus

Kyselymerkkijonon välimuistiin ohjausobjektit, miten tiedostot ovat välimuistiin, kun ne sisältävät kyselymerkkijonoja.

> [AZURE.IMPORTANT] Vakio- ja Premium CDN tuotteet tarjoavat saman kyselymerkkijonon välimuistiin toimintoja, mutta käyttöliittymän eroaa.  Tässä asiakirjassa kerrotaan liittymän **Azure CDN**Premium-Verizon.  Kyselymerkkijonon välimuistiin **Azure CDN vakio-Akamai** ja **Azure CDN vakio-Verizon**kanssa, katso [hallinta välimuistiin toiminta CDN-pyyntöjen kyselymerkkijonoja](cdn-query-string.md).

Kolmessa ovat käytettävissä:

- **Vakio-välimuistin**: Tämä on oletusarvo.  CDN reuna-solmu välittää kyselymerkkijonon jostakin pyytäjän alkuperän ensimmäisen pyynnön ja välimuistin kohteen.  Kaikki seuraavat pyynnöt, että annetaan reuna-solmu-palvelimella olevat ohittaa kyselymerkkijonon, kunnes välimuistissa resurssi vanhenee.
- **ei-välimuistin**: Tässä tilassa pyynnöt ja kyselymerkkijonoja ei tallenneta välimuistiin CDN reuna-solmu-palvelussa.  Reuna-solmu hakee kohteen suoraan alkuperän ja välittää sen pyytäjän sivupyynnön kanssa.
- **yksilöivä välimuistin**: Tässä tilassa tulkitsee sivupyynnön kanssa kyselymerkkijonon yksilöllinen resurssi omassa välimuistin kanssa.  *Foo.ashx?q=bar* hakemus origin vastausta tapauksessa esimerkiksi välimuistiin reuna-solmu osoitteessa ja palauttaa saman kyselymerkkijono kanssa myöhemmin tallentaa.  Pyyntö *foo.ashx?q=somethingelse* välimuistin kuin erillisessä resurssi omassa ajan Live.

##<a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Kyselymerkkijonon välimuistiasetukset premium CDN profiilien muuttaminen

1. Valitse **hallinta** -painike CDN profiili-sivu.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-query-string-premium/cdn-manage-btn.png)

    CDN hallinta-portaalin avautuu.

2. Vie hiiren osoitin **HTTP suuri** -välilehti ja valitse viemällä hiiren osoittimen **Asetukset** -valikon avauspainike.  Valitse **kyselymerkkijonon välimuistin**.

    Kyselyn merkkijono välimuistin asetukset ovat näkyvissä.

    ![CDN kyselymerkkijonon välimuistiasetukset](./media/cdn-query-string-premium/cdn-query-string.png)

3. Kun olet tehnyt valinnan, napsauta **Päivitä** -painiketta.


> [AZURE.IMPORTANT] Asetuksiin tehdyt muutokset ei välttämättä näy heti, kun kuluvaa aikaa rekisteröinnin kautta CDN leviäminen.  <b>Azure CDN Verizon-</b> profiileista välittäminen yleensä kestää 90 minuutin kuluessa, mutta joissakin tapauksissa saattaa toimia hitaasti.
