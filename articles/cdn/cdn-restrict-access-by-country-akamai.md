<properties
    pageTitle="Azure CDN-sisältöä maittain | Microsoft Azure"
    description="Lisätietoja oman Azure CDN sisältöä Geo suodatus-toiminnon avulla."
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="akucer"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2016"
    ms.author="Lichard"/>

#<a name="restrict-access-to-your-content-by-country---akamai"></a>Rajoittaa sisällön maittain - Akamai

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Akamai standardiksi](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>Yleiskatsaus

Kun käyttäjä pyytää sisältöä, oletusarvoisesti, sisällön served riippumatta siitä, jossa käyttäjä näkyvät tässä pyytää. Haluat ehkä rajoittaa sisällön maittain. Tässä ohjeaiheessa kerrotaan, jotta sallimaan-palvelun määrittäminen **Geo suodatus** -toiminnon käyttämisestä tai maittain käytön estäminen.

> [AZURE.IMPORTANT] Verizon ja Akamai tuotteet tarjoavat samat geo suodattaminen toiminnot, mutta käyttöliittymän eroaa. Tämän asiakirjan kuvattu **Azure CDN vakio-Akamai**käyttöliittymässä. Geo suodatuksen **Azure CDN Standard/Premium-Verizon**kanssa Katso [maa - Verizon sisällön käyttöoikeuden rajoittaminen](cdn-restrict-access-by-country.md).

Lisätietoja huomioon otettavia seikkoja, jotka koskevat määrittäminen tällaista rajoituksen ohjeaiheen lopussa kohdassa [huomioon otettavia seikkoja](cdn-restrict-access-by-country.md#considerations) .  

![Maa-suodatus](./media/cdn-filtering/cdn-country-filtering-akamai.png)

##<a name="step-1-define-the-directory-path"></a>Vaihe 1: Määritä kansiopolku

Valitse portaalin päätepiste ja etsiä Geo suodatus-välilehti, toiminto vasemmanpuoleisessa siirtymisruudussa.

Maa-suodattimen määritettäessä haluamaasi kohtaan, johon käyttäjät on sallittu tai estetty suhteellinen polku on määritettävä. Voit hakea geo suodattaminen kaikki tiedostot, joiden "/" tai kansion polun "/ kuvat /" määrittäminen kansiot valitsemat. Voit myös käyttää geo suodattaminen yhteen tiedostoon määrittämällä tiedoston ja jätät pois vinoviiva "/ pictures/city.png".

Esimerkki kansion polku suodatus:

    /                                 
    /Photos/
    /Photos/Strasbourg/
    /Photos/Strasbourg/city.png

##<a name="step-2-define-the-action-block-or-allow"></a>Vaihe 2: Määritä toiminnon: estäminen tai salliminen

**Estäminen:** Määritetyn maista käyttäjiltä estetään access pyytää rekursiivinen polun vastaavien. Jos ei ole muita maan suodatusasetusten on määritetty kyseisessä sijainnissa, sitten muiden käyttäjien käyttöoikeuksien salliminen.

**Salli:** Vain käyttäjien määritetyn maista salliminen access pyytää rekursiivinen polun kalusto.

##<a name="step-3-define-the-countries"></a>Vaihe 3: Määritä maat

Valitse maat, jotka haluat estäminen tai salliminen polun. Lisätietoja on artikkelissa [Azure CDN-Akamai koodit](https://msdn.microsoft.com/library/mt761717.aspx).

Esimerkiksi säännön estäminen /Photos/Strasbourgissa/suodattaa tiedostojen, kuten:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>Maakoodien

**Geo suodatus** -ominaisuus käyttää koodit maat, joista pyyntö tulee sallia tai estää suojatun kansion määrittämiseen. Etsii koodit [Azure CDN-Akamai koodit](https://msdn.microsoft.com/library/mt761717.aspx). 

##<a id="considerations"></a>Huomioon otettavia seikkoja

- Saattaa kestää muutaman minuutin maasi suodattaminen määritysten muutokset tulevat voimaan.
- Tämä ominaisuus ei tue yleismerkkejä (esimerkiksi "*").
- Suhteellinen polku liittyvät geo suodattaminen määritys on tähän polkuun rekursiivisesti.
- Vain yksi sääntö voi suojata sama suhteellinen polku (ei voi luoda useita maa-suodattimia, jotka osoittavat samaan suhteellinen polku. Kansion voi kuitenkin olla useita maan suodattimia. Tämä on rekursiivinen laatu maan suodattimet. Toisin sanoen aiemmin määritetyn kansion alikansio voi määrittää eri maa-suodatin.

