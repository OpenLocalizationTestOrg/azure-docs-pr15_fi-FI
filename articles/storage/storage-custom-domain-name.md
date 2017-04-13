<properties
    pageTitle="Määritä Blob storage päätepiste toimialuenimen | Microsoft Azure"
    description="Opettele mukautettuja toimialueen yhdistäminen Blob storage päätepisteen Azure-tallennustilan tilin perinteinen Azure-portaalissa."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Mukautetun toimialuenimen Blob storage-päätepisteen määrittäminen

## <a name="overview"></a>Yleiskatsaus

Voit määrittää mukautetun toimialueen käyttäminen Azure-tallennustilan tilin tietojen Blob-objektien käyttäminen. Blob-objektien tallennustilaan päätepisteen oletusarvo on `<storage-account-name>.blob.core.windows.net`. Jos yhdistät mukautettua toimialuetta ja alitoimialue, kuten **www.contoso.com** blob-päätepisteen tallennustilan tili ja sitten käyttäjät voivat myös access-Blob-objektien tietojen-tallennustilan tilin käyttäminen toimialueen.

>[AZURE.IMPORTANT] Azure-tallennustilan ei tue mukautettujen toimialueiden HTTPS vielä. Emme tietoinen siitä, että asiakkaat voivat olla kiinnostuneita tätä ominaisuutta, ja se on käytettävissä uuteen versioon.

Mukautetun toimialueen osoittamaan tallennustilan tilin blob-päätepisteen kahdella eri tavalla. Helpoin tapa on luoda yhdistäminen mukautetun toimialueen ja -alitoimialueelle blob-päätepisteen CNAME-tietue. CNAME-tietue on DNS-ominaisuus, joka yhdistää lähdetoimialueen kohdetoimialueella. Tässä tapauksessa Lähdetoimialue on mukautetun toimialueen ja -alitoimialueelle--Huomaa, että alitoimialue on aina pakollinen. Kohdetoimialueen on Blob-palvelupäätepiste.

Yhdistämistä mukautetun toimialueen blob-päätepisteen voi kuitenkin johtaa käyttökatkot toimialueen lyhyt ajan samalla, kun toimialue on rekisteröiminen [Azure perinteinen Portal](https://manage.windowsazure.com). Jos sovellus palvelun tason-sopimus (SLA), joka edellyttää, joka ei ole käyttökatkot on tällä hetkellä tukemista mukautetun toimialueen, voit käyttää Azure **asverify** alitoimialueen antamaan keskitason rekisteröintivaihe niin, että käyttäjät voi käyttää toimialueen DNS yhdistäminen tapahtuu aikana.

Seuraavassa taulukossa on esimerkki URL-osoitteet nimeltä **mystorageaccount**tallennustilan tilillä tietojen Blob-objektien käyttäminen. Tallennustilan tilin rekisteröity mukautettu toimialue on **www.contoso.com**:

Resurssilaji|URL-muodot
---|---
Tallennustilan tilin|**Oletusarvoinen URL-osoite:** http://mystorageaccount.blob.core.windows.net<p>**Mukautettu toimialueen URL-osoite:** http://www.contoso.com</td>
BLOB|**Default URL-osoite:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**Mukautettu toimialueen URL-osoite:** http://www.contoso.com/mycontainer/myblob
Pääkansio säilö|**Oletusarvoinen URL-osoite:** http://mystorageaccount.blob.core.windows.net/myblob tai http://mystorageaccount.blob.core.windows.net/$ pääkansion/myblob<p>**Mukautettu toimialueen URL-osoite:** http://www.contoso.com/myblob tai http://www.contoso.com/$ pääkansion/myblob

## <a name="register-a-custom-domain-for-your-storage-account"></a>Mukautetun toimialueen tallennustilan tilin rekisteröiminen

Tämän toiminnon avulla voit rekisteröidä mukautetun toimialueen, jos sinulla ei ole epävarma ottaa lyhyesti käyttäjät eivät saa toimialueen tai mukautettua toimialuetta ei ole tällä hetkellä olisi isännöi sovelluksen.

Jos sovellus, joka ei voi olla mikä tahansa käyttökatkot tällä hetkellä tukemista mukautetun toimialueen, käytä <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">mukautetun toimialueen käyttäminen välittävät asverify alitoimialueen tallennustilan tilin rekisteröiminen</a>kuvatut toimet.

Voit määrittää mukautetun toimialuenimen, sinun on luotava uusi CNAME-tietue toimialueen rekisteröintipalvelu kanssa. CNAME-tietue määrittää tunnusnimen toimialuenimi; Tässä tapauksessa yhdistää mukautetun toimialueen osoite Blob storage päätepisteen tallennustilan tilissäsi.

Kunkin rekisteröintipalvelu on samanlainen, mutta se on hieman erilainen tapa CNAME-tietue, joka määrittää, mutta joka on sama. Huomaa, että useita basic toimialueen rekisteröinti pakettien ei tarjoa DNS-määrityksiä, niin voit joutua päivittämään toimialueen rekisteröinti-paketti, ennen kuin voit luoda CNAME-tietue.

1.  Siirry [Azure perinteinen Portal](https://manage.windowsazure.com) **tallennustila** -välilehdessä.

2.  Valitse **tallennustila** -välilehdessä, jonka haluat yhdistää mukautetun toimialueen tallennustilan tilin nimi.

3.  Valitse **määritys** -välilehti.

4.  Näytön alareunassa valitsemalla **Hallitse toimialueen** **Mukautetun toimialueen hallinta** -valintaikkunan näyttämiseen. Teksti-valintaikkunan yläreunassa näet tietoja siitä, miten voit luoda CNAME-tietue. Tämä toiminto, joka viittaa **asverify** alitoimialueen tekstin ohittaminen.

5.  Kirjaudu sisään DNS-Rekisteröintipalvelun sivustossa, ja siirry hallintasivu DNS. Saatat huomata esimerkiksi **Toimialuenimi**, **DNS**tai **Nimi hallinta**-osassa.

6.  Etsi kohta CNAMEs hallintaan. Saatat joutua Siirry Lisäasetukset-sivulla ja Etsi sanoja **CNAME**- **Alias**tai **alitoimialueita**.

7.  Luo uusi CNAME-tietue ja anna alitoimialueen alias, kuten **www** - tai **kuvia**. Kirjoita isäntänimi, joka on Blob-objektien Palvelupäätepiste, valitse Muotoile- **mystorageaccount.blob.core.windows.net** (jossa **mystorageaccount** on tallennustilan tilin nimi). Isäntänimi käyttämään annetaan puolestasi **Mukautetun toimialueen hallinta** -valintaikkunan näytettävä teksti.

8.  Kun olet luonut CNAME-tietue, palaa **Mukautettua toimialuetta hallinta** -valintaikkunaan ja alitoimialue, kuten **Mukautetun toimialuenimi** -kentässä mukautetun toimialueen nimi. Esimerkiksi jos toimialueesi on **contoso.com** ja että alitoimialue on **WWW-tunnistetta**, kirjoita **www.contoso.com**; Jos yhteyttä alitoimialue on **valokuvia**, kirjoita **photos.contoso.com**. Huomaa, että alitoimialue on tehty.

9. Mukautetun toimialuenimen rekisteröinnin valitsemalla **Rekisteröi** .

    Jos rekisteröinti onnistuu, näyttöön tulee sanoma, joka on **Mukautettu toimialue on aktiivinen**. Käyttäjät voivat nyt Blob-objektien tietojen mukautetun toimialueen, kunhan heillä on tarvittavat käyttöoikeudet.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Mukautetun toimialueen käyttäminen välittävät asverify alitoimialueen tallennustilan tilin rekisteröiminen

Näiden ohjeiden avulla voit rekisteröidä mukautetun toimialueen Jos mukautetun toimialueen tällä hetkellä tukemista hakemus SLA, joka edellyttää, että ongelma voi ei käyttökatkot. Luomalla CNAME-TIETUE, joka osoittaa asverify. &lt;alitoimialueen&gt;. &lt;customdomain&gt; asverify avulla. &lt;storageaccount&gt;. blob.core.windows.net, olet rekisteröinyt toimialueesi Azure valmiiksi. Voit luoda toisen CNAME-TIETUE, joka osoittaa &lt;alitoimialueen&gt;. &lt;customdomain&gt; , &lt;storageaccount&gt;. blob.core.windows.net, jolloin liikenne muuttaminen vastaamaan mukautettua toimialuetta ohjataan blob-päätepiste.

Asverify alitoimialue on erityinen alitoimialuetta Azure tunnista. Soluviittauksen eteen **asverify** , oman alitoimialue, sallit Azure tunnista mukautetun toimialueen muuttamatta toimialueen DNS-tietue. Kun muokkaat toimialueen DNS-tietueen, se yhdistetään ei käyttökatkot blob päätepisteen.

1.  Siirry [Azure perinteinen Portal](https://manage.windowsazure.com) **tallennustila** -välilehdessä.

2.  Valitse **tallennustila** -välilehdessä, jonka haluat yhdistää mukautetun toimialueen tallennustilan tilin nimi.

3.  Valitse **määritys** -välilehti.

4.  Näytön alareunassa valitsemalla **Hallitse toimialueen** **Mukautetun toimialueen hallinta** -valintaikkunan näyttämiseen. Teksti-valintaikkunan yläreunassa näet tietoja siitä, miten voit luoda käyttämällä **asverify** alitoimialueen CNAME-tietue.

5.  Kirjaudu sisään DNS-Rekisteröintipalvelun sivustossa, ja siirry hallintasivu DNS. Saatat huomata esimerkiksi **Toimialuenimi**, **DNS**tai **Nimi hallinta**-osassa.

6.  Etsi kohta CNAMEs hallintaan. Saatat joutua Siirry Lisäasetukset-sivulla ja Etsi sanoja **CNAME**- **Alias**tai **alitoimialueita**.

7.  Luo uusi CNAME-tietue ja anna alitoimialueen alias, joka sisältää asverify alitoimialueen. Esimerkiksi määrität alitoimialueen näkyvät muodossa **asverify.www** tai **asverify.photos**. Kirjoita isäntänimi, joka on Blob-objektien Palvelupäätepiste, valitse Muotoile- **asverify.mystorageaccount.blob.core.windows.net** (jossa **mystorageaccount** on tallennustilan tilin nimi). Isäntänimi käyttämään annetaan puolestasi **Mukautetun toimialueen hallinta** -valintaikkunan näytettävä teksti.

8.  Kun olet luonut CNAME-tietue, palaa **Mukautettua toimialuetta hallinta** -valintaikkunaan ja **Mukautetun toimialuenimi** -kenttään mukautetun toimialueen nimeä. Esimerkiksi jos toimialueesi on **contoso.com** ja että alitoimialue on **WWW-tunnistetta**, kirjoita **www.contoso.com**; Jos yhteyttä alitoimialue on **valokuvia**, kirjoita **photos.contoso.com**. Huomaa, että alitoimialue on tehty.

9.  Valitse valintaruutu, jossa lukee **Lisäasetukset: mukautetun toimialueen preregister 'asverify' alitoimialueen avulla**.

10. Mukautetun toimialueen preregister valitsemalla **Rekisteröi** .

    Jos preregistration onnistuu, näyttöön tulee sanoma, joka on **Mukautettu toimialue on aktiivinen**.

11. Tässä vaiheessa mukautettu toimialue on vahvistettu Azure mukaan, mutta toimialueesi liikenne ei ole vielä reitittyvät tallennustilan tilin. Viimeistele prosessi, palaa DNS Rekisteröintipalvelun sivustossa, ja luo toinen CNAME-tietue, joka yhdistää oman alitoimialueen Blob-palvelupäätepiste. Määritä esimerkiksi alitoimialueen **www** tai **valokuvia**ja isäntänimi kuin **mystorageaccount.blob.core.windows.net** (jossa **mystorageaccount** on tallennustilan tilin nimi). Tässä vaiheessa mukautetun toimialueen rekisteröinti on valmis.

12. Voit poistaa luomasi käyttämällä **asverify**CNAME-tietue-sellaisena kuin se oli välittävät vaiheena tarvittavat.

Käyttäjät voivat nyt Blob-objektien tietojen mukautetun toimialueen, kunhan heillä on tarvittavat käyttöoikeudet.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Varmista, että mukautettu toimialue viittaa Blob-palvelupäätepiste

Voit varmistaa, että mukautettu toimialue on määritetty Blob-Palvelupäätepisteen todellakin, Luo blob storage tilisi julkinen säilöön. Valitse selaimessa, käyttämään URI-Osoitteen seuraavassa muodossa blob:

-   http://<*subdomain.customdomain*>/<*mycontainer*>/<*myblob*>

Seuraavat URI avulla voi käyttää verkkolomakkeen **photos.contoso.com** mukautetun alitoimialueen, joka yhdistää Blob-objektien **myforms** säilössä kautta:

-   http://Photos.contoso.com/myforms/applicationform.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>Unregister mukautetun toimialueen tallennustilan tililtä

Rekisteröintiä mukautetun toimialueen, toimi seuraavasti: 

1. Kirjautuminen [Azure perinteinen Portal](https://manage.windowsazure.com). 

2. Valitse siirtymisruudussa **tallennustilan**. 

3. Valitse **tallennustilan** -sivulla Näytä koontinäyttö tallennustilan-tilin nimi. 

5. Valitse valintanauhan, **Hallita toimialueen**. 

6. Valitse **Mukautettu toimialue hallinta** -valintaikkunan **Poista**. 


## <a name="additional-resources"></a>Lisäresursseja

-   [Yhdistämisestä mukautetun toimialueen sisällön toimituksen (CDN) päätepiste](../cdn/cdn-map-content-to-custom-domain.md)
