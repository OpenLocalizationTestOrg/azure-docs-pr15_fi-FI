<properties
    pageTitle="Azure AD ja sovellusten: käsittää yritystietokannan sovelluskehittäjille | Microsoft Azure"
    description="Kirjoitettu IT-ammattilaisille, tässä artikkelissa on ohjeita integroimiseen sovellusten Azure Active Directory-hakemistosta."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure AD ja sovellusten: kehittää liiketoiminta-sovellukset

Tässä oppaassa on yleiskatsaus kehityssovellusten liiketoiminta-(LoB) varten Azure Active Directory (AD). Käyttäjäryhmää on Active Directory ja Office 365: n Yleiset järjestelmänvalvojat.

## <a name="overview"></a>Yleiskatsaus

Valitse oman organisaation kertakirjautumisen Office 365: n sovellusten Azure Active Directory-integrointi antaa käyttäjien. Ottaa sovelluksen Azure AD-avulla voit määrittää sovelluksen todennus-käytäntö. Lisätietoja ehdollisen access ja suojaaminen sovellusten monimenetelmäisen todentamisen (MFA) kanssa on [määrittäminen käyttösäännöt](active-directory-conditional-access-azuread-connected-apps.md).

Voit rekisteröidä sovelluksesi käyttämään Azure Active Directory. Sovellus rekisteröidään tarkoittaa, että kehittäjille voi käyttää Azure AD todentaa käyttäjät ja käyttäjän resurssit, kuten sähköpostin, kalenterin ja asiakirjojen käyttöoikeuksien pyytäminen.

Hakemistossa (ei vieraiden) jäsen voi rekisteröidä sovelluksen nimeltään muussa *sovelluksen objektin luominen*.

Sovelluksen rekisteröinti sallii käyttäjälle seuraavasti:

- Jäsenyyden hankkiminen niiden Azure AD tunnistavaa sovellusta
- Hae vähintään yksi tietoja/näppäimet, sovellus käyttämällä todennusta AD
- Tuotemerkin sovelluksen Azure-portaalissa mukautettu nimi, logo, jne.
- Käytä Azure AD luvan ominaisuudet-sovelluksessa mukaan lukien:
  - Roolipohjainen käyttöoikeuksien valvonta (RBAC)
  - Azure Active Directory oAuth luvan palvelimeksi (secure sovelluksen näyttämiä API)

- Tarvittavat käyttöoikeudet määritellä tarpeen toimisi oikein, mukaan lukien:-sovelluksen käyttöoikeuksien (vain Yleiset järjestelmänvalvojat). Esimerkki: roolin jäsenyyden toiseen Azure AD sovelluksen tai roolin jäsenyyden suhteessa Azure resurssi, resurssiryhmä- tai tilaus - valtuutetun käyttöoikeudet (kaikki käyttäjät). Esimerkki: Azure AD-kirjautuminen ja luku-profiili


> [AZURE.NOTE]Oletusarvon mukaan kuka tahansa jäsen voi rekisteröidä sovelluksen. Lisätietoja rekisteröitymisestä jäsenet-sovellusten käyttöoikeuksien rajoittamisesta on artikkelissa [miten sovellusten lisätään Azure AD](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Seuraavassa on hyödyllisiä, Yleinen järjestelmänvalvoja, voit tehdä niiden sovelluksen valmis tuotannon sovelluskehittäjät:

- Määritä käyttösäännöt (access-käytäntö/MFA)
- Määritä sovellus edellyttää käyttäjän varauksen ja määritettävä käyttäjille
- Estää oletusarvon suostumusta käyttäjäkokemus

## <a name="configure-access-rules"></a>Määritä käyttösäännöt

Määritä sovelluksen kohti käyttösäännöt SaaS sovelluksia. Voit esimerkiksi edellyttävät MFA tai Salli vain luotettavien verkoissa käyttäjien pääsyn. Tämän ominaisuuden tiedot ovat käytettävissä asiakirjan [määrittäminen käyttösäännöt](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Määritä sovellus edellyttää käyttäjän varauksen ja määritettävä käyttäjille

Oletusarvon mukaan käyttäjät voivat käyttää sovelluksia ilman on varattu. Jos sovellus paljastaa roolit tai jos haluat sovelluksen näkyvän käyttäjän access-paneeli, vaadit käyttäjän tehtävän.

[Käyttäjän varauksen vaatimista](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Jos ole Azure AD Premium- tai Enterprise Mobility Suite (EMS)-tilaaja, suosittelemme ryhmien käyttäminen. Käyttäjäryhmien määrittäminen sovelluksen avulla voit delegoida ryhmän omistajalle käyttöoikeuksien hallinnan. Voit luoda ryhmään tai pyydä vastaavan osapuolen organisaation ryhmän hallinta-ominaisuutta käyttämällä ryhmän luominen.

[Käyttäjien määrittäminen-sovellukseen](active-directory-applications-guiding-developers-assigning-users.md)  
[Käyttäjäryhmien määrittäminen-sovellukseen](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Piilota käyttäjän lupaa

Oletusarvoisesti jokaisella käyttäjällä käy läpi hyväksyntä-toiminto kirjautuminen. Käyttäjien käyttöoikeuksia sovellus, jossa pyydetään lupaa mobiilikäyttöä voi olla disconcerting käyttäjille, jotka eivät tunne tällaisten päätösten tekemiseen.

Sovellusten, joihin luotat voit yksinkertaistaa käyttäjäkokemuksen suostut organisaation puolesta-sovellukseen.

Saat lisätietoja käyttäjän lupaa ja Azure suostumusta-ratkaisun [Integrointi sovellusten Azure Active Directory-hakemistosta](active-directory-integrating-applications.md).

##<a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Suojatun etäkäyttö Azure AD-sovelluksen välityspalvelimen paikallisen sovellusten ottaminen käyttöön](active-directory-application-proxy-get-started.md)
- [Azure ehdollisen käyttöoikeuden SaaS sovellusten esikatselu](active-directory-conditional-access-azuread-connected-apps.md)
- [Azure AD-sovelluksia hallinta](active-directory-managing-access-to-apps.md)
- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
