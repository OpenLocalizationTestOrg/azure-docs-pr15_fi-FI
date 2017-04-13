<properties
    pageTitle="Suorituskyvyn parantaminen pakkaamalla Azure CDN | Microsoft Azure"
    description="Lue, miten voit parantaa tiedoston siirron nopeutta ja kasvattaa sivun kuormituksen suorituskyvyn Azure CDN tiedostojen pakkaamalla."
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

# <a name="improve-performance-by-compressing-files"></a>Suorituskyvyn parantaminen pakkaamalla

Pakkaus on yksinkertainen ja tehokas tapa parantaa tiedoston siirron nopeutta ja suurentaa sivun kuormituksen suorituskyvyn pienentää tiedostokokoa ennen sen lähettämistä palvelimesta. Se pienentää kaistanleveys ja tarjoaa enemmän vastaa kokemus käyttäjiä.

Pakata kahdella tavalla:

- Voit ottaa pakkaamisen origin palvelimessa, jossa kirjainkoko CDN pakatut tiedostot kulkevat ja pitää pakatut tiedostot, joka pyytää niiden asiakkaiden.
- Voit ottaa pakkauksen suoraan CDN reunapalvelimet, jossa kirjainkoko CDN pakkaa tiedostot ja yhteyshenkilönä loppukäyttäjät, vaikka heillä ei pakkaa alkuperäisestä palvelimesta.

> [AZURE.IMPORTANT] CDN määritysmuutoksia kestää jonkin aikaa leviäminen verkon kautta.  <b>Azure-Akamai CDN</b> profiilien välittäminen suorittaa yleensä alle minuutin.  <b>Azure CDN Verizon-</b> profiileista, näet yleensä tekemäsi muutokset koskevat 90 minuuttia.  Jos olet määrittänyt pakkaamisen CDN päätepisteen ensimmäistä kertaa, ota huomioon odottaa 1-2 tuntia ja varmista pakkaamisen asetukset on välitetty ponnahtaa ennen vianmääritys

## <a name="enabling-compression"></a>Pakkaamisen ottaminen käyttöön

> [AZURE.NOTE] Vakio- ja Premium CDN tasoa ovat samat pakkaamisen toiminnot, mutta käyttöliittymän eroaa.  Saat lisätietoja vakio- ja Premium CDN tasoa erot [Azure CDN yleiskatsaus](cdn-overview.md).

### <a name="standard-tier"></a>Vakio taso

> [AZURE.NOTE] Tässä osassa koskee **Azure CDN vakio-Verizon** ja **Azure CDN vakio Akamai-** profiileista.

1. Napsauta haluat hallita CDN päätepisteen CDN profiili-sivu.

    ![CDN profiilin sivu päätepisteet](./media/cdn-file-compression/cdn-endpoints.png)

    CDN päätepisteen-sivu avautuu.

2. Valitse **Määritä** -painiketta.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-file-compression/cdn-config-btn.png)

    CDN määritys-sivu avautuu.

3. Ota **pakkaamisen**.

    ![CDN Pakkaamisasetukset](./media/cdn-file-compression/cdn-compress-standard.png)

4. Käyttää oletusarvon eri tai Muokkaa luetteloa poistamalla tai lisäämällä tiedostotyypit.
    
    > [AZURE.TIP] Aikaa on mahdollista, ei kannattaa käyttää pakkaamisen pakattu tiedostomuotoihin, kuten ZIP, MP3, MP4, JPG.
    
5. Kun olet tehnyt haluamasi muutokset, napsauta **Tallenna** -painiketta.

### <a name="premium-tier"></a>Premium taso

> [AZURE.NOTE] Tässä osassa koskee **Azure CDN Premium Verizon-** profiileista.

1. Valitse **hallinta** -painike CDN profiili-sivu.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-file-compression/cdn-manage-btn.png)

    CDN hallinta-portaalin avautuu.

2. Vie hiiren osoitin **HTTP suuri** -välilehti ja valitse viemällä hiiren osoittimen **Asetukset** -valikon avauspainike.  Valitse **pakkaamisen**.

    Pakkaamisasetukset näytetään.

    ![Tiedostojen pakkaaminen](./media/cdn-file-compression/cdn-compress-files.png)

3. Pakata valitsemalla **Pakkaus käytössä** -valintanappi.  Kirjoita MIME-tyypit, haluatko pakata luettelona pilkuilla erotettu (ilman välilyöntejä) **Tiedostotyypit** -tekstiruutuun.
        
    > [AZURE.TIP] Aikaa on mahdollista, ei kannattaa käyttää pakkaamisen pakattu tiedostomuotoihin, kuten ZIP, MP3, MP4, JPG. 

4. Kun olet tehnyt haluamasi muutokset, valitse **Päivitä** -painiketta.


## <a name="compression-rules"></a>Pakkaamisen säännöt

Seuraavassa taulukossa kuvataan Azure CDN pakkaamisen toiminta joka tapauksessa.

> [AZURE.IMPORTANT] **Azure-Verizon CDN** (vakio ja Premium) vain olevalla tiedostot on pakattu.  Voitaisiin pakkaus, tiedosto on:
>
> - On suurempi kuin 128 tavua.
> - Olla pienempi kuin 1 Megatavu.
> 
> Kaikki tiedostot ovat oikeuta pakkaamisen **Azure CDN-Akamai**.
>
> Azure CDN tuotteita tiedoston on oltava MIME-tyyppi, joka on [määritetty pakkaus](#enabling-compression).
>
> **Azure CDN Verizon-** profiileista (vakio ja Premium)-tuen **gzip**, **Kovera**tai **bzip2** koodaus.  **Azure CDN Akamai-** profiileista tukevat vain **gzip** koodaus.
>
> **Azure-Akamai CDN** päätepisteet pyytää **gzip** koodattu tiedostot aina origin pyyntö riippumatta.

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Pakkaamisen käytöstä tai tiedosto ei ole oikeutettu pakkaus

|Asiakkaan pyydetty format (joko Hyväksy koodaus ylätunniste)|Välimuistiin tallennetut tiedostomuoto|CDN vastauksen asiakkaalle|Huomautuksia|
|----------------|-----------|------------|-----|
|Pakattu|Pakattu|Pakattu|   |
|Pakattu|Pakkaamattomat|Pakkaamattomat|    | 
|Pakattu|Välimuistiin|Pakattu tai puretaan|Määräytyy origin vastaus|
|Pakkaamattomat|Pakattu|Pakkaamattomat|    |
|Pakkaamattomat|Pakkaamattomat|Pakkaamattomat|    |   
|Pakkaamattomat|Välimuistiin|Pakkaamattomat|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Pakkaaminen käyttöön ja tiedosto on oikeutettu pakkaus

|Asiakkaan pyydetty format (joko Hyväksy koodaus ylätunniste)|Välimuistiin tallennetut tiedostomuotoon|CDN vastauksen asiakkaalle|Huomautuksia|
|----------------|-----------|------------|-----|
|Pakattu|Pakattu|Pakattu|CDN transcodes välillä tuetut tiedostomuodot|
|Pakattu|Pakkaamattomat|Pakattu|CDN suorittaa pakkaus|
|Pakattu|Välimuistiin|Pakattu|CDN suorittaa pakkaus, jos origin palauttaa pakkaamattomat.  **Azure-Verizon CDN** välittää pakkaamattomat tiedoston ensimmäisen pyynnön ja sitten pakkaa ja välimuistiin myöhemmät tiedosto.  Tiedostojen `Cache-Control: no-cache` ylätunniste ei voi pakata. 
|Pakkaamattomat|Pakattu|Pakkaamattomat|CDN suorittaa purku|
|Pakkaamattomat|Pakkaamattomat|Pakkaamattomat|     |  
|Pakkaamattomat|Välimuistiin|Pakkaamattomat|     |

## <a name="media-services-cdn-compression"></a>Media Services-palveluiden CDN pakkaus

CDN Media-palvelut on otettu käyttöön streaming päätepisteet, pakkaus on käytössä oletusarvoisesti seuraavat sisältötyypit: sovelluksen/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, sovelluksen/f4m + xml. Voit ei ota käyttöön/poistaa käytöstä pakkaus maininta koskee tyyppejä Azure-portaalissa.  

## <a name="see-also"></a>Katso myös
- [CDN tiedostojen pakkaamisen vianmääritys](cdn-troubleshoot-compression.md)    
