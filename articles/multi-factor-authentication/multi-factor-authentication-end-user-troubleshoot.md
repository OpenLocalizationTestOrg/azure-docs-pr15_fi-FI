<properties
    pageTitle="Kaksivaiheinen vahvistus on vianmääritys | Microsoft Azure"
    description="Tämän asiakirjan antaa käyttäjien tietoja, jos ne Azure Monimenetelmäisen todentamisen ongelma voi ilmetä."
    services="multi-factor-authentication"
    keywords = "monimenetelmäisen todentamisen asiakas-todennus ongelma Korrelaatiotunnus"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="having-trouble-with-two-step-verification"></a>Tarvitsetko lisäneuvoja kaksivaiheista vahvistusta varten

Tässä artikkelissa käsitellään joitakin ongelmia, joita voit kohdata kaksivaiheista vahvistusta varten. Jos ongelma ilmenee ei sisällytetä tähän, anna yksityiskohtaiset palautteen kommentit-kohtaan niin, että voit parantamiseen.

## <a name="i-lost-my-phone-or-it-was-stolen"></a>Olen kadottanut puhelimella tai se on varastamiselta

Voit siirtyä takaisin tilisi kahdella eri tavalla. Ensimmäinen on Kirjaudu sisään käyttämällä vaihtoehtoisia käyttöoikeuden puhelinnumero, jos olet määrittänyt yhden. Toinen on Kysy järjestelmänvalvojaltasi Poista asetukset.

Jos puhelimesi katkesi tai varastamiselta, on suositeltavaa myös sinulla ole järjestelmänvalvoja, Palauta sovelluksen salasanat ja poista mitään mitalilla laitteet. Jos järjestelmänvalvoja ei ole varma siitä, miten voit tehdä tämän, osoita ne tässä artikkelissa: [käyttäjien hallinta ja laitteet](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords).


### <a name="use-an-alternate-phone-number"></a>Vaihtoehtoinen puhelinnumero käyttäminen

Jos olet määrittänyt useita vahvistus-asetuksia, kuten toissijainen puhelinnumero tai todentaja sovelluksen jonkin toisen laitteen, voit jokin näistä kirjautuminen.

Voit kirjautua sisään käyttämällä vaihtoehtoinen puhelinnumero, toimimalla seuraavasti:

1. Kirjaudu sisään tavalliseen tapaan.
2. Valitse **Käytä vaihtoehtoisen tarkistusmenetelmän**tilin vahvistaminen edelleen pyydettäessä.

    ![Eri todentaminen](./media/multi-factor-authentication-end-user-manage/differentverification.png)

3. Valitse puhelinnumero, joihin sinulla on käyttöoikeus.

    ![Vaihtoehtoinen puhelinnumero](./media/multi-factor-authentication-end-user-manage/altphone2.png)

4. Kun olet takaisin sisään tilin [hallinta-asetusten](multi-factor-authentication-end-user-manage-settings.md) muuttamiseen todennus puhelinnumerosi.

>[AZURE.IMPORTANT]
>On tärkeää määrittää toissijaisen todennus puhelinnumero. Jos ensisijainen puhelinnumero ja mobile-sovellus on sama puhelin, sinun on annetaan kolmas vaihtoehto Jos puhelimen katoaa tai varastamiselta.

### <a name="clear-your-settings"></a>Poista asetukset

Jos et ole määrittänyt toissijaisen todennusta, puhelinnumero ja valitse pyydä apua järjestelmänvalvojalta on. Pyydettävä häntä Poista asetukset, jotta seuraavan kerran kirjaudut sisään, sinua pyydetään [tilin](multi-factor-authentication-end-user-first-time.md) määrittäminen uudelleen.


## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Minulla ei vastaanoteta tekstiä tai soittaa puhelimella

On useita syitä, miksi saattaa yrittää kirjautua sisään, mutta eivät saa tekstin tai puhelu. Jos olet onnistuneesti saanut tekstit tai puhelut aiemman puhelimen, valitse syynä on luultavasti ongelma puhelin-palvelussa ei ole tiliä. Varmista, että sinulla on hyvä solun signaalin ja jos, jonka haluat vastaanottaa tekstiviesti Varmista, että puhelin- ja palvelupakettisi tukevat tekstiviestejä.

Jos olet odotti useamman minuutin ennen tekstiä tai puhelun, nopein tapa tuoda tilisi on Kokeile eri vaihtoehtoa.

1. Valitse **Käytä vaihtoehtoisen tarkistusmenetelmän** sivulla, jolla odottaa tarkistuksen.

    ![Eri todentaminen](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

2. Valitse puhelin luku- tai vastaanottokuittausta menetelmä, jota haluat käyttää.

    Jos olet saanut useita vahvistus koodit, uusin yksi toimii.

Jos sinulla ei ole määritetty toista menetelmää, ota yhteyttä järjestelmänvalvojaan ja pyydä heitä Poista asetukset. Kun seuraavan kerran, kun kirjaudut sisään, voit pyydetään [monimenetelmäisen todentamisen](multi-factor-authentication-end-user-first-time.md) määrittäminen uudelleen.


Jos sinulla on usein viiveitä virheelliset solun signaalin vuoksi, suosittelemme [Microsoft todentaja-sovelluksen](multi-factor-authentication-microsoft-authenticator.md) käyttäminen että smartphone. Sovellus voi luoda satunnaisia suojaus-koodit, jolla kirjaudut ja koodit ei edellytä mitään solun signaalin tai internet-yhteys.


## <a name="app-passwords-are-not-working"></a>Sovelluksen salasanat eivät toimi

Varmista ensin, että ohjelman salasana on kirjoitettu oikein.  Jos se ei edelleenkään onnistu, kokeile kirjautua sisään ja [Luo uusi sovelluksen salasana](multi-factor-authentication-end-user-app-passwords.md).  Jos tämä ei toimi, ota yhteys järjestelmänvalvojaan ja pyydettävä häntä [poistaa aiemmin ohjelmasalasanat](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) ja sen jälkeen voit luoda uuden.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Vastauksen koskevaan ongelmaan ei löydy.

Jos olet kokeillut näitä vianmääritystoimia, mutta ovat edelleen käytössä ilmenee ongelmia, ota yhteyttä järjestelmänvalvojaan tai henkilö, joka määrittää monimenetelmäisen todentamisen puolestasi. Hän olisi voi auttaa sinua.

Lisäksi voit lähettää kysymyksen [Azure AD-keskustelupalstat](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) tai [Ota yhteyttä tukeen](https://support.microsoft.com/contactus) ja olemme vastata ongelmasi heti, kun emme voi.

Jos otat yhteyttä tukeen, Sisällytä seuraavat tiedot:

- **Käyttäjätunnus** – mikä on sähköpostiosoite, olet yrittänyt kirjautua sisään?
- **Yleinen virhe kuvaus** – mitä tarkka virhesanoma näkyy voit nähdä?  Jos oli virhesanomaa, kuvaavat odottamatonta toimintaa huomannut, yksityiskohtaiset tiedot.
- **Sivun** – sivun käytitkö, kun näyttöön tuli virheen (sisältää URL-osoite)?
- **Virhekoodi** - virhekoodi tulee perille.
- **Istunto** - vastaanotat tietyn istunnon tunnus.
- **Korrelaatiotunnuksen** – mikä oli luodaan, kun käyttäjä tuli virheen korrelaatio-tunnuskoodi.
- **Aikaleima** – mikä oli tarkan päivämäärän ja kellonajan näyttöön tuli virheen (sisältää aikavyöhyke)?

Monet näistä löytyy Kirjaudu sisään-sivulla. Kun tarkistat ei oman Kirjautumisvirheitä aiheuttavien samanaikaisesti, valitse **Näytä tiedot**.

![Kirjaudu sisään virheen yksityiskohtaiset tiedot](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Tietojen auttaa meitä Ratkaise ongelma mahdollisimman pian.

## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita
- [Kaksivaiheinen vahvistus-asetusten hallinta](multi-factor-authentication-end-user-manage-settings.md)  
- [Microsoft Authenticator sovelluksen usein kysytyt kysymykset](multi-factor-authentication-app-faq.md)
