<properties 
   pageTitle="Web-välityspalvelimen StorSimple laitteen määrittäminen | Microsoft Azure"
   description="Opi käyttämään Windows PowerShell StorSimple StorSimple laitteen web välityspalvelimen asetusten määrittäminen."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-web-proxy-for-your-storsimple-device"></a>Web-välityspalvelimen StorSimple-laitteen määrittäminen

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kuvataan Windows PowerShell StorSimple avulla voit määrittää ja tarkastella web-välityspalvelimen StorSimple laitteeseesi. Web-välityspalvelimen käytetään StorSimple laitteen viestinnässä pilven kautta. Web-välityspalvelimen avulla lisätään suojausta, suodattimen sisältöä välimuistin helpottaminen kaistanleveyden vaatimukset tai jopa analytics apua.

Web-välityspalvelimen on valinnainen määritys StorSimple laitteeseesi. Voit määrittää WWW-välityspalvelimen vain Windows PowerShellin kautta StorSimple. Kokoonpano on kaksivaiheinen prosessi seuraavasti:

1. Web välityspalvelinasetusten ohjatun määritystoiminnon tai Windows PowerShellin kautta StorSimple cmdlet-komennot määrittäminen.

2. Voit ottaa määritetyn web välityspalvelimen asetusten Windows PowerShellin kautta StorSimple cmdlet-komennot.

Web-välityspalvelimen määritys on valmis, kun Microsoft Azure StorSimple hallintapalvelu-ja Windows PowerShellin voit tarkastella määritetyn web-välityspalvelimen StorSimple. 

Kun olet lukenut Tässä opetusohjelmassa, saa oikeuden:

- Määrittää WWW-välityspalvelimen käyttämällä ohjattuun asennukseen ja cmdlet-komennot
- Ottaminen käyttöön web-välityspalvelimen kautta cmdlet-komennot
- Web-välityspalvelimen asetusten tarkasteleminen Azure perinteinen portaalissa
- Web-välityspalvelimen määritysten aikana vianmääritys


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Määrittää WWW-välityspalvelimen kautta Windows PowerShellin StorSimple

Jommankumman seuraavista avulla web-välityspalvelimen asetusten määrittäminen:

- Ohjatun määritystoiminnon opastavat määritysvaiheet.

- Windows PowerShell StorSimple cmdlet-komennoista.

Seuraavissa osissa käsitellään eri tavalla.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Web-välityspalvelimen kautta ohjatun määritystoiminnon määrittäminen

Voit ohjattu määritystoiminto opastaa web välityspalvelimen määritykset vaiheet. Seuraavien toimien määrittäminen web-välityspalvelimen laitteessasi.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Voit määrittää WWW-välityspalvelimen kautta ohjatun määritystoiminnon

1. Sarja console-valikon vaihtoehto 1, **Kirjaudu sisään täysi käyttöoikeus** ja anna **laitteen järjestelmänvalvojan salasanan**. Kirjoita seuraava komento asetukset ohjatun-istunnon aloittaminen:

    `Invoke-HcsSetupWizard`

2. Jos kyseessä on, että olet käyttänyt ohjatun määritystoiminnon laitteen rekisteröinnin ensimmäistä kertaa, sinun on kaikki tarvittavat verkko-asetusten määrittämiseen, kunnes pääset WWW-välityspalvelimen määritykset. Jos laite on jo rekisteröity, voit hyväksyä kaikki määritetyn verkkoasetukset, kunnes WWW-välityspalvelimen määritykset. Ohjatussa asennustoiminnossa web-välityspalvelimen asetusten Kirjoita kehotettaessa **Kyllä**.

3. Määritä IP-osoite tai täydellinen toimialuenimi (FQDN) web-välityspalvelimen ja TCP-porttinumero, joita haluat käyttää viestinnässä pilven kautta laitteen **Web välityspalvelimen URL-osoite**. Käytä seuraava kaava:

    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`

    TCP-porttinumero 8080 on määritetty oletusarvoisesti.

4. Valitse todennustyyppi **NTLM**, **Basic**tai **ei mitään**. Tavallinen on vähiten turvallinen todennus välityspalvelimen asetukset. NT LAN Manager (NTLM) on erittäin turvallinen ja monimutkaisia todennusprotokolla, joka käyttää kolme tapaa sähköpostijärjestelmän (joskus neljä aitouden tarvitaanko) käyttäjä todennetaan. Oletustodennus on NTLM. Lisätietoja on kohdassa [Perus](http://hc.apache.org/httpclient-3.x/authentication.html) - ja [NTLM-todennusta](http://hc.apache.org/httpclient-3.x/authentication.html). 

    > [AZURE.IMPORTANT] **Laitteen seuranta-kaaviot eivät toimi, kun Basic StorSimple hallintapalvelu tai välityspalvelimen kokoonpanoa laitteen NTLM-todennus on käytössä. Seurannan kaavioiden käyttäminen on varmistaa, että todennus on määritetty ei mitään.**

5. Jos käytät todennusta, anna **Web välityspalvelimen käyttäjänimi** ja **Web-välityspalvelimen salasana**. Sinun on myös salasana.

    ![Määrittää WWW-välityspalvelimen StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Jos laitteessa ensimmäistä kertaa, jatka rekisteröinnin. Jos laite on jo rekisteröity, ohjattu toiminto päättyy. Määritetyt asetukset tallennetaan.

Web-välityspalvelimen nyt myös otetaan käyttöön. Voit ohittaa vaiheen [käyttöön web-välityspalvelimen](#enable-web-proxy) ja siirry [Azure perinteinen portaalissa Näytä web välityspalvelimen](#view-web-proxy-settings-in-the-azure-classic-portal).


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Määritä web välityspalvelin Windows PowerShellin kautta StorSimple cmdlet-komennot

Web-välityspalvelimen asetusten määrittäminen vaihtoehtoinen tapa on StorSimple Cmdlet-komentoja Windows PowerShell. Seuraavien toimien web-välityspalvelimen määrittäminen.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Voit määrittää WWW-välityspalvelimen kautta cmdlet-komennot

1. Valitse sarja konsoli-valikon vaihtoehto 1, **Kirjaudu sisään täydet oikeudet**. Anna kehotettaessa **laitteen järjestelmänvalvojan salasanan**. Oletus-salasana on `Password1`.

2. Kirjoita komentoriville seuraava komento:

    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`

    Anna ja vahvista salasana pyydettäessä alla kuvatulla tavalla.

    ![Määrittää WWW-välityspalvelimen StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

Web-välityspalvelimen on nyt määritetty ja on otettava käyttöön.

## <a name="enable-web-proxy"></a>Web-välityspalvelimen ottaminen käyttöön

Web-välityspalvelimen on oletusarvoisesti pois käytöstä. Kun olet määrittänyt WWW-välityspalvelimen StorSimple laitteen, sinun on käytettävä Windows PowerShell StorSimple WWW-välityspalvelimen käyttöön.

> [AZURE.NOTE] **Tämä vaihe ei pakollinen, jos asensit ohjatun määritystoiminnon web-välityspalvelimen asetusten määrittäminen. Web-välityspalvelimen on automaattisesti käytössä oletusarvoisesti, istunnon ohjatun asennuksen jälkeen.**

Suorittamalla Windows PowerShell StorSimple web-välityspalvelimen laitteen käyttöön seuraavasti:

#### <a name="to-enable-web-proxy"></a>Jos haluat ottaa käyttöön web-välityspalvelin

1. Valitse sarja konsoli-valikon vaihtoehto 1, **Kirjaudu sisään täydet oikeudet**. Anna kehotettaessa **laitteen järjestelmänvalvojan salasanan**. Oletus-salasana on `Password1`.

2. Kirjoita komentoriville seuraava komento:

    `Enable-HcsWebProxy`

    Web-välityspalvelimen määritykset on nyt käytössä StorSimple laitteen.

    ![Määrittää WWW-välityspalvelimen StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Web-välityspalvelimen asetusten tarkasteleminen Azure perinteinen portaalissa

Web-välityspalvelimen asetukset on määritetty Windows PowerShell-liittymän kautta, eikä niitä voi muuttaa perinteinen portaalissa. Voit kuitenkin tarkastella nämä asetukset on määritetty perinteinen-portaalissa. Seuraavien toimien tarkastelemaan web-välityspalvelin.

#### <a name="to-view-web-proxy-settings"></a>Jos haluat tarkastella web-välityspalvelimen asetukset
1. Siirry **StorSimple hallinta > laitteet**. Valitse ja valitse haluamasi laite ja siirry sitten **Määritä**.
1. Vieritä alas **Määritä** sivun **välityspalvelimen verkko** -osaan. Voit tarkastella määritetyn web-välityspalvelimen StorSimple laitteen alla kuvatulla tavalla.

    ![Näytä Web-välityspalvelimen hallinta-portaalissa](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)
 
## <a name="errors-during-web-proxy-configuration"></a>Web-välityspalvelimen määritysten aikana virheet

Jos web-välityspalvelimen asetukset on määritetty väärin, virhesanomat näytetään Windows PowerShell-käyttäjä StorSimple. Seuraavassa taulukossa kerrotaan joistakin virheilmoituksia, niiden mahdollinen syitä ja toimia.

|Sarja ei.|HRESULT virhekoodi|Mahdollisista syy|Suositellut toimet|
|:---|:---|:---|:---|
|1.|0x80070001|Komento suoritetaan lähettämät passiivinen ja ei ole enää aktiivinen ohjaimella viestiä.|Suorita-komento aktiivinen ohjaimen. Voit suorittaa komennon lähettämät passiivinen, sinun on korjaa yhteydet-passiivinen aktiivinen valvojalle. Tarvitset Microsoft Support tehdä, jos tämä on katkennut.|
|2.|0x800710dd - toiminnon tunniste ei kelpaa|Välityspalvelimen asetuksia ei tueta StorSimple virtual laitteessa.|Välityspalvelimen asetuksia ei tueta StorSimple virtual laitteessa. Nämä voidaan määrittää vain StorSimple fyysinen laitteessa.|
|3.|0x80070057 - parametri ei kelpaa|Jokin säädetty välityspalvelimen asetuksia ei ole kelvollinen.|URI ei ole annettu oikeassa muodossa. Käytä seuraava kaava:`http://<IP address or FQDN of the web proxy server>:<TCP port number>`|
|4.|0x800706ba - RPC palvelin ei ole käytettävissä|Pääkansio syynä on jokin seuraavista:</br></br>Klusteria ei ole ylöspäin.</br></br>Tietopolkua palvelu ei ole käynnissä.</br></br>Komento suoritetaan lähettämät passiivinen ja ei ole enää aktiivinen ohjaimella viestiä.|Ota osallistuminen Microsoft Support varmistaaksesi, että klusterin on käytössä ja Tietopolkua-palvelu on käynnissä.</br></br>Suorita-komento lähettämät aktiivinen. Jos haluat suorittaa komennon passiivinen ohjauskoneen, sinun on varmistettava passiivinen ohjauskoneen viestiä aktiivinen ohjaimella. Tarvitset Microsoft Support tehdä, jos tämä on katkennut.|
|5.|0x800706be - etäproseduurikutsu epäonnistui|Klusteria ei ole käytettävissä.|Ota osallistuminen Microsoft Support varmistaa, että klusterin ylöspäin.|
|6.|0x8007138f - Klusteriresurssia ei löydy|Ympäristön palvelun klusteriresurssin ei löydy. Näin voi käydä, jos asennus ei ole oikein.|Joudut ehkä suorittamaan factory, Palauta laitteessasi. Joudut ehkä luoda ympäristö resurssin. Ota yhteyttä Microsoft Support vaiheisiin.|
|7.|0x8007138c - klusteriresurssi ei online-tilaan|Ympäristön tai Tietopolkua klusteriresurssit eivät ole online-tilassa.|Ota yhteyttä Microsoftin Support varmistaa, että Tietopolkua ja ympäristö palveluresurssin ovat online-tilassa.|

> [AZURE.NOTE] 
> 
> -  Virhesanomat yllä luettelo ei ole täydellinen. 
> - Web-välityspalvelimen liittyviä virheitä ei näytetä StorSimple hallintapalvelu Azure perinteinen Portalissa. Jos näkyvissä on web-välityspalvelimen ongelmasta, kun määritykset on tehty, laitteen tila muuttuu **offline-tila** perinteinen-portaalissa. |

## <a name="next-steps"></a>Seuraavat vaiheet

- Jos kohtaat ongelmia käyttöönotto laitteen tai web-välityspalvelimen asetusten, viitata [vianmääritys StorSimple laitteen käyttöönoton](storsimple-troubleshoot-deployment.md).

- Opettele käyttämään StorSimple hallintapalveluun, siirry [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
