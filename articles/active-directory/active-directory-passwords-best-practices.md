<properties
    pageTitle="Parhaat käytännöt: Azure AD-salasanojen hallinta | Microsoft Azure"
    description="Käyttöönoton ja käytön parhaat käytännöt otoksen ohjeet ja koulutus apuviivat Azure Active Directoryn salasanojen hallinta."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="deploying-password-management-and-training-users-to-use-it"></a>Salasanojen hallinta ja koulutus käyttäjiä käyttämään sitä käyttöönotto

> [AZURE.IMPORTANT] **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).

Kun ottaminen käyttöön salasanan, sinun on suoritettava seuraava vaihe on-palvelun avulla organisaatiosi käyttäjiä. Tällöin sinun on Varmista, että käyttäjien on määritetty käyttämään palvelua oikein ja myös, että käyttäjillä on tarvitsemansa onnistuu hallintaa salasanansa koulutusta. Tässä artikkelissa selitetään sinulle seuraavia käsitteitä:

* [**Hankkiminen käyttäjillesi määritetty salasanojen hallinta**](#how-to-get-users-configured-for-password-reset)
  * [Mikä on määritetty salasanan tili](#what-makes-an-account-configured)
  * [Kuinka voit täyttää todennus](#ways-to-populate-authentication-data)
* [**Parhaita tapoja siirtää organisaation salasanan**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [Sähköpostipohjainen rahoittaja + otoksen sähköpostin viestintä](#email-based-rollout)
  * [Luo oma mukautettu salasanan hallinta-portaalin käyttäjille](#creating-your-own-password-portal)
  * [Opit käyttämään pakotettu rekisteröinti käyttäjien rekisteröidä kirjautumiseen](#using-enforced-registration)
  * [Käyttäjätilien todentamistiedot lataaminen](#uploading-data-yourself)
* [**Esimerkki käyttäjän ja tuen koulutusmateriaalin (tulossa!)**](#sample-training-materials)

## <a name="how-to-get-users-configured-for-password-reset"></a>Miten käyttäjät määritetty salasanan palauttaminen
Tässä osassa kuvataan sinulle eri tavat, jolla voit varmistaa, että kaikki organisaation käyttäjät voivat käyttää Omatoiminen salasanan tehokkaasti siltä varalta, että ne unohtanut salasanansa.

### <a name="what-makes-an-account-configured"></a>Mikä on määritetty tili
Käyttäjä voi käyttää salasanan, **kaikki** seuraavat ehdot täyttyvät:

1.  Salasanan on otettava käyttöön hakemistossa.  Lue, miten voit ottaa käyttöön salasanan lukemalla [käyttäjät voivat palauttaa Azure AD-salasanansa](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) tai [antaa käyttäjien AD salasanansa tai palauttaminen](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
2.  Käyttäjän täytyy olla käyttöoikeus.
 - Cloud käyttäjille käyttäjällä on oltava **kaikki maksettua Office 365-käyttöoikeus**- tai **AAD Basic** tai määritetty **AAD Premium-käyttöoikeus** .
 - Paikallinen (liitetty tai hash synkronoitu)-käyttäjien käyttäjällä **on oltava määritetty AAD Premium-käyttöoikeutta**.
3.  Käyttäjän on oltava **määritetty todentamistiedot vähimmäismäärä** nykyinen salasana Salasanakäytäntö mukaisesti.
 - Todentamistiedot pidetään määritetty jos hakemiston vastaavaan kenttään on muotoiltu tietoja.
 - Todentamistiedot vähimmäismäärä on määritetty **vähintään yhteen** käytössä todennusasetukset Jos yksi portti käytäntö on määritetty, tai **vähintään kahden** käytössä todennusasetukset, jos kaksi portti käytäntö on määritetty.
4.  Jos käyttäjä käyttää paikallisen tilin, valitse [Salasana takaisinkirjoituksen](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) on otettu käyttöön ja otettu käyttöön

### <a name="ways-to-populate-authentication-data"></a>Täytä todentamistiedot tavalla
Sinulla on useita vaihtoehtoja siitä, miten voit määrittää organisaation käyttäjien tietoja käytettävän salasanan.

- Käyttäjien [Hallinta-portaalin Azure](https://manage.windowsazure.com) -tai [Office 365-hallintaportaalissa](https://portal.microsoftonline.com) muokkaaminen
- Synkronoi käyttäjän ominaisuuksia suoraan Azure AD-paikallisen Active Directory-toimialueen Azure AD-synkronointi avulla
- Windows PowerShellin avulla voit muokata käyttäjän ominaisuuksia [seuraavien ohjeiden](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users)mukaan.
- Käyttäjät voivat rekisteröidä omat tietonsa luotsaa rekisteröinti-portaaliin osoitteessa [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
- Käyttäjien salasanan, kun synkronointi niiden Azure AD-tilille määrittämällä Rekisteröi [**edellyttää, että käyttäjät rekisteröityä, kun kirjaudun sisään?**](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) määritysvaihtoehto **Kyllä**.

Käyttäjien ei tarvitse rekisteröidä järjestelmän toimimaan salasanan.  Esimerkiksi jos aiemmin Matkapuhelin- tai office numerot ovat paikallisen kansion, voit synkronoida ne Azure AD- ja Käytämme ne salasanan automaattisesti.

Voit myös lukea lisätietoja [siitä, miten tietoja käytetään salasanan palauttaminen](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) ja [miten voit lisätä yksittäisten käyttöoikeuksien kentät PowerShellin avulla](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users).

## <a name="what-is-the-best-way-to-roll-out-password-reset-for-users"></a>Mikä on paras tapa aloittamaan käyttäjien salasanan?
Salasanan Yleiset rahoittaja vaiheet ovat seuraavat:

1.  Ottaa käyttöön salasanan hakemistossa siirtymällä [Azure hallinta-portaalin](https://manage.windowsazure.com) **määrittäminen** -välilehteen ja valitsemalla **Kyllä** **Käyttäjien käyttöön salasanan** -asetus.
2.  Käyttöoikeuksien tarvittavat kunkin käyttäjän, jolle haluat tarjota salasanan siirtymällä [Azure hallinta-portaalin](https://manage.windowsazure.com) **käyttöoikeudet** -välilehti.
3.  Voit myös rajoittaa salasanan ryhmälle käyttäjiä aloittamaan ominaisuus hitaasti ajan kuluessa määrittämällä **Salasanan rajoita käyttöoikeus** -sivulla **Kyllä** ja valitsemalla Suojaus-ryhmä, johon haluat ottaa käyttöön salasanan (Huomautus nämä käyttäjät on kaikki on määritetty käyttöoikeuksia).
4.  Opasta käyttäjiä käyttämään salasanan lähettämällä joko neuvot heitä rekisteröimään, sähköpostin ottaminen käyttöön pakotettu rekisteröinti access-paneelin tai lataamalla haluamasi todentamistiedot kyseisille käyttäjille itse Dirsync-työkalun, PowerShell tai [Azure hallinta-portaalin](https://manage.windowsazure.com)kautta.  Lisätietoja tästä ovat jäljempänä.
5.  Ajan kuluessa voit tarkastella käyttäjien rekisteröinti siirtyminen raportit-välilehti ja tarkastelemalla [**Salasanan palauttaminen rekisteröinti toimintojen**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) -raportti.
6.  Kun hyvä käyttäjämäärä olet rekisteröinyt, katsella niitä siirtyminen raportit-välilehti ja tarkastelemalla [**salasanan palauttaminen**](active-directory-passwords-get-insights.md#view-password-reset-activity) -käyttöraportti salasanan avulla.

Ilmoittaa käyttäjille, että Rekisteröidy ja salasanan organisaatiosi käyttää useilla tavoilla.  Ne on kuvattu alla.

### <a name="email-based-rollout"></a>Sähköpostipohjainen käyttöönoton helpottaminen
Helpoin tapa ilmoittaa käyttäjille tietoja Rekisteröidy tai käyttää salasanan palauttaminen on ehkä lähettämällä sähköpostiviestin neuvot niitä tarvittaessa.  Alla on mallin avulla voit tehdä tämän.  Vapaasti korvaa värit / verrattuna kaavaan oman logon valitseminen Voit mukauttaa sen tarpeisiisi sopivaksi.

  ![][001]

Voit [ladata sähköpostimallin tähän](http://1drv.ms/1xWFtQM).

### <a name="creating-your-own-password-portal"></a>Oman salasanan portal luominen
Yksi vaihtoehto, joka toimii hyvin suurten asiakkaiden käyttöönotto salasanan hallintatoiminnot Luo yksittäinen "salasana portal", joka käyttäjien avulla voit hallita kaikkia kohteita, jotka liittyvät salasanansa yhteen paikkaan.  

Useita suurin asiakkaita luo pääkansion DNS-tietoja, esimerkiksi https://passwords.contoso.com sekä linkit Azure AD-salasanan palauttaminen portal, salasanan rekisteröinti portaaliin ja salasanan muuttaminen sivut valitsemalla.  Tällä tavalla sähköpostin viestinnän tai fliers viesteihin, voit lisätä yhdellä asiat selkeästi, käyttäjät voivat avata, kun he suorittavat toisen aloittaa palvelun URL-osoite.

Käyttäminen tässä, emme olet luonut yksinkertaista sivun, joka käyttää uusin vastaa Käyttöliittymän rakenne tuotantoparadigmojen ja toimii kaikissa selaimissa ja mobiililaitteisiin.

  ![][007]

Voit [ladata tähän sivustoon mallin](https://github.com/kenhoff/password-reset-page).  On suositeltavaa mukauttaminen logon ja värien organisaatiosi on.

### <a name="using-enforced-registration"></a>Käyttämällä pakotettu rekisteröinti
Jos haluat käyttäjien rekisteröidä salasanan itse, voit myös tarkastaa ne Rekisteröidy silloin, kun ne Kirjaudu sisään osoitteessa [http://myapps.microsoft.com](http://myapps.microsoft.com)access-paneeli.  Voit ottaa tämä asetus, jos oman kansion **määrittäminen** -välilehdestä ottamalla **Rekisteröi Access-paneelin kirjautumisessa edellyttävät käyttäjät** -vaihtoehto.  

Voit myös määrittää, onko ne pyydetään voit rekisteröidä uudelleen muokkaamalla **päivinä, ennen kuin käyttäjien on vahvistettava sen yhteyshenkilön tiedot** -asetus on jokin muu kuin nolla-arvo määritettävä ajan kuluttua. Lisätietoja on kohdassa [Mukauttaminen käyttäjän salasanan hallinta toiminnan](active-directory-passwords-customize.md#password-management-behavior) .

  ![][002]

Kun otat tämän asetuksen, kun access-paneelin kirjautuminen käyttäjiä, he näkevät valikko, joka ilmoittaa, että järjestelmänvalvoja on täytyttävä ne vahvistamiseksi yhteystietonsa. He voivat käyttää sitä, hänen salasanansa on vaihdettu, jos ne kadotat access tilissä.

  ![][003]

Valitsemalla **Tarkista nyt** tuo ne **salasanan rekisteröinti-portaalissa** osoitteessa [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) ja edellyttää heitä rekisteröimään.  Rekisteröinnin kautta tällä menetelmällä voit hylätty **Peruuta** -painiketta tai sulkemalla ikkunan, mutta käyttäjät ovat muistutuksen aina, kun ne kirjautua sisään, jos ne eivät rekisteröidy.

  ![][004]

### <a name="uploading-data-yourself"></a>Tietojen lataaminen itse
Jos haluat ladata todentamistiedot, valitse käyttäjien ei tarvitse rekisteröidä salasanan, ennen kuin siitä nollata salasanoja.  Kun käyttäjillä todentamistiedot määritetty tilin, joka täyttää salasanan palauttaminen käytäntö on määritetty ja valitse nämä käyttäjät voivat nollata salasanoja.

Mitä ominaisuuksia, voit määrittää yhteyden AAD tai Windows PowerShell-kohdassa [tietoja käytetään salasanan](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

Voit ladata todentamistiedot [Azure hallinta-portaalin](https://manage.windowsazure.com) kautta noudattamalla seuraavia ohjeita:

1.  Siirry [Azure hallinta-portaalin](https://manage.windowsazure.com) **Active Directory-tunniste** hakemistossa.
2.  Valitse **käyttäjät** -välilehdessä.
3.  Valitse käyttäjä, olet kiinnostunut luettelosta.
4.  Ensimmäinen-välilehdessä etsii **Vaihtoehtoiseen**, joita voidaan käyttää ominaisuuden käyttöön salasanan.

    ![][005]

5.  Valitse **Määritys** -välilehti.
6.  Tällä sivulla etsii **Toimiston puhelinnumero**, **Matkapuhelin**, **Todennus puhelin**ja **Todennus sähköpostin**.  Nämä ominaisuudet voi määrittää myös antaa käyttäjän yhteystietoluetteloonsa salasanan.

    ![][006]

Katso, miten kukin ominaisuuksia voi käyttää [tietoja käytetään salasanan](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) .

Artikkelissa kerrotaan, miten voit lukea ja määrittää nämä tiedot PowerShellin [käyttäminen salasanan palauttaminen PowerShell käyttäjät tiedot](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) .

## <a name="sample-training-materials"></a>Esimerkki koulutusmateriaali
Yritämme otoksen koulutusmateriaalin, joiden avulla voit nopeasti IT-organisaation ja käyttäjien käytön käyttöönotto ja salasanan avulla.  Tulossa!


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Linkkejä salasanan palauttaminen dokumentaatio
Alla on linkkejä kaikkiin Azure AD-salasanan ohjeissa:

* **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).
* [**Toiminta**](active-directory-passwords-how-it-works.md) - palveluun ja kuusi eri osat tietoja kunkin onko
* [**Käytön aloittaminen**](active-directory-passwords-getting-started.md) - tietoja, jotta käyttäjät voivat palauttaa ja muuttaa cloud tai paikalliseen salasanansa
* [**Mukauta**](active-directory-passwords-customize.md) - Opi mukauttamaan ulkoasun ja ilmeen ja palvelun organisaation tarpeisiin toiminta
* [**Hae tiedot**](active-directory-passwords-get-insights.md) - tietoja Microsoftin integroitu raportointiominaisuudet
* [**Usein kysytyt kysymykset**](active-directory-passwords-faq.md) - vastauksia usein kysyttyihin kysymyksiin
* [**Vianmääritys**](active-directory-passwords-troubleshoot.md) – Opi palvelun nopeasti liittyviä ongelmia
* [**Lisätietoja**](active-directory-passwords-learn-more.md) – Valitse syvä kyselyjä teknisiä tietoja palvelun toiminta



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"
