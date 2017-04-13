<properties
    pageTitle="Salasanakäytännöt ja Azure Active Directoryn rajoitusten | Microsoft Azure"
    description="Tässä artikkelissa kuvataan käytäntöjä, jotka koskevat salasanat Azure Active Directoryn, mukaan lukien sallittu merkkien, pituus ja vanheneminen"
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Salasanakäytännöt ja Azure Active Directoryn rajoitukset

Tässä artikkelissa kuvataan salasanakäytännöt ja monimutkaisuusvaatimukset liittyvät Azure AD-kansioon tallennettu käyttäjätilit.

> [AZURE.IMPORTANT] **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName-käytäntöjä, jotka koskevat kaikkia käyttäjätilejä

Jokaisen käyttäjätili, jolla on Azure AD-Todennusjärjestelmän kirjautuminen arvon on oltava yksilöllinen nimi (UPN) määrite tiliin liittyvien. Seuraavassa taulukossa on jäsennykset käytäntöjä, jotka koskevat sekä paikallisen Active Directory Orpotermit käyttäjätilit (synkronoitu pilveen) ja vain pilvipalveluita käyttäjätileille.

|   Ominaisuus           |     UserPrincipalName-vaatimukset  |
|   ----------------------- |   ----------------------- |
|  Sallitut    |  <ul> <li>A – Ö</li> <li>a - ö </li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  Merkkiä, ei sallita  | <ul> <li>Mikä tahansa '@' merkki, joka on ei erotat toimialueen käyttäjänimi.</li> <li>Ei voi olla kauden merkin ".' edeltävän '@' symboli</li></ul> |
| Pituus-rajoitukset  |       <ul> <li>Koko pituus voi olla 113 merkit</li><li>ennen kuin 64 merkkiä ‘@’ symboli</li><li>Kun 48 merkkiä ‘@’ symboli</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Salasanakäytännöt, jotka koskevat vain cloud käyttäjätilit

Seuraavassa taulukossa on kuvattu käytettävissä salasanan käytäntöasetukset, jotka voidaan lisätä käyttäjätilit, jotka on luotu ja Azure AD-hallinnasta.

|  Ominaisuus       |    Vaatimukset          |
|   ----------------------- |   ----------------------- |
|  Sallitut   |   <ul><li>A – Ö</li><li>a - ö </li><li>0 – 9</li> <li>@# $ % ^ & * - _ ! + [] {} = & #124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  Merkkiä, ei sallita   |       <ul><li>Unicode-merkkejä</li><li>Välilyönnit</li><li> **Vahvojen salasanojen**: ei voi olla piste merkin ".' edeltävän '@' symboli</li></ul> |
|   Salasanan rajoitukset | <ul><li>vähintään 8 merkkiä ja 16 merkkiä</li><li>**Vahvojen salasanojen**: edellyttää 3 ulos 4 seuraavasti:<ul><li>Pieniä kirjaimia</li><li>Isot kirjaimet</li><li>Luvut (0-9)</li><li>Symbolit (katso yllä salasanan rajoituksia)</li></ul></li></ul> |
| Salasanan vanhentumisen kesto      | <ul><li>Oletusarvo: **90** päivää </li><li>Arvo on määritettävä Active Directory moduulin Windows PowerShellin Azure-määrittäminen MsolPasswordPolicy cmdlet-komennolla.</li></ul> |
| Salasanan vanhentumisen ilmoituksen |  <ul><li>Oletusarvo: **14** päivää (ennen vanhenee)</li><li>Arvo on määritettävä määrittäminen MsolPasswordPolicy cmdlet-komennolla.</li></ul> |
| Salasanan umpeutuminen |  <ul><li>Oletusarvo: **Epätosi** päivää (osoittaa, että salasanan umpeutuminen on käytössä) </li><li>Yksittäisten käyttäjätilien Set-MsolUser-cmdlet-komennolla voit määritetty arvo. </li></ul> |
|  Salasanojen  | Viimeinen salasana ei voi käyttää uudelleen. |
|  Salasanan historia kesto | Jatkuva |
|  Tilin lukitseminen | 10 epäonnistua kirjautumisen yritysten jälkeen (väärän salasanan) on lukittuna käyttäjän minuutin ajan. Edelleen virheellisiä Kirjautumisvirheitä aiheuttavien yritykset lukkiutuu käyttäjän lisätä kestoja. |


## <a name="next-steps"></a>Seuraavat vaiheet

* **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).
* [Salasanojen hallinta missä tahansa](active-directory-passwords.md)
* [Salasanojen hallinta toiminta](active-directory-passwords-how-it-works.md)
* [Salasanan hallintaa (IRM) käytön aloittaminen](active-directory-passwords-getting-started.md)
* [Mukauta salasanojen hallinta](active-directory-passwords-customize.md)
* [Salasanojen hallinta parhaista käytännöistä](active-directory-passwords-best-practices.md)
* [Toiminnallisia havainnollistamisen tehostaminen salasanan hallintaraporttien hankkiminen](active-directory-passwords-get-insights.md)
* [Salasanojen hallinta usein kysytyt kysymykset](active-directory-passwords-faq.md)
* [Salasanojen hallinta vianmääritys](active-directory-passwords-troubleshoot.md)
* [Opi lisää](active-directory-passwords-learn-more.md)
