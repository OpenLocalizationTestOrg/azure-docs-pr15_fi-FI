
<properties
    pageTitle="Azure Active Directory ehdollisen käyttöoikeuden teknisissä | Microsoft Azure"
    description="Ohjausobjektin ehdollisen käyttöoikeuden kanssa Azure Active Directory tarkistaa tiettyjen ehtojen, voit valita, kun käyttäjän todentamista ja ennen kuin sallit sovelluksen käyttöä. Ehtojen täyttyessä, kun käyttäjä on todennus ja sovelluksen käyttöoikeus."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Azure Active Directory ehdollisen käyttöoikeuden tekniset tiedot

## <a name="services-enabled-with-conditional-access"></a>Käytössä ehdollinen Access Services
Ehdollinen käyttösäännöt tuetaan eri Azure AD-sovellusten välillä. Tämä luettelo sisältää:

- Liitetty sovellusten Azure AD-sovelluksen valikoimasta
- Salasanan SSO-sovelluksia Azure AD-sovelluksen valikoimasta
- Sovellusten rekisteröityjä Azure sovelluksen välityspalvelin
- Liiketoiminnan ja usean vuokraajan sovellusten Azure AD rekisteröityjä kehittämä rivi
- Visual Studio
- Azure Remote-sovellus
-   Dynamics CRM:
- Microsoft Office 365: n Yammer
- Microsoft Office 365: n Exchange Online
- Microsoft Office 365 SharePoint Online (mukaan lukien OneDrive for Business)


## <a name="enable-access-rules"></a>Ota käyttöön käyttösäännöt

Kukin sääntö voi ottaa käyttöön tai poistettu käytöstä sovelluksen kantalukujen kohden. Kun säännöt ovat **edelleen** ne käytössä ja ulkoisille käyttäjille sovelluksen pakotettu. Kun ne ovat **käytöstä** ei käytetä ja eivät vaikuta kokemus käyttäjien kirjautuminen.

## <a name="applying-rules-to-specific-users"></a>Sääntöjen soveltaminen tietyille käyttäjille
Sääntöjen voi suojata tiettyjä käyttäjille käyttöoikeusryhmän määrittämällä **Käytä**perusteella. **Käytä** voidaan määrittää **Kaikille käyttäjille** tai **ryhmille**. Jos määritetty **Kaikille** käyttäjille säännöt koskevat kuka tahansa käyttäjä, sovelluksen käyttämiseen. **Ryhmät** -vaihtoehdon avulla tiettyjä suojaus- ja jakeluryhmien on valittuna, säännöt pakotetaan suoritettaviksi vain näitä ryhmiä varten.

Säännön käyttöönotossa on yhteinen ensin Käytä sitä rajoitetun useat käyttäjät, jotka ovat pilottivaiheisiin ryhmissä. Kun valmis sääntö voi suojata **Kaikille**käyttäjille. Tämä aiheuttaa säännön suoritettaviksi kaikille organisaation käyttäjille.

Ryhmiä voidaan myös vapauttaa käytännön **lukuun ottamatta** -vaihtoehdon. Kaikki jäsenet ryhmän vapauttaa, vaikka ne näkyvät mukana ryhmän.

## <a name="at-work-networks"></a>"Töissä" verkot


Ehdollinen käyttösäännöt, jotka "töissä", verkon riippuvaisia luotettujen IP-osoitealueet, jotka on määritetty Azure AD-AD FS "sisällä corpnet"-ryhmän tai. Seuraavat säännöt ovat seuraavat:

- Vaadi monimenetelmäisen todentamisen kun ei töissä
- Kun ei töissä käytön estäminen

Tukivaihtoehdot tarkkuuden "töissä" verkot

1. Määrittää [monimenetelmäisen todentamisen määrityssivulla](../multi-factor-authentication/multi-factor-authentication-whats-next.md)luotettujen IP-osoitealueet. Ehdollinen käyttöoikeuskäytäntö käyttämällä määritettyjä alueita kaikissa todennuspyynnön ja suojaustunnuksen liittoutumispalvelujen Laske sääntöjä. 
2. Määritä käyttö sisällä väittää corpnet tämä vaihtoehto voidaan käyttää liitetyt hakemistojen selaaminen, AD FS avulla. [Lisätietoja sisällä coronet saatavat](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Määritä julkinen IP-osoitealueet. Määritä-välilehdessä hakemistossa, voidaan asettaa julkisen IP-osoitteet. Ehdollinen Access käyttää näitä "töissä" IP-osoitteet, näin määrittäminen yli 50 IP-osoite rajoituksen, joka on toimialuerekisteröintipalvelun MFA asetukset-sivun Lisää alueita.



## <a name="rules-based-on-application-sensitivity"></a>Sovelluksen luottamuksellisuus perustuvat säännöt

Säännöt määritetään salliminen suuren arvon palveluiden sovelluksen suojattu ilman vaikuttavat muiden palvelujen kohden. Ehdollinen käyttösäännöt voi määrittää sovelluksen **määrittäminen** -välilehdessä. 

Tällä hetkellä tarjottu säännöt:

- **Monimenetelmäisen todentamisen edellyttävät**
 - Kaikki käyttäjät, jotka käytäntö otetaan käyttöön tarvitaan vähintään kerran monimenetelmäisen todentamisen kautta tarkistamiseen.
 
- **Vaadi monimenetelmäisen todentamisen kun ei töissä**
 - Jos tämä käytäntö otetaan käyttöön, kaikki käyttäjät on suoritettava olet suorittanut monimenetelmäisen todentamisen vähintään kerran, jos ne käyttää palvelua etäkohteeseen vapaa-aika. Jos ne siirretään työ etäkohteeseen, ne edellyttää suorittaa monimenetelmäisen todentamisen palvelua käytettäessä.
 
- **Kun ei töissä käytön estäminen** 
 - Kun käyttäjät siirtyvät työn etäkohteeseen, ne estetään, jos "Kun ei töissä käytön estäminen"-käytäntö otetaan käyttöön niitä.  Ne uudelleen käyttöoikeuksien salliminen työn sijainti osoitteessa.


## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita

- [Office 365: een ja muiden sovellusten käytön yhteydessä Azure Active Directory](active-directory-conditional-access.md)
- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
