<properties
    pageTitle="Microsoft todentaja-sovelluksen usein kysytyt kysymykset"
    description="On usein kysyttyjä kysymyksiä ja vastauksia Microsoft Authentication-sovellus ja Azure Monimenetelmäisen todentamisen liittyviä luettelo."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar, librown"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator-application-faq"></a>Microsoft Authenticator sovelluksen usein kysytyt kysymykset

Microsoft Authenticator-sovellus korvataan Azure todentaja-sovellus ja ei suositella sovellus, kun käytät Azure Monimenetelmäisen todentamisen. Tätä sovellusta ei ole käytettävissä Windows Phone-, Android- ja iOS.

## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

- **Mitä on tapahtunut Azure todentaja, Multi-factor Auth ja Microsoft-tilin sovelluksia?**

    Microsoft Authenticator sovellus korvaa toisiinsa nämä sovellukset. Päivittää Microsoft Authenticator Azure todentajaa. Jos käytät multi-factor Auth tai Microsoft-tilin sovelluksia, asenna Microsoft Authenticator ja lisää tilisi uudelleen. Varmista, että Viimeistele lisääminen tileiltäsi uuden sovelluksen ennen poistamista vanhojen.

- **Käytän jo Authenticator Microsoft-sovelluksen vahvistus-koodien. Miten vaihdan yhdellä napsautuksella push-ilmoitukset?**  

    Hyväksymisestä Kirjaudu sisään push-ilmoituksen kautta on käytettävissä vain Microsoft-tilit, ei kolmansien osapuolten-tili, kuten Google tai Facebookista. Työpaikan tai oppilaitoksen Microsoft-tili organisaation voit valita tämän asetuksen, vaikka käytöstä.

    Jos käytät Microsoft-tiliä henkilökohtaisen tilisi ja haluat siirtyä push-ilmoitukset, haluat lisätä tilin uudelleen. Rekisteröi uudelleen tilisi laitteen ja push-ilmoitusten määrittäminen.  

    Jos tiliä ei ole kaksivaiheinen vahvistus on otettu käyttöön, on kohdassa [tietoja kaksivaiheista vahvistusta varten](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) , Määritä, onko juuri sinulle.  

- **Milloin minulla on käyttää Iphonessa tai iPadissa yhdellä napsautuksella push-ilmoitukset?**  

    Tämä ominaisuus on Beta elokuussa, kun se on laajasti käytettävissä Microsoft-tilien loppuun saakka. Voit halutessasi beeta-kehittämisohjelmaan lähettänyt sähköpostia msauthenticator@microsoft.com. Etunimi, Sukunimi ja Apple ID sisällytettävä viesti.  

- **Toimivatko-Microsoft tilien yhdellä napsautuksella push-ilmoitukset?**  

    Ei, push-ilmoitukset toimivat vain Microsoft ja Azure Active Directory-tilit. Jos työpaikan tai oppilaitoksen käyttää Azure AD-tilejä, he voivat poistaa tämän toiminnon käytöstä.  

- **Voin varmuuskopiosta laitteeseeni ja Oma tili-koodien puuttuu tai ei toimi. Mitä tapahtui?**  

    Tietoturvasyistä olemme ei tilit varmuuskopioista palauttaminen sovelluksen. IOS-sovellukseen palauttaminen varmuuskopiosta, jos asiakkaiden näkyvät edelleen, mutta ne eivät toimi joka voi vastaanottaa kirjautumisen tarkastukset tai luo suojauksen koodit. Kun sovellus palauttaa, poista tilit ja lisää ne uudelleen.

- **Sain uusi laite. Miten vanha laitteesta Microsoft Authenticator poistaminen ja siirtäminen uuteen?**

    Microsoft Authenticator laajennussovellus lisätään uusi laite ei automaattisesti poista sitä muilla laitteilla. Jos haluat hallita, mitkä laitteet on määritetty tilissäsi, sivustossa saman, jonka avulla voit hallita kaksivaiheista vahvistusta varten ja halutessaan poistaa vanhoja sovelluksia.

    Tämä sivusto on henkilökohtainen Microsoft-tili, [tilin suojaus](https://account.microsoft.com/security) -sivu. Työpaikan tai oppilaitoksen Microsoft-tili voi olla [Omat sovellukset](https://myapps.microsoft.com) - tai mukautetun portal organisaatiosi on määrittänyt tämän sivuston.

## <a name="contact-us"></a>Yhteystiedot

Jos kysymykseesi ei vastata tätä, jätä kommentin sivun alareunassa. Tai kun emme voi [Ota yhteyttä tukeen](https://support.microsoft.com/contactus) ja olemme vastaa ongelmasi.

Jos otat yhteyttä tukeen, Lisää mahdollisimman paljon seuraavat tiedot, kuten voit tehdä seuraavia toimia:

- **Käyttäjätunnus** – mikä on sähköpostiosoite, olet yrittänyt kirjautua sisään?
- **Yleinen virhe kuvaus** – mitä tarkka virhesanoma näkyy voit nähdä?  Jos oli virhesanomaa, kuvaavat odottamatonta toimintaa huomannut, yksityiskohtaiset tiedot.
- **Sivun** – sivun käytitkö, kun näyttöön tuli virheen (sisältää URL-osoite)?
- **Virhekoodi** - virhekoodi tulee perille.
- **Istunto** - vastaanotat tietyn istunnon tunnus.
- **Korrelaatiotunnuksen** – mikä oli luodaan, kun käyttäjä tuli virheen korrelaatio-tunnuskoodi.
- **Aikaleima** – mikä oli tarkka päivämäärä ja kellonaika, näyttöön tuli virheen (sisältää aikavyöhyke)?

Monet näistä löytyy Kirjaudu sisään-sivulla. Kun tarkistat ei oman Kirjautumisvirheitä aiheuttavien samanaikaisesti, valitse **Näytä tiedot**.

![Kirjaudu sisään virheen yksityiskohtaiset tiedot](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Tietojen auttaa meitä Ratkaise ongelma mahdollisimman pian.

## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita

- [Azure Monimenetelmäisen todentamisen usein kysytyt kysymykset](multi-factor-authentication-faq.md)  
- [Tietoja kaksivaiheinen vahvistus on](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) Microsoft-tilien
- [Kaksivaiheinen vahvistus on ongelmia?](multi-factor-authentication-end-user-troubleshoot.md)
