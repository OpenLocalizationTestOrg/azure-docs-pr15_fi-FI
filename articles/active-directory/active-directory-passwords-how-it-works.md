<properties
    pageTitle="Toiminta: Azure AD-salasanojen hallinta | Microsoft Azure"
    description="Lue lisää eri osat Azure AD salasanojen hallinta, mukaan lukien kohtaa, johon käyttäjät rekisteröityä, palauttaminen ja niiden salasanojen ja järjestelmänvalvojien määrittäminen, raportoinnissa ja ota käyttöön paikallisen Active Directory-salasanojen hallinta."
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

# <a name="how-password-management-works"></a>Salasanojen hallinta toiminta

> [AZURE.IMPORTANT] **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).

Salasanojen hallinta Azure Active Directoryn koostuu looginen osat, jotka on kuvattu alla.  Valitse kunkin linkin Lisätietoja osaa.

- [**Salasanan hallinta määritys-portaalin**](#password-management-configuration-portal) – järjestelmänvalvojat voivat hallita, miten salasanoja hallitaan niiden omistajien siirtymällä niiden kansio määrittäminen-välilehdessä [Azure hallinta-portaalin](https://manage.windowsazure.com)eri rakenteita.
- [**Käyttäjän rekisteröinti Portal**](#user-registration-portal) – käyttäjät voivat rekisteröityä salasanan tämän web-portaalin kautta.
- [**Käyttäjän salasanan palauttaminen Portal**](#user-password-reset-portal) – käyttäjät voivat vaihtaa oman salasanat useilla eri haasteita järjestelmänvalvojan salasanan palauttaminen käytännön mukaisesti
- [**Käyttäjän salasanan muuttaminen Portal**](#user-password-change-portal) – käyttäjät voivat vaihtaa oman salasanansa milloin tahansa niiden vanha salasana syöttämällä ja valitsemalla Uusi salasana web-portaalissa
- [**Salasanan hallintaraporttien**](#password-management-reports) – järjestelmänvalvojat voivat tarkastella ja analysoida salasanan palauttaminen ja rekisteröintiä tehtävän käyttäjän vuokraajan siirtymällä [Azure hallinta-portaalin](https://manage.windowsazure.com) niiden hakemisto "Raportit-välilehden"Käyttöraporttien"-osa
- [**Salasanan takaisinkirjoituksen osa Azure AD Connect**](#password-writeback-component-of-azure-ad-connect) – järjestelmänvalvojat voit myös ottaa salasana takaisinkirjoituksen-ominaisuutta, kun asentaminen Azure AD Connect käyttöön hallinnointi liitetty tai salasana synkronoida käyttäjien salasanat pilvestä.

## <a name="password-management-configuration-portal"></a>Salasanan hallinta määritysten Portal
Voit määrittää salasanan hallintakäytäntöjen [Azure hallinta-portaalin](https://manage.windowsazure.com) käyttäminen siirtymällä kansion **määrittäminen** välilehti **Käyttäjän salasanan palauttaminen käytäntöä** kohtaan määritettyyn hakemistoon.  Määritys-sivulla voit hallita monia miten salasanoja hallitaan organisaatiossa, mukaan lukien:

- Ottaminen käyttöön ja poistaminen käytöstä salasanan hakemistossa kaikille käyttäjille
- Haasteita lukumäärän valitseminen (yksi tai kaksi) käyttäjän täytyy kerrotaan yhteystietoluetteloonsa salasanan
- Haluat ottaa käyttöön käyttäjien organisaation haluttu haasteita tietyntyyppiset määrittäminen:
 - Matkapuhelin (vahvistus koodi tekstiä tai äänipuhelu)
 - Toimiston puhelinnumero (äänipuhelu)
 - Vaihtoehtoiseen (vahvistus koodi sähköpostitse)
 - Tietoturvaan liittyvät kysymykset (älykkäät todennus)
- Kysymyksiä lukumäärän valitseminen käyttäjä on rekisteröitävä voi käyttää suojauksen kysymyksiä todennusmenetelmän (jotka vain, jos tietoturvaan liittyvät kysymykset ovat käytössä)
- Kysymyksiä lukumäärän valitseminen käyttäjä on annettava aikana Palauta käyttämään suojauksen kysymyksiä todennusmenetelmän (jotka vain, jos tietoturvaan liittyvät kysymykset ovat käytössä)
- Käyttämällä valmiiksi purkitettu ja lokalisoidut-suojausta kysymyksiin, joita käyttäjä voi valita, haluatko käyttää kun rekisteröidään salasanan palauttaminen (jotka vain, jos tietoturvaan liittyvät kysymykset ovat käytössä)
- Määrittäminen muokatut kysymyksiin, joita käyttäjä voi halutessaan kun rekisteröidään käytettävän salasanan palauttaminen (jotka vain, jos tietoturvaan liittyvät kysymykset ovat käytössä)
- Pakollista käyttäjien rekisteröidä salasanan mennessään [http://myapps.microsoft.com](http://myapps.microsoft.com)kohdistuksen Access-paneeli.
- Vahvista uudelleen aiemmin rekisteröidyn tietonsa jälkeen määritettäviä päivinä edellyttävät käyttäjät on kulunut (näkyvissä vain, jos pakotettu rekisteröinti käytössä)
- Mukautettu tukipalvelu sähköpostin tai URL-Osoitetta, joka näkyy käyttäjille siltä varalta, että heillä on ongelmia niiden salasanojen
- Käyttöönoton tai käytöstäpoiston salasanan takaisinkirjoituksen ominaisuuksien (kun salasana takaisinkirjoituksen on otettu käyttöön AAD yhdistämistä käyttämällä)
- Salasanan takaisinkirjoituksen agentti tilan tarkasteleminen (kun salasana takaisinkirjoituksen on otettu käyttöön AAD yhdistämistä käyttämällä)
- Sähköposti-ilmoitusten ottaminen käyttöön käyttäjille, kun oman salasana on vaihdettu (löytyy [Azure hallinta-portaalin](https://manage.windowsazure.com) **ilmoitukset** -osiossa)
- Sähköposti-ilmoitusten ottaminen käyttöön järjestelmänvalvojille, kun muiden järjestelmänvalvojien salasanojen omia (löytyy [Azure hallinta-portaalin](https://manage.windowsazure.com) **ilmoitukset** -osiossa
- Portaalin mukauttaminen käyttäjän salasanan palauttaminen ja salasanan sähköpostit organisaation logo ja nimi vuokraajan mukauttaminen (löytyy [Azure hallinta-portaalin](https://manage.windowsazure.com) **Kansion ominaisuudet** -osan mukauttaminen-toiminnon avulla

Lisätietoja salasanojen hallinta organisaatiossa määrittämisestä on artikkelissa [käytön aloittaminen: Azure AD salasanojen hallinta](active-directory-passwords-getting-started.md).

##<a name="user-registration-portal"></a>Käyttäjän rekisteröinti Portal
Käyttäjät voivat käyttää salasanan, ennen niiden cloud käyttäjätilit on päivitettävä varmistaaksesi, että ne kulkevat salasanan palauttaminen haasteisiin niiden järjestelmänvalvojan määrittämiä haluamasi määrä oikeita tietoja.  Järjestelmänvalvojat voivat myös määrittää tämän todennustiedot niiden käyttäjien puolesta Azure- tai Office web portaaleihin, Dirsync-työkalun avulla / Azure AD Connect tai Windows PowerShell.

Jos käyttäjien Rekisteröi Omat tiedot, myös annamme web-sivun käyttäjät voivat avata sen tiedot.  Tämän sivun avulla käyttäjät voivat määrittää todennustiedot mukaisesti salasanan palauttaminen käytäntöjä, jotka on otettu käyttöön organisaatiossa.  Kun tiedot on tarkistettu, se tallennetaan pilveen käyttäjätili käytettävän tilin palautus voi tehdä myöhemmin. Näin mitä rekisteröinti portaalin näyttää seuraavalta:

  ![][001]

Lisätietoja on artikkelissa [käytön aloittaminen: Azure AD salasanojen hallinta](active-directory-passwords-getting-started.md) ja [parhaat käytännöt: Azure AD salasanojen hallinta](active-directory-passwords-best-practices.md).

##<a name="user-password-reset-portal"></a>Käyttäjän salasanan palauttaminen Portal
Kun olet ottanut käyttöön käyttäjien Omatoiminen salasanan palauttaminen, organisaation käyttäjien Omatoiminen salasanan palauttaminen käytännön määrittäminen ja varmistetaan, että käyttäjillä on tarvittavat yhteyshenkilön tiedot hakemistossa, organisaation käyttäjät voivat oman järjestelmänvalvojan salasanan automaattisesti millä tahansa web-sivulla, joka käyttää työpaikan tai oppilaitoksen tilille kirjauduttaessa (kuten [portal.microsoftonline.com](https://portal.microsoftonline.com)). Esimerkiksi seuraavia sivuilla, käyttäjät näkevät **Eikö kirjautuminen onnistu?** linkki.

  ![][002]

Napsauttamalla tätä linkkiä Käynnistä käyttäjien Omatoiminen salasanan palauttaminen-portaalissa.

  ![][003]

Lisätietoja siitä, miten käyttäjät voivat vaihtaa oman salasanansa on artikkelissa [käytön aloittaminen: Azure AD salasanojen hallinta](active-directory-passwords-getting-started.md).

##<a name="user-password-change-portal"></a>Käyttäjän salasanan muuttaminen Portal
Jos käyttäjien muuttaa omia salasanoja, käyttäjä voi tehdä niin käyttämällä salasanan muuttaminen portaalin milloin tahansa.  Käyttäjät voivat käyttää salasanan muuttaminen portaalin Access-paneelin profiili-sivulla tai napsauttamalla "Vaihda salasana-linkin Office 365-sovelluksissa.  Tapauksessa salasanansa voimassaolo päättyy, kun käyttäjät myös pyydetään muuttaa niitä automaattisesti, kun kirjaudun sisään.

  ![][004]

Sekä tällaisissa tapauksissa Jos salasana takaisinkirjoituksen on otettu käyttöön ja käyttäjän joko liitetty tai salasanan synkronointi olisi nämä muutetut salasanat kirjoitetaan takaisin paikallisen Active Directory. Näin mitä salasanan muuttaminen portaalin näyttää seuraavalta:

  ![][005]

Lisätietoja siitä, miten käyttäjät voivat vaihtaa oman paikallisen Active Directory-salasanat-kohdassa [käytön aloittaminen: Azure AD salasanojen hallinta](active-directory-passwords-getting-started.md).

##<a name="password-management-reports"></a>Salasanojen hallinta-raportit
Siirtyminen **Raportit** -välilehti ja esittää **Lokeja** -osassa, näet kaksi salasanojen hallinta raporttia: **salasanan tehtävän** ja **salasanan rekisteröinti tehtävän**.  Käytä nämä kaksi raporttia, voit avata näkymän käyttäjien rekisteröiminen ja organisaation salasanan avulla. Näin raportit julkaistaan millaiseksi [Azure hallinta-portaalin](https://manage.windowsazure.com):

  ![][006]

Lisätietoja on artikkelissa [Hae tiedot: Azure AD salasanojen hallintaraporttien](active-directory-passwords-get-insights.md).

##<a name="password-writeback-component-of-azure-ad-connect"></a>Azure AD Connect salasanan takaisinkirjoituksen osa
Jos organisaation käyttäjien salasanat ovat peräisin paikallisen ympäristön (joko federation tai salasanojen synkronoinnin), voit asentaa uusimman version Azure AD Connect käyttöön päivittäminen suoraan pilveen salasanat.  Tämä tarkoittaa, että käyttäjien unohda tai muokattavaa AD salasanan, käyttäjä voi tehdä niin suoraan Internetistä.  Näin löydät salasanan takaisinkirjoituksen Azure AD Connect ohjatun asennuksen:

  ![][007]

Saat lisätietoja Azure AD Connect [aloittaminen: Azure AD Connect](active-directory-aadconnect.md). Saat lisätietoja salasanan takaisinkirjoituksen [käytön aloittaminen: Azure AD salasanojen hallinta](active-directory-passwords-getting-started.md).


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Linkkejä salasanan palauttaminen dokumentaatio
Alla on linkkejä kaikkiin Azure AD-salasanan ohjeissa:

* **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).
* [**Käytön aloittaminen**](active-directory-passwords-getting-started.md) - tietoja, jotta käyttäjät voivat palauttaa ja muuttaa cloud tai paikalliseen salasanansa
* [**Mukauta**](active-directory-passwords-customize.md) - Opi mukauttamaan ulkoasun ja ilmeen ja palvelun organisaation tarpeisiin toiminta
* [**Parhaat käytännöt**](active-directory-passwords-best-practices.md) - Lue, miten voit ottaa nopeasti ja tehokkaasti salasanojen organisaation hallinta
* [**Hae tiedot**](active-directory-passwords-get-insights.md) - tietoja Microsoftin integroitu raportointiominaisuudet
* [**Usein kysytyt kysymykset**](active-directory-passwords-faq.md) - vastauksia usein kysyttyihin kysymyksiin
* [**Vianmääritys**](active-directory-passwords-troubleshoot.md) – Opi palvelun nopeasti liittyviä ongelmia
* [**Lisätietoja**](active-directory-passwords-learn-more.md) – Valitse syvä kyselyjä teknisiä tietoja palvelun toiminta



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
