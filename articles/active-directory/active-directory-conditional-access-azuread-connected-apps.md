<properties
    pageTitle="Azure ehdollisen käyttöoikeuden SaaS sovellusten | Microsoft Azure"
    description="Azure AD-ehdollinen Accessin avulla voit määrittää sovelluksen kohti monimenetelmäisen todentamisen käyttösäännöt ja estää ei-luotettujen verkon käyttäjien mahdollisuus. "
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="markvi"/>

# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Azure ehdollinen Active Directoryn käytön aloittaminen

Azure Active Directory ehdollisen käyttöoikeuden [SaaS](https://azure.microsoft.com/overview/what-is-saas/) -sovellusten ja Azure AD yhteydessä sovellusten avulla voit määrittää ryhmän, sijainti ja sovelluksen luottamuksellisuus perusteella ehdollisen käyttöoikeuden. 

Sovelluksen luottamuksellisuus perusteella ehdollinen Accessissa voit määrittää monimenetelmäisen todentamisen (MFA) käyttösäännöt sovellusta kohden. MFA kohti sovellus mahdollistaa käyttäjille, jotka eivät ole luotetussa verkon käytön estäminen. Voit käyttää MFA säännöt kaikille käyttäjille, jotka on määritetty sovellukseen tai vain ryhmän suojausryhmien.  Käyttäjät voidaan jättää ulkopuolelle MFA vaatimus, jos ne ovat käyttäminen sovelluksen sisällä organisaation verkon IP-osoite.

Nämä ominaisuudet ovat käytettävissä asiakkaille, jotka on ostettu Azure Active Directory-Premium-käyttöoikeus.

## <a name="scenario-prerequisites"></a>Skenaario edellytykset
* Käyttöoikeuden määrittäminen Azure Active Directory-Premium

* Liitetty tai hallittu Azure Active Directory-vuokraajan

* Liitetyt alihallinnat edellyttää, että monimenetelmäisen todentamisen on käytössä.

## <a name="configure-per-application-access-rules"></a>Sovelluksen kohti käyttösäännöt määrittäminen

Tässä osassa kuvataan, miten voit määrittää sovelluksen kohti käyttösäännöt.

1. Kirjaudu Azure perinteinen-portaaliin tilille, jolla on yleisen järjestelmänvalvojan Azure AD avulla.
2. Valitse vasemmanpuoleisessa ruudussa **Active Directorysta**.
3. Valitse kansio-välilehdessä hakemistossa.
4. Valitse **sovellukset** -välilehti.
5. Valitse sovellus, jossa säännön määrittäminen.
6. Valitse **määritys** -välilehti.
7. Vieritä access säännöt-osaan. Valitse haluamasi käyttösäännöt.
8. Määritä säännön koskevat käyttäjät.
9. Käytännön käyttöön valitsemalla **käytössä olevan**.

##<a name="understanding-access-rules"></a>Tietoja käyttösäännöt

Tässä osassa kerrotaan tuetut Azure ehdollinen sovellusten käyttö käyttösäännöt yksityiskohtainen kuvaus.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Käyttäjien määrittäminen access koskevat

Oletusarvon mukaan kaikki käyttäjät, joilla on sovelluksen käyttöä koskevat käytännön. Voit myös rajoittaa käytännön käyttäjille, jotka ovat määritettyä suojausryhmien jäseniä. **Lisää ryhmä** -painikkeen avulla voit valita yhden tai useamman ryhmän valitseminen-valintaikkunassa, joita access-sääntöä käytetään. Tämän valintaikkunan avulla voidaan myös poistaa valitut ryhmät. Kun säännöt valituista käyttää ryhmiä, access-säännöt vain suoritettaviksi käyttäjät, jotka kuuluvat johonkin määritetyn käyttöoikeusryhmät.

Käyttöoikeusryhmät voivat myös erikseen ulkopuolelle käytännön **lukuun ottamatta** -vaihtoehdon valitseminen ja määrittämällä yksi tai useita ryhmiä. Käyttäjät, jotka ovat **lukuun ottamatta** -luettelossa-ryhmän jäsen ‑osa ei ole veloittaa monimenetelmäisen todentamisen tarpeen, vaikka heillä ei olisikaan, jota access-sääntö koskee ryhmän jäsen.
Alla olevassa käyttösäännöt edellyttävät monimenetelmäisen todentamisen käytettäessä sovelluksen valvojat-ryhmän kaikki käyttäjät.

![Asetus ehdollinen käyttösäännöt MFA kanssa](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Ehdollinen käyttösäännöt MFA kanssa
Jos käyttäjä on määritetty käyttäjäkohtainen multi-factor authentication-toiminnon avulla, tämä asetus, jos käyttäjä yhdistää sovelluksen multi-factor authentication-sääntöjä. Tämä tarkoittaa käyttäjä, joka on määritetty käyttäjäkohtainen monimenetelmäisen todentamisen tarvitaan suorittamiseen monimenetelmäisen todentamisen, vaikka ne on tehty poikkeus sovelluksen monimenetelmäisen todentamisen säännöt. Lisätietoja multi-factor authentication ja käyttäjäkohtainen asetukset.

### <a name="access-rule-options"></a>Säännön käyttöasetukset
Tuetaan seuraavista vaihtoehdoista:

* **Monimenetelmäisen todentamisen edellyttävät**: käyttäjät, joille access koskevat, on suoritettava valmis monimenetelmäisen todentamisen ennen käytännön käytössä olevan sovelluksen käyttöä.

* **Monimenetelmäisen todentamisen kun ei töissä edellyttävät**: käyttäjä, joka on peräisin luotettujen IP-osoitetta ei vaadita monimenetelmäisen todentamisen suorittamiseen. Monimenetelmäisen todentamisen asetukset-sivulla voidaan määrittää luotettujen IP-osoitealueet.

* **Kun ei töissä käytön estäminen**: käyttäjä, joka on luotettu IP-osoite ei ole peräisin estetään. Monimenetelmäisen todentamisen asetukset-sivulla voidaan määrittää luotettujen IP-osoitealueet.

### <a name="setting-rule-status"></a>Säännön tilan määrittäminen
Käyttöoikeuksien sääntöä tilaa avulla sääntöjen ottaminen käyttöön ja poistaa sen käytöstä. Kun access-säännöt ovat poissa käytöstä, monimenetelmäisen todentamisen vaatimus ei säilytetä.

### <a name="access-rule-evaluation"></a>Accessin käsittelemisen

Käyttösäännöt arvioidaan, kun käyttäjä käyttää liitetyt sovellus, joka käyttää OAuth 2.0, OpenID yhteyden, SAML tai WS Federation. Lisäksi käyttösäännöt arvioidaan, kun OAuth 2.0- ja OpenID yhteyden muokkaaminen access-tunnuksen päivityksen tunnuksen avulla. Jos käytännön arviointi epäonnistuu, kun päivityksen tunnuksen käytetään, palautetaan virhe **invalid_grant** , tämä ilmaisee käyttäjän täytyy tarkistamiseen uudelleen asiakkaalle.

###<a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Liittoutumispalvelut antamaan monimenetelmäisen todentamisen määrittäminen

Liitetyt alihallinnoista, saat MFA voidaan suorittaa Azure Active Directory tai paikallisen AD FS ‑palvelin.

Oletusarvon mukaan MFA ilmenee ylläpitämä Azure Active Directory-sivulla. MFA paikallisen määrittämiseen **– SupportsMFA** -ominaisuus on määritettävä **Tosi** Azure Active Directoryn käyttämällä Windows PowerShellin Azure AD-moduulin.

Seuraavassa esimerkissä esitetään ottamisesta käyttöön paikallisen MFA käyttämällä contoso.com-vuokraajassa [määrittäminen MsolDomainFederationSettings cmdlet-komennon](https://msdn.microsoft.com/library/azure/dn194088.aspx) :

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Määrittämisen tämän merkinnän lisäksi suorittamiseen monimenetelmäisen todentamisen on oltava määritettynä liitetyt vuokraajan AD FS-esiintymä. [Azure Monimenetelmäisen todentamisen paikalliseen käyttöönottoon](../multi-factor-authentication/multi-factor-authentication-get-started-server.md)ohjeiden mukaisesti.

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Office 365: een ja muiden sovellusten käytön yhteydessä Azure Active Directory](active-directory-conditional-access.md)
- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
