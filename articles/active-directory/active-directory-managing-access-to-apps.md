<properties
  pageTitle="Hallinta käyttämällä Azure AD-sovelluksia |  Microsoft Azure"
  description="Tässä artikkelissa kuvataan, miten Azure Active Directoryn avulla organisaatiot voivat määrittää sovellukset, johon kukin käyttäjä voi käyttää."
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>Sovellusten käyttöoikeuksien hallinta

Käyttöoikeuksien hallinta, käytön arviointi ja raportoinnin edelleen olla hankalaa, kun sovellus on integroitu organisaation tunnistetietojen järjestelmän. Monissa tapauksissa IT-järjestelmänvalvojien tai tukipalvelusta on otettava jatkuvaa aktiivinen rooli-sovelluksia käyttöoikeuksien hallinta. Joskus Yleiset tai osaston IT-työryhmän suorittaa tehtävän. Usein varauksen päätös eivät voi delegoida business päätös-maker, ennen kuin se tekee niiden hyväksymistä varauksen.  Muiden organisaatioiden sijoittaa integrointi aiemmin luotuun automaattinen tunnistetietojen ja käytön hallintajärjestelmän, kuten Roolipohjainen käyttöoikeuksien valvonta (RBAC) tai määritteiden perusteella käytönvalvonta (ABAC). Integrointi ja säännön kehittäminen yleensä erityinen ja kallista. Seurantaa tai raportoinut joko hallintatavan on omat erilliset, kallista ja monimutkaisia sijoitus.

## <a name="how-does-azure-active-directory-help"></a>Kuinka Azure Active Directoryn avulla?

 Azure AD tukee laaja käyttöoikeuksien hallinnan määritetyn sovellusten ottaminen käyttöön organisaatiot voivat helposti saavuttamiseksi oikean access käytännöt vakiovalikoimasta automaattinen, määrite varauksen (ABAC tai RBAC skenaariota) – delegointi ja järjestelmänvalvojan hallinta. Azure AD helposti toteuttaa monimutkaisia käytännöt yhdistäminen useita yhden sovelluksen hallinnan mallit ja voi myös käyttää uudelleen hallinnan säännöt saman käyttäjäryhmien sovellusten välillä.

 - [Uuden lisääminen tai olemassa olevien sovellusten käyttäminen](active-directory-sso-integrate-saas-apps.md)


 Azure AD-sovelluksen varauksen ohjeessa on ensisijainen tehtävän tilasta:

- **Yksittäisen kuormituksen** Järjestelmänvalvoja directory yleisen järjestelmänvalvojan oikeuksilla voit valita yksittäisiä käyttäjätilejä ja antaa heille sovelluksen käyttöä.
- **Ryhmän perustuva liitos (maksettu Azure AD)** Kansion yleisen järjestelmänvalvojan oikeuksilla järjestelmänvalvoja määrittää ryhmän sovellus. Tiettyjen käyttäjien käyttöoikeuksia määräytyy ovatko ryhmän jäsenet hän yrittää käyttää sovelluksen aikaan. Toisin sanoen järjestelmänvalvoja voi luoda tehtävän säännöllä "Nykyinen määritetyn ryhmän jäsen on sovelluksen käyttöoikeuksia", jossa sanotaan tehokkaasti. Tämä tehtävä-vaihtoehto järjestelmänvalvojat etuja mistä tahansa Azure AD ryhmän hallinta-asetuksia, kuten [määrite perustuvan dynaamisen ryhmät](active-directory-accessmanagement-manage-groups.md), ulkoisen järjestelmän ryhmät (esimerkiksi paikallisen Active Directory tai työpäivä) tai järjestelmänvalvojan tai Mykistä-service rajoitetun ryhmiä. Yhden ryhmän voi helposti määrittää useita sovelluksia varmistaminen varauksen affiniteetti sovellusten voit jakaa tehtävien säännöt, pienentämällä Yleinen hallinta kyselyä. Huomaa, että sisäkkäisen ryhmän jäsenyyksiä ei tue ryhmän perustuva varauksen sovellusten tällä hetkellä.

Käytä kahden tehtävän kolmesta tilasta, järjestelmänvalvojat voit tehostaa minkä tahansa olisi varauksen hallintatavan.

Azure AD-kanssa käyttö- ja varauksen raportointi on täysin integroitu-järjestelmänvalvojat voivat helposti raportoinnissa tehtävän tilan, tehtävän virheet ja jopa käyttö ottaminen käyttöön.

## <a name="complex-application-assignment-with-azure-ad"></a>Monimutkaisen sovelluksen Azure AD tehtävään

Harkitse jokin sovellus, esimerkiksi Salesforce. Useissa yrityksissä Salesforce käyttää pääasiassa markkinointi- ja organisaatioissa. Markkinoinnin jäseniltä on usein etuoikeutettu Salesforce-pääsy erittäin samalla, kun myynnin jäseniltä on rajoitetut käyttöoikeudet. Monissa tapauksissa laaja populaation tietotyöntekijät rajoittaa sovelluksen. Sääntöjen poikkeusten vaikeuttaen asioita. Se on usein markkinoinnin tai myynnin johtajuus ryhmiä käyttöoikeuden tai muuttaa niiden roolit ulkopuolisista yleinen sääntöjen prerogative.

Azure AD-sovelluksia, kuten Salesforce voi olla valmiiksi määritetyn kertakirjautuminen (SSO) ja automaattinen valmistelu. Kun sovellus on määritetty, järjestelmänvalvoja voi ottaa erikseen toiminto, joka luo ja määritä haluamasi ryhmät. Tässä esimerkissä järjestelmänvalvoja voi suorittaa seuraavat tehtävät:

- [Dynaamiset ryhmät](active-directory-accessmanagement-manage-groups.md) voidaan määrittää edustamaan kaikkien jäsenten käyttämällä määritteitä, kuten osaston tai rooli markkinointi- ja ryhmät automaattisesti:

    - Kaikkien jäsenten markkinoinnin ryhmät määritetään "markkinoinnin" Salesforce-roolin

    - Kaikkien jäsenten myyntitiimiksi "Myynti" Salesforce-roolin määritetään ryhmät. Lisätietoja tarkennuksen voi käyttää useita ryhmiä, jotka edustavat alueellisen eri Salesforce-rooleille määritettyjen myynnin ryhmiä.

- Poikkeus-järjestelmä käyttöön Omatoiminen ryhmän onnistunut kunkin roolin. Esimerkiksi "Salesforce markkinoinnin poikkeus"-ryhmän voi luoda omatoimisen ryhmänä. Ryhmän voi määrittää markkinoinnin Salesforce-roolin ja markkinoinnin johtajuus ryhmän voidaan tehdä omistajat. Markkinoinnin johtajuus ryhmän jäsenten voitu lisääminen tai poistaminen käyttäjät, liity-käytännön määrittäminen tai jopa Hyväksy tai estä liittymisen yksittäisten käyttäjien pyynnöt. Tätä tuetaan tiedot työntekijä tarvittavat sisäänrakennetun, joka ei tarvitse erityisiä koulutus omistajat ja jäsenet kautta.

Tässä tapauksessa kaikkien varattujen käyttäjien automaattisesti valmisteltava Salesforce-, ne on lisätty niiden roolimääritys päivittää Salesforce eri ryhmille. Käyttäjät voi löydät ja käyttää Salesforce – Microsoft application access Ohjauspaneelin, Office web-asiakkaille, tai jopa siirtymällä niiden organisaation Salesforce-kirjautumissivulle. Järjestelmänvalvojat olisi voivat tarkastella helposti käyttö ja tehtävän tilan kirjaamiseen Azure AD-raportointia varten.

Järjestelmänvalvojat lopputuloksen access käytännöt tietyn roolien [Azure AD ehdollisen käyttöoikeuden](active-directory-conditional-access.md) . Käytännöt sisällyttää onko käyttö on sallittu yritysympäristössä ja jopa Monimenetelmäisen todentamisen tai laitteen vaatimukset saavuttamiseksi access eri tapauksissa ulkopuolella.

## <a name="how-can-i-get-started"></a>Miten voi aloitetaan?

Ensimmäinen, jos et ole vielä Internetiin Azure AD ja olet järjestelmänvalvoja:

 - [Kokeile!](https://azure.microsoft.com/trial/get-started-active-directory/) -voi maksuton 30 päivän kokeiluversio tänään rekisteröityminen ja ottaa ensimmäisen cloud-ratkaisua kohdasta 5 minuuttia, voit käyttää tätä linkkiä

Azure AD-ominaisuuksia, jotka mahdollistavat tilin jakaminen ovat seuraavat:

- [Ryhmämääritys](active-directory-accessmanagement-self-service-group-management.md)
- Azure AD-sovellusten lisääminen
- Tehtävän käytön aloittaminen
- Sovelluksen varauksen usein kysytyt kysymykset
- [Sovelluksen Raporttinäkymät-ikkunan/käyttöraportit](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Mistä saan lisätietoja?

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Sovellusten ehdollinen käytön suojaaminen](active-directory-conditional-access.md)
- [Omatoiminen ryhmän hallinta/SSAA](active-directory-accessmanagement-self-service-group-management.md)
