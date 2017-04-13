<properties
     pageTitle="Voit yhdistää Azure sisällön toimittamisen verkon (CDN) sisällön mukautetun toimialueen | Microsoft Azure"
     description="Tässä ohjeaiheessa esitellään yhdistämisestä CDN sisällön mukautettu toimialue."
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
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Yhdistämisestä mukautetun toimialueen sisällön toimituksen (CDN) päätepiste
Voit yhdistää mukautetun toimialueen CDN päätepisteen voi käyttää omaa toimialuenimeä URL välimuistissa olevaa sisältöä käyttämällä alitoimialuetta azureedge.net sijaan.

Yhdistä mukautetun toimialueen CDN päätepisteen kahdella tavalla:

1. [Luo CNAME-tietue, jossa toimialueen rekisteröintipalvelu ja Yhdistä mukautetun toimialueen ja -alitoimialueelle CDN päätepiste](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    CNAME-tietue on DNS-ominaisuus, joka yhdistää tietolähteen toimialueen, kuten `www.contosocdn.com` tai `cdn.contoso.com`, kohde-toimialueeseen. Tässä tapauksessa Lähdetoimialue on mukautettu toimialue ja subdomain (alitoimialue, kuten **www** - tai **cdn** on aina pakollinen). Kohdetoimialueen on CDN päätepiste.  

    Yhdistämistä mukautetun toimialueen CDN päätepisteen voi kuitenkin johtaa käyttökatkot toimialueen lyhyt ajan samalla, kun toimialue on rekisteröiminen Azure-portaalissa.

2. [**Cdnverify** keskitason rekisteröinti vaiheen lisääminen](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    Jos sovellus, joka edellyttää, joka ei ole käyttökatkot palvelun tason-sopimus (SLA) on tällä hetkellä tukemista mukautetun toimialueen, voit käyttää Azure **cdnverify** alitoimialueen antamaan keskitason rekisteröintivaihe, niin, että käyttäjät voi käyttää toimialueen DNS yhdistäminen tapahtuu aikana.  

Kun olet rekisteröinyt jollakin edellä mainittujen mukautetun toimialueen, haluat [tarkistaa mukautetun alitoimialueen viittaa CDN päätepiste](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [AZURE.NOTE] Jos haluat yhdistää toimialueen CDN päätepisteen kanssa, sinun on luotava CNAME-tietue. CNAME-tietueet Yhdistä tietyn alitoimialueita `www.contoso.com` tai `cdn.contoso.com`. Ei voida yhdistää CNAME-tietue päätoimialue, kuten `contoso.com`.
>    
> Alitoimialue voi olla vain yksi CDN päätepiste liity. CNAME-tietue, jonka luot reitittää kaikki liikenne osoitettu alitoimialueen määritetyn päätepisteelle.  Jos liität esimerkiksi `www.contoso.com` CDN-päätepiste, jonka voit ei voi liittää sen muihin Azure päätepisteet, kuten tallennustilan tilin-päätepisteen tai cloud-palvelupäätepiste. Kuitenkin voit käyttää eri alitoimialueita saman toimialueen eri päätepisteiden. Voit myös yhdistää eri alitoimialueita saman CDN päätepisteelle.
>
> Huomaa, että kestää mukautetun toimialuemuutokset välittäminen CDN reunan solmujen **90 minuuttia** päätepisteet **Azure CDN-Verizon** (vakio ja Premium).

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Mukautetun toimialueen käyttäminen Azure CDN päätepisteen rekisteröiminen

1.  Lokitiedoston [Azure Portal](https://portal.azure.com/).
2.  Valitse **Selaa**, valitse **CDN-profiileista**sitten CDN profiili, jolla haluat yhdistää mukautetun toimialueen päätepiste.  
3.  Valitse **CDN profiili** -sivu, johon haluat liittää alitoimialueen CDN päätepiste.
4.  Päätepisteen sivu yläreunassa olevaa **Lisää mukautettu toimialue** -painiketta.  **Lisää mukautettu toimialue** -sivu tulee päätepisteen isäntänimi johdettu CDN-päätepisteen avulla uusi CNAME-tietue. Host name-osoitteen muoto näkyy ** &lt;EndpointName >. azureedge.net**.  Voit kopioida tämän isäntänimi käyttäminen CNAME-tietueen luomisessa.  
5.  Siirry toimialuerekisteröijän sivuston ja Etsi kohta DNS-tietueiden luomista varten. Saatat huomata esimerkiksi **Toimialuenimi**, **DNS**tai **Nimi hallinta**-osassa.
6.  Etsi kohta CNAMEs hallintaan. Saatat joutua Siirry Lisäasetukset-sivulla ja Etsi sanoja CNAME Alias tai alitoimialueita.
7.  Luo uusi CNAME-tietue, joka yhdistää valitun alitoimialueen (esimerkiksi **www** tai **cdn**) isäntänimi säädetty **lisätään mukautettu toimialue** -sivu.
8.  Palaa **lisätään mukautettu toimialue** -sivu ja kirjoita mukautettua toimialuetta, mukaan lukien alitoimialueen-valintaikkunassa. Kirjoita esimerkiksi muodossa toimialuenimi `www.contoso.com` tai `cdn.contoso.com`.   

    Azure tarkistamaan, että CNAME-tietue on määrittämäsi toimialuenimi. Jos CNAME-TIETUE on oikein, mukautettu toimialue on vahvistettu.  **Azure-Verizon CDN** (vakio ja Premium) päätepisteet voi kestää jopa 90 minuuttia leviäminen kaikkien CDN reunan solmujen, mutta mukautetun toimialueasetukset.  

    Huomaa, että joissakin tapauksissa saattaa kestää leviäminen nimi-palvelimiin Internetissä CNAME-tietueen. Jos toimialueen ei ole vahvistettu välittömästi ja uskot CNAME-tietue on oikein, odota muutama minuutti ja yritä uudelleen.


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Mukautetun toimialueen käyttäminen välittävät cdnverify alitoimialueen Azure CDN päätepisteen rekisteröiminen  

1. Lokitiedoston [Azure Portal](https://portal.azure.com/).
2. Valitse **Selaa**, valitse **CDN-profiileista**sitten CDN profiili, jolla haluat yhdistää mukautetun toimialueen päätepiste.  
3. Valitse **CDN profiili** -sivu, johon haluat liittää alitoimialueen CDN päätepiste.
4. Päätepisteen sivu yläreunassa olevaa **Lisää mukautettu toimialue** -painiketta.  **Lisää mukautettu toimialue** -sivu tulee päätepisteen isäntänimi johdettu CDN-päätepisteen avulla uusi CNAME-tietue. Host name-osoitteen muoto näkyy ** &lt;EndpointName >. azureedge.net**.  Voit kopioida tämän isäntänimi käyttäminen CNAME-tietueen luomisessa.
5. Siirry toimialuerekisteröijän sivuston ja Etsi kohta DNS-tietueiden luomista varten. Saatat huomata esimerkiksi **Toimialuenimi**, **DNS**tai **Nimi hallinta**-osassa.
6. Etsi kohta CNAMEs hallintaan. Saatat joutua Siirry Lisäasetukset-sivulla ja Etsi sanoja **CNAME**- **Alias**tai **alitoimialueita**.
7. Luo uusi CNAME-tietue ja anna alitoimialueen alias, joka sisältää **cdnverify** alitoimialueen. Esimerkiksi määrität lisäämäsi alitoimialue näkyvät muodossa **cdnverify.www** tai **cdnverify.cdn**. Kirjoita isäntänimi, joka on CDN-päätepisteen muodossa **cdnverify.&lt; EndpointName >. azureedge.net**.
8. Palaa **lisätään mukautettu toimialue** -sivu ja kirjoita mukautettua toimialuetta, mukaan lukien alitoimialueen-valintaikkunassa. Kirjoita esimerkiksi muodossa toimialuenimi `www.contoso.com` tai `cdn.contoso.com`. Huomaa, että tässä vaiheessa sinun ei tarvitse tekstimuotoisen alitoimialueen **cdnverify**kanssa.  

    Azure tarkistamaan, että CNAME-tietue on kirjoittamasi cdnverify toimialuenimi.
9. Tässä vaiheessa mukautettu toimialue on vahvistettu Azure mukaan, mutta toimialueesi liikenne ei ole vielä reitittyvät CDN päätepiste. Kun odottaa niin pitkä, jotta mukautettua toimialuetta asetukset leviäminen CDN reunan solmujen (90 minuuttia **Azure CDN-Verizon**, 1-2 minuuttia **Azure CDN-Akamai**), palaa DNS Rekisteröintipalvelun web-sivuston ja luo toinen CNAME-tietue, joka yhdistää oman alitoimialueen CDN päätepiste. Esimerkiksi määrittää alitoimialueen **www** tai **cdn**ja isäntänimi kuin ** &lt;EndpointName >. azureedge.net**. Tässä vaiheessa mukautetun toimialueen rekisteröinti on valmis.
10. Voit poistaa luomasi käyttämällä **cdnverify**CNAME-tietue-sellaisena kuin se oli välittävät vaiheena tarvittavat.  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Tarkista, mukautetut alitoimialueen viittaa CDN päätepiste

- Sen jälkeen, kun olet suorittanut rekisteröinnin mukautetun toimialueen, voit käyttää sisältöä, joka on välimuistissa osoitteessa mukautetun toimialueen CDN päätepiste.
Varmista ensin, että sinulla on julkinen sisältöön, joka päätepisteen osoitteessa välimuistissa. Jos CDN päätepisteen on liitetty tallennustilan-tiliin, CDN välimuistiin julkisen blob säilöjen sisällön. Voit esikatsella mukautettua toimialuetta, varmista, että säilö on määritetty sallimaan yleisön ja että se sisältää vähintään yhden blob.
- Siirry selaimella mukautetun toimialueen Blob-objektien osoitetta. Jos mukautetun toimialueen on esimerkiksi `cdn.contoso.com`, välimuistin Blob-objektien URL-osoite on samankaltainen kuin seuraava URL-osoite: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Katso myös

[Kuinka otetaan käyttöön Azure sisällön toimittamisen verkon (CDN)](./cdn-create-new-endpoint.md)  
