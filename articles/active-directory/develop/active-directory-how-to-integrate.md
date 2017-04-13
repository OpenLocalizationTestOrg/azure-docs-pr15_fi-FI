<properties
   pageTitle="Voit integroida Azure Active Directory | Microsoft Azure"
   description="Apuviivaan edut ja resurssien Azure Active Directory-integrointia varten."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="integrating-with-azure-active-directory"></a>Azure Active Directory-integraation

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory on organisaatiot, joiden yrityksen arvosanojen käyttäjähallinnan cloud sovellukset.  Azure AD-integrointi antaa käyttäjien virtaviivaistettu kirjautuminen ja IT-käytännön mukainen sovelluksen avulla.

## <a name="how-to-integrate"></a>Miten voit integroida

Voit integroida Azure AD sovelluksen useilla tavoilla.  Voit hyödyntää mahdollisimman monta tai vähän tilanteista kuin vastaava sovelluksen.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Tue Azure AD-sovellukseen sisäänkirjautuminen avulla

**Pienennä hankaukselle kirjautumisongelmien ja pienentää.** Käyttämällä Azure AD kirjautumaan sovelluksesi käyttäjät ei tarvitse Lisää nimiä ja salasanan.  Kehittäjän sinun on yksi vähemmän tallentamiseen ja suojaa salasana.  Ei tarvitse käsitellä unohtuneen salasanan vaihtamisesta eivät ole merkittäviä säästöjen, yksinään.  Azure AD tehostaa merkki-joillekin maailman suosituimpia cloud-sovellukset, myös Office 365: ssä ja Microsoft Azure.  Kanssa miljoonia satoja käyttäjiltä organisaatiot miljoonia on käyttäjälle on jo kirjautunut Azure AD.  Lisätietoja [lisääminen tuki Azure AD-Kirjaudu sisään](../active-directory-authentication-scenarios.md).

**Yksinkertaistaa Kirjaudu määrittäminen sovelluksen.**  Rekisteröidy sovelluksen, aikana Azure AD lähettää tärkeitä tietoja käyttäjän niin, että voit täyttää valmiiksi rekisteröityminen täyttämällä lomakkeen tai poistaa sen kokonaan.  Käyttäjät voivat kirjautua sovelluksen kautta kaltaisia sosiaalisen median ja mobile-sovellusten a tuttuja hyväksyntä-toiminto niiden Azure AD-tilin avulla.  Kuka tahansa käyttäjä, voit rekisteröityä ja kirjaudu sisään sovellukseen, joka on integroitu Azure AD niin, ettei se osallistuminen.  Lue lisää [allekirjoitus-up sovelluksesi Azure AD-tilille kirjaudutaan](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Selaa käyttäjille ja hallita käyttäjän valmistelu sovelluksen käyttöoikeuksien hallinta

**Selaa käyttäjää hakemistossa.**  Graph-Ohjelmointirajapinnan avulla käyttäjä voi etsiä ja kutsumalla muita organisaation muiden käyttäjien selaamalla tai myöntäminen asemesta voit kirjoittaa sähköpostin osoitteet.  Käyttäjät voivat selata käyttämällä tuttuja osoite kirjan tyylin käyttöliittymä, mukaan lukien organisaation hierarkian tietojen tarkasteleminen.  Lisätietoja [Kaavion API](../active-directory-graph-api.md).

**Voit käyttää Active Directory-ryhmä ja jakeluluettelot asiakkaalle on jo hallinta uudelleen.**  Azure AD sisältää toisiinsa, että asiakkaalle on jo sähköpostin käyttämällä ja käyttöoikeuksien hallinta.  Graph-Ohjelmointirajapinnan käyttäminen käyttää uudelleen ryhmiin sen sijaan, että edellyttävän asiakkaalle, voit luoda ja hallita joukko sovelluksessa ryhmiä.  Ryhmätiedot voidaan lähettää myös tunnusten Kirjaudu sovellukseen.  Lisätietoja [Kaavion API](../active-directory-graph-api.md).

**Käytä Azure AD, kenellä on oikeus sovelluksen ohjausobjektiin.**  Järjestelmänvalvojat ja Azure AD-sovelluksen omistajat voivat määrittää access sovellusten tietyille käyttäjille ja ryhmille.  Graph-Ohjelmointirajapinnan käyttäminen, voit lukea tämän luettelon ja käyttää sitä valmistelu ja resurssien varaustiedoista valmistelu ja sovelluksen käyttämiseen.

**Käytä Azure AD-roolien perusteella käyttöoikeuksien hallinta.**  Järjestelmänvalvojat ja sovelluksen omistajat voivat määrittää käyttäjien ja ryhmien roolit, joilla voit määrittää, kun sovellus rekisteröidään Azure AD.  Roolitiedot lähetetään tunnusten sisäänkirjautuminen sovelluksen ja voi lukea Graph-Ohjelmointirajapinnan käyttäminen.  Lisätietoja [Azure Active Directory käyttöoikeuksien käyttämisestä](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Pääset käsiksi käyttäjäprofiilin, kalenterin, sähköposti, yhteystiedot, tiedostoja ja muut

**Azure AD on Office 365: ssä ja muiden Microsoft business-palveluiden todennus-palvelin.**  Jos Azure AD tue kirjauduttaessa sovellukseen tai tuki nykyiset käyttäjätilit linkittäminen käyttämällä OAuth 2.0 Azure AD-käyttäjätilejä, voit pyytää luku- ja kirjoitusoikeudet käyttäjäprofiilin, kalenterin, sähköpostin, yhteystietojen, tiedostoja ja muita tietoja.  Voit saumattomasti kirjoittaa käyttäjän kalenterin tapahtumia ja lukea tai kirjoittaa tiedostoja OneDrive.  Lisätietoja [Office 365-ohjelmointirajapinnan käyttäminen](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Siirrä ylemmälle tasolle sovelluksesi Azure-ja Office 365: n kauppapaikat

**Voit viedä organisaatiot, joilla on jo käytössä Azure AD miljoonia sovellusta.**  Käyttäjät, jotka Etsi tiedosto ja Etsi nämä kauppapaikat on jo käytössä yksi tai useita pilvipalveluihin, että ne qualified cloud palvelun asiakkaat.  Lisätietoja edistäminen sovelluksesi [Azure](https://azure.microsoft.com/marketplace/partner-program/)Marketplacesta.

**Kun käyttäjien rekisteröinti tapahtuu sovelluksen, se näkyy niiden Azure AD access-ruudusta ja Office 365-sovellusten käynnistimeen.**  Käyttäjät pystyvät nopeasti ja helposti palauttaa sovelluksen myöhemmin parantaminen käyttäjän välitys.  Lisätietoja [Azure AD käyttää Ohjauspaneelin](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Device Servicen ja palvelun tietosuojaa

**Käytön Azure AD jäsenyyksien hallinta palveluiden ja-laitteet vähentää sinun on kirjoitettava koodi ja ottaa käyttöön IT voit hallita käyttöoikeuksia.**  Palveluiden ja -laitteet voivat tunnusten saaminen Azure AD käyttämällä OAuth ja käyttämään näiden tunnusten verkko-ohjelmointirajapinnan.  Käyttämällä Azure AD voit välttää monimutkaisia todennus koodin kirjoittamista.  Koska palvelut ja laitteiden käyttäjätiedot tallennetaan Azure AD-IT voit hallita näppäimet ja kumottujen varmenteiden sen sijaan, että voit tehdä tämän erikseen sovelluksen yhdessä paikassa.

## <a name="benefits-of-integration"></a>Integrointi edut

Azure AD-integrointi sisältyy edut, joita ei tarvitse kirjoittaa koodia.

### <a name="integration-with-enterprise-identity-management"></a>Integrointi yrityksen jäsenyyksien hallinta

**Auttaa noudattamaan IT käytännöt sovelluksen.**  Organisaatioiden integroida niiden yrityksen käyttäjätietojen hallinta-järjestelmiä Azure AD-niin henkilön jättää organisaation, kun ne automaattisesti menettävät käyttöoikeutensa sovelluksen ilman IT hänen nimeään tarvitse lisätä vaatii ylimääräisiä toimia.  IT voit hallita, kuka voi käyttää sovelluksen ja selvittää, mitä käytäntöjen - varten tarvittavat esimerkissä multi-factor authentication - pienentämisestä sinun on ehkä monimutkaisia yrityksen käytäntöjä noudattamisen koodin kirjoittaminen.  Azure AD tarjoaa Järjestelmänvalvojat, joilla on yksityiskohtaiset valvontaloki, jotka kirjautunut sovelluksen niin se voi seurata käyttöä.

**Azure AD laajentaa Active Directory pilveen, jotta sovelluksesi voidaan yhdistää AD.**  Monet organisaatiot maailmanlaajuisesti käyttämällä Active Directory sekä niiden pääasiallisen kirjautuminen käyttäjätietojen hallintajärjestelmän ja edellyttävät verkkosovelluksistaan AD-käyttöä varten.  Sovelluksen Azure AD-integraation integroituu Active Directorysta.

### <a name="advanced-security-features"></a>Laajennettu ominaisuudet

**Monimenetelmäisen todentamisen.**  Azure AD sisältää alkuperäisen monimenetelmäisen todentamisen.  IT-järjestelmänvalvojien edellyttää, että multi-factor authentication-sovelluksen käyttämään niin, että sinun ei tarvitse koodi tuki.  Lisätietoja [Monimenetelmäisen todentamisen](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Erheellisiin Kirjaudu sisään tunnistus.**  Azure AD käsittelee Kirjaudu apuohjelmien yli miljardia päivässä käytettäessä tietokoneen learning algoritmit tunnistaa epäilyttävistä tehtävän ja ilmoittaa mahdolliset IT-järjestelmänvalvojille.  Sovelluksen saa tukemalla Azure AD-kirjautuminen etu on suojaus. Lisätietoja [tarkasteleminen Azure Active Directory access-raportti](../active-directory-view-access-usage-reports.md).

**Ehdollisen käyttöoikeuden.**  Monimenetelmäisen todentamisen lisäksi järjestelmänvalvojat voivat edellyttää tiettyjen ehtojen on täytyttävä, jotta käyttäjät voivat kirjautua sisään sovelluksen.  Ehdot, jotka voit määrittää Sisällytä IP-osoitealueita asiakaslaitteet, määritettyjen ryhmien jäsenyyden ja access käyttää laitteen tilan.  Lisätietoja [ehdollisen Azure Active Directory-käyttöoikeuden](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Helppo kehittäminen

**Teollisuuden vakio protokollat.**  Microsoft on sitoutunut tukevat yleisesti käytettyjen.  Azure AD tukee SAML 2.0, OpenID yhteyden 1.0 OAuth 2.0 tai WS Federation 1.2 todennus-protokollaa.  Graph-Ohjelmointirajapinnan on yhteensopiva OData 4.0.  Jos jo sovellus tukee SAML 2.0 tai OpenID yhteyden 1.0-protokollaa liitetyt kirjauduttaessa, lisäämällä Azure AD-tuki voidaan yksinkertaista.  Lisätietoja [Azure AD tue todennusprotokollat](../active-directory-authentication-protocols.md).

**Avaa lähde-kirjastot.**  Microsoft toimittaa täysin tuettu Avaa lähde kirjastojen Suositut kielet ja ympäristöjen nopeus kehitystä.  Lähdekoodin käyttöoikeus Apache 2.0-kohdassa ja voit vapaasti haarautuvat ja osallistua takaisin projektit.  Lisätietoja [Azure AD-todennus kirjastoihin](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Maailmanlaajuinen tavoitettavuus- ja suuri käytettävyys

**Azure AD on otettu käyttöön palvelinkeskusten eri puolilla maailmaa ja on hallittu ja seurattava ympäri.**  Azure AD on käyttäjätietojen hallintajärjestelmän Microsoft Azure-ja Office 365: ssä, ja se on otettu käyttöön 28 palvelinkeskusten eri puolilla maailmaa.  Kansion tiedot on varmasti replikoida vähintään kolme palvelinkeskusten.  Yleinen kuormituksen tasoitusmääritykset varmistaa käyttäjät voivat käyttää lähimpään kopio, niiden tiedot sisältävä Azure AD ja uudelleen reitittää automaattisesti muihin palvelinkeskusten pyynnöt Jos havaitaan ongelma.

## <a name="next-steps"></a>Seuraavat vaiheet

[Aloita kirjoittaminen koodi](../active-directory-developers-guide.md#getting-started).

[Allekirjoita-käyttäjät käyttämällä Azure AD](../active-directory-authentication-scenarios.md)
