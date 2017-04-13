<properties
    pageTitle="Vianmääritys sovelluksen välityspalvelimen | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Azure AD-sovelluksen välityspalvelimen vianmääritys."
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
    ms.date="08/19/2016"
    ms.author="kgremban"/>



# <a name="troubleshoot-application-proxy"></a>Vianmääritys sovelluksen välityspalvelin

Jos virheitä ilmenee käytettäessä julkaistu sovellus tai julkaisun sovelluksissa, tarkista seuraavat vaihtoehdot toimiiko Microsoft Azure AD-sovelluksen välityspalvelimen oikein:

- Avaa Windows Services konsoli ja varmista, että **Microsoft AAD sovelluksen välityspalvelimen Connector** -palvelu on käytössä ja käynnissä. Voit myös kannattaa tarkastella sovelluksen välityspalvelimen palvelun ominaisuudet-sivulla seuraavassa kuvassa esitetyllä tavalla:  
  ![Microsoft AAD sovelluksen välityspalvelimen yhdistimen ominaisuudet-ikkunan kuva](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)

- Avaa Tapahtumienvalvonta ja Etsi sovellus välityspalvelimen yhdistimen tapahtumat **sovellukset- ja palvelulokit** > **Microsoft** > **AadApplicationProxy** > **yhdistimen** > **järjestelmänvalvoja**.
- Tarvittaessa yksityiskohtaisia lokeja ovat käytettävissä ottamalla analyysin ja virheenkorjaus lokit ja sovelluksen välityspalvelimen yhdistimen istunnon lokissa ottaminen käyttöön.

## <a name="the-page-is-not-rendered-correctly"></a>Sivun ei käsitellä oikein

Virhesanomaa ei kuulu, ongelmat saattavat jatkuu sovelluksen värien tai toimi oikein. Näin voi käydä, jos artikkelissa polku on julkaistu, mutta sovellus edellyttää sisällön polun ulkopuolella oleviin.

Esimerkiksi jos julkaiset polku https://yourapp/app, mutta sovellus kutsuu kuvia https://yourapp/media, he eivät voi muodostaa. Varmista, että julkaiset sovelluksen sinun täytyy lisätä tarvittavassa sisällössä ylin taso polku. Tässä esimerkissä olisi http://yourapp/.

Jos muutat viitatun sisältöä on kuitenkin yhä tarkempaa linkin polusta purkaa käyttäjien polusta, seuraavassa blogimerkinnässä [asetus oikea linkki sovelluksen välityspalvelimen sovellusten Azure AD access-ruudusta ja Office 365-sovellusten käynnistimeen](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="general-errors"></a>Yleiset virheet

Virhe | Kuvaus | Tarkkuus
--- | --- | ---
Yrityksen sovelluksen ei voi käyttää. Ei ole oikeutta käyttää tätä sovellusta. Todennus epäonnistui. Varmista, että voit määrittää käyttäjän käytön tämän sovelluksen. | Voit ehkä ole määritetty tämän sovelluksen käyttäjä. | Valitse **sovellus** -välilehteen ja valitse **käyttäjät ja ryhmät**-tämän käyttäjän tai ryhmän määrittää tämän sovelluksen.
Yrityksen sovelluksen ei voi käyttää. Ei ole oikeutta käyttää tätä sovellusta. Todennus epäonnistui. Varmista, että käyttäjällä on käyttöoikeuden Azure Active Directory-Premium tai Basic. | Käyttäjät voivat nähdä tämän virheen, kun yrität käyttää sovellusta, jos niitä ei ole nimenomaisesti määritetty Premium/Basic käyttöoikeuksin tilaajan järjestelmänvalvoja on julkaistu. | Avaa tilaajan Active Directory- **käyttöoikeudet** -välilehti ja varmista, että tämän käyttäjän tai ryhmän määritetään Premium tai Basic käyttöoikeuden.


## <a name="connector-troubleshooting"></a>Yhdistimen vianmääritys
Jos rekisteröinti epäonnistuu yhdistimen ohjatun asennuksen aikana, sinun on kaksi tapaa tarkastella virheen syy. Etsi **sovelluksia**ja palveluja Logs\Microsoft\AadApplicationProxy\Connector\Admin tapahtumalokiin tai suorittaa seuraavan Windows PowerShell-komennon.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| Virhe | Kuvaus | Tarkkuus |
| --- | --- | --- |
| Yhdistimen rekisteröinti epäonnistui: Varmista, että käytössä sovelluksen välityspalvelimen Azure hallinta-portaalissa ja että kirjoitit Active Directory-käyttäjänimen ja salasanan oikein. Virhe: "vähintään yksi virhe." | Azure AD kirjautuessasi suorittamatta suljettu rekisteröinti-ikkunassa. | Suorita ohjattu yhdistin uudelleen ja rekisteröi Connector. |
| Yhdistimen rekisteröinti epäonnistui: Varmista, että käytössä sovelluksen välityspalvelimen Azure hallinta-portaalissa ja että kirjoitit Active Directory-käyttäjänimen ja salasanan oikein. Virhe: "AADSTS50001: resurssin `https://proxy.cloudwebappproxy.net /registerapp` on poistettu käytöstä." | Sovelluksen välityspalvelin on poistettu käytöstä.  | Varmista, että otat sovelluksen välityspalvelimen Azure perinteinen portaalissa ennen kuin yrität rekisteröidä yhdistin. Katso lisätietoja sovelluksen välityspalvelimen ottamisesta käyttöön, [ottaa käyttöön sovelluksen välityspalvelinta](active-directory-application-proxy-enable.md). |
| Yhdistimen rekisteröinti epäonnistui: Varmista, että käytössä sovelluksen välityspalvelimen Azure hallinta-portaalissa ja että kirjoitit Active Directory-käyttäjänimen ja salasanan oikein. Virhe: "vähintään yksi virhe." | Jos rekisteröinti-ikkuna avautuu ja sulkeutuu heti ilman, että voit kirjautua sisään, näyttöön tulee todennäköisesti tämä virhe. Tämä virhe ilmenee, kun kyseessä on verkko virhe järjestelmään. | Varmista, että ei voi muodostaa selaimesta julkiseen sivustoon ja portit ovat avoinna [sovelluksen välityspalvelimen edellytykset](active-directory-application-proxy-enable.md)mukaisesti. |
| Yhdistimen rekisteröinti epäonnistui: Varmista, että tietokone on yhteydessä Internetiin. Virhe: "ilmeni päätepistettä seuraa kohteessa `https://connector.msappproxy.net :9090/register/RegisterConnector` , joka voi hyväksyä viestin. Syynä on usein virheellinen osoite tai SOAP-toiminto. Katso InnerException, jos se on valittavissa lisätietoja. " | Kirjaudu sisään käyttämällä Azure AD-käyttäjänimi ja salasana, mutta tämä virhesanoma, jos lähettää kaikki portit 8081 yläpuolella on estetty. | Varmista, että tarvittavat portit ovat avoinna. Lisätietoja on artikkelissa [sovelluksen välityspalvelimen edellytykset](active-directory-application-proxy-enable.md). |
| Poista virhe esitetään rekisteröinti-ikkunassa. Ei voi jatkaa – vain, jos haluat sulkea ikkunan. | Antamasi Väärä käyttäjänimi tai salasana. | Yritä uudestaan. |
| Yhdistimen rekisteröinti epäonnistui: Varmista, että käytössä sovelluksen välityspalvelimen Azure hallinta-portaalissa ja että kirjoitit Active Directory-käyttäjänimen ja salasanan oikein. Virhe: "AADSTS50059: vuokraajan tunnistaminen tietoja ei löydy joko pyynnön tai annettu tunnistetietoja ilmaisemalla ja Etsi palvelun mukainen URI epäonnistui. | Yrität kirjautua sisään käyttämällä Microsoft-Account ja eivät kuuluu toimialueeseen, joka on osa, jota yrität käyttää hakemiston Organisaatiotunnus. | Varmista, että järjestelmänvalvoja on osa on sama kuin vuokraajan toimialue toimialuenimi, esimerkiksi jos Azure AD-toimialueen contoso.com, järjestelmänvalvojan on oltava admin@contoso.com. |
| Ei voitu hakea nykyisen suorittamisen käytännön PowerShell-komentosarjojen suorittamisen. | Jos Connector-asennus epäonnistuu, tarkista, varmista, että PowerShell suorittamisen käytäntö ei ole käytössä. | Avaa ryhmäkäytännön editori. Siirry **Tietokoneen määritykset** > **Järjestelmänvalvojan malleja** > **Windowsin osien** > **Windows PowerShellin** ja kaksoisnapsauta **komentosarjojen suorittamisen käyttöön**. Tämän voi määrittää **Ei ole määritetty** tai **käytössä**. Jos **käytössä**, varmista, että asetukset-kohdassa suorittamisen käytäntö on määritetty joko **paikallisen komentosarjojen ja remote allekirjoitetun komentosarjoja** tai **kaikki**komentosarjoja. |
| Yhdistimen määritykset lataaminen ei onnistu. | Yhdistimen Asiakasvarmenne, jota käytetään todennusta varten, on vanhentunut. Tämä saattaa ilmetä myös, jos sinulla on asennettu välityspalvelimen takana yhdistin. Tässä tapauksessa yhdistimen ei voi käyttää Internetiä ja voi säätää etäkäyttäjien sovellukset. | Uusia luottamuksen manuaalisesti `Register-AppProxyConnector` Windows PowerShellin cmdlet-komento. Jos sitten yhdistimen takana välityspalvelinta, se on myönnettävä Internet-yhteyttä yhdistimen tilit "verkkopalveluita" ja "Paikallinen järjestelmä." Tämä onnistuu, ne käyttöoikeuksien myöntämistä välityspalvelimen tai määrittämällä ne voi ohittaa välityspalvelimen. |
| Yhdistimen rekisteröinti epäonnistui: Varmista, että olet Yleinen järjestelmänvalvoja, voit rekisteröidä yhdistimen Active Directory. Virhe: "rekisteröinti hylättiin." | Yrität kirjautua sisään alias ei ole tämän toimialueen järjestelmänvalvoja. Sitten yhdistimen aina asennetaan kansioon, joka omistaa käyttäjän toimialue. | Varmista, että järjestelmänvalvojan rooliin sisäänkirjautuminen on yleinen Azure AD-vuokraajan käyttöoikeudet.|


## <a name="kerberos-errors"></a>Kerberos-virheet

| Virhe | Kuvaus | Tarkkuus |
| --- | --- | --- |
| Ei voitu hakea nykyisen suorittamisen käytännön PowerShell-komentosarjojen suorittamisen. | Jos Connector-asennus epäonnistuu, tarkista, varmista, että PowerShell suorittamisen käytäntö ei ole käytössä. | Avaa ryhmäkäytännön editori. Siirry **Tietokoneen määritykset** > **Järjestelmänvalvojan malleja** > **Windowsin osien** > **Windows PowerShellin** ja kaksoisnapsauta **komentosarjojen suorittamisen käyttöön**. Tämän voi määrittää **Ei ole määritetty** tai **käytössä**. Jos **käytössä**, varmista, että asetukset-kohdassa suorittamisen käytäntö on määritetty joko **paikallisen komentosarjojen ja remote allekirjoitetun komentosarjoja** tai **kaikki**komentosarjoja. |
| 12008 - azure AD ylitetty sallitut Kerberos todennus yrittää yhteyttä palvelimeen. | Tapahtuman saattaa johtua virheellisistä määrityksistä Azure AD välillä ja sovelluksen palvelimeen tai päivämäärän ja kellonajan määrityksessä molemmissa tietokoneissa ongelma. | Yhteyttä palvelimeen hylätty Azure AD luoma Kerberos-lippu. Varmista, että Azure AD ja sovelluksen palvelimeen on määritetty oikein. Varmista, että Azure AD-päivämäärä ja kellonaika-määritys ja sovelluksen palvelimeen on synkronoitu. |
| 13016 - azure AD ei voi hakea Kerberos-lippu käyttäjän puolesta, koska ei ole UPN reuna-tunnuksen tai access-eväste. | On ilmennyt ongelma STS-määritys. | Korjaa STS UPN ilmoituksen määritykset. |
| 13019 - azure AD ei voi hakea Kerberos-lippu käyttäjän puolesta Yleiset API-virheen vuoksi. | Tapahtuman saattaa johtua virheellisistä määrityksistä Azure AD välillä ja toimialueen ohjauskonepalvelimeen tai päivämäärän ja kellonajan määrityksessä molemmissa tietokoneissa ongelma. | Toimialueen ohjauskoneen hylätty Azure AD luoma Kerberos-lippu. Tarkista, että Azure AD ja sovelluksen palvelimeen on määritetty oikein, erityisesti SPN määritykset. Varmista, että Azure AD on yhdistetty samaan toimialueen ohjauskoneen, varmista, että toimialueen ohjauskoneen muodostaa luottamussuhde Azure AD merkitseminen toimialue. Varmista, että Azure AD-päivämäärä ja kellonaika-määritys ja toimialueen ohjauskoneen on synkronoitu. |
| 13020 - azure AD ei voi hakea Kerberos-lippu käyttäjän puolesta, koska SPN yhteyttä palvelimeen ei ole määritetty. | Tapahtuman saattaa johtua virheellisistä määrityksistä Azure AD välillä ja toimialueen ohjauskonepalvelimeen tai päivämäärän ja kellonajan määrityksessä molemmissa tietokoneissa ongelma. | Toimialueen ohjauskoneen hylätty Azure AD luoma Kerberos-lippu. Tarkista, että Azure AD ja sovelluksen palvelimeen on määritetty oikein, erityisesti SPN-määritys. Varmista, että Azure AD on yhdistetty samaan toimialueen ohjauskoneen, varmista, että toimialueen ohjauskoneen muodostaa luottamussuhde Azure AD merkitseminen toimialue. Varmista, että päivämäärä ja kellonaika-määritysten Azure AD- ja toimialueen ohjauskoneen on synkronoitu. |
| 13022 - azure AD ei voi todentaa käyttäjän, koska yhteyttä palvelimeen reagoi Kerberos-todennus yritykset HTTP 401-virheen. | Tapahtuman saattaa johtua virheellisistä määrityksistä Azure AD välillä ja sovelluksen palvelimeen tai päivämäärän ja kellonajan määrityksessä molemmissa tietokoneissa ongelma. | Yhteyttä palvelimeen hylätty Azure AD luoma Kerberos-lippu. Varmista, että Azure AD ja sovelluksen palvelimeen on määritetty oikein. Varmista, että Azure AD-päivämäärä ja kellonaika-määritys ja sovelluksen palvelimeen on synkronoitu. |
| Sivuston eivät näy sivulla. | Käyttäjän voi tulla seuraava virhesanoma, kun yrität käyttää sovellusta, jos sovellus on IWA-sovellus on julkaistu. Tämän sovelluksen määritetyn ‑objektin voi olla väärä. | IWA sovellusten: Varmista, että määritetty tämän sovelluksen SPN ovat oikein. |
| Sivuston eivät näy sivulla. | Käyttäjän voi tulla seuraava virhesanoma, kun yrität käyttää sovellusta, jos sovellus on OWA-sovellus on julkaistu. Tämä voi johtua seuraavista: <br> -Sovelluksen määritetyn ‑objektin on virheellinen. <br> -Käyttäjä, joka ei voi käyttää sovelluksen käyttää Microsoft-tilin sijaan ERISNIMI Yritystili kirjautuminen tai käyttäjä on Vieras-käyttäjän. <br> -Käyttäjä, joka ei voi käyttää sovellusta ei ole määritetty oikein tämän sovelluksen paikallinen reunassa. | Pienentämään vastaavasti vaiheet: <br> -Varmista että määritetty tämän sovelluksen SPN on oikein. <br> -Varmista, että käyttäjä kirjautuu sisään käyttämällä niiden yritystili, joka vastaa julkaistu sovellus toimialueen. Microsoft-Account käyttäjät ja Vierailijan ei voi käyttää IWA sovellukset. <br> -Varmista Varmista, että käyttäjälle on tarvittavat käyttöoikeudet Taustajärjestelmä sovelluksen Paikallinen kone-määritysten mukaisesti. |
| Yrityksen sovelluksen ei voi käyttää. Ei ole oikeutta käyttää tätä sovellusta. Todennus epäonnistui. Varmista, että voit määrittää käyttäjän käytön tämän sovelluksen. | Käyttäjät voivat nähdä tämän virheen, kun yrität käyttää sovellusta, jos niissä käytetään Microsoft-tilien sen sijaan, että niiden Yritystili kirjautuminen julkaistu. Vierailevien käyttäjien myös voi tulla seuraava virhesanoma: | Microsoft-Account käyttäjät ja vieraiden Eikö IWA sovellukset. Varmista, että käyttäjä kirjautuu sisään käyttämällä niiden yritystili, joka vastaa julkaistu sovellus toimialueen. |
| Yrityksen sovelluksen ei onnistu juuri nyt. Yritä uudelleen myöhemmin... Yhdistimen aikakatkaisu. | Käyttäjät voivat nähdä tämän virheen, kun yrität käyttää sovellusta, jos niitä ei ole oikein määritetty tämän sovelluksen paikallinen reunassa on julkaistu. | Varmista, että käyttäjillä on tarvittavat käyttöoikeudet Taustajärjestelmä sovelluksen Paikallinen kone-määritysten mukaisesti. |


## <a name="see-also"></a>Katso myös

- [Ottaa käyttöön sovelluksen välityspalvelinta Azure Active Directory](active-directory-application-proxy-enable.md)
- [Sovelluksen välityspalvelimen sovellusten julkaiseminen](active-directory-application-proxy-publish.md)
- [Ottaa käyttöön kertakirjautumisen](active-directory-application-proxy-sso-using-kcd.md)
- [Ehdollinen käytön käyttöön ottaminen](active-directory-application-proxy-conditional-access.md)

Uusimmat uutiset ja päivitykset Tutustu [sovelluksen välityspalvelin-blogi](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
