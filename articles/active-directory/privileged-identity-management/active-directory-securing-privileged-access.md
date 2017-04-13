<properties
    pageTitle="Sellaisten käytön suojaamisesta Azure AD | Microsoft Azure"
    description="Aihe, jossa kerrotaan tavoista suojaamiseen sellaisten access Azure, Azure Active Directory ja Microsoft Online Services."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>Sellaisten käytön suojaamisesta Azure AD

Voit suojata liiketoiminnan kohteiden Moderni organisaation tärkeä ensimmäinen vaihe on sellaisten käytön. Sellaisten-tilit ovat niitä, jotka hallintaan ja hallita IT-järjestelmien. Cyber hyökkääjät kohde saat käyttöösi organisaation tiedot ja järjestelmien näissä tileissä. Jos haluat suojata sellaisten access-eristämään asiakkaat ja järjestelmien riskistä, joku julkaisemisen.

Lisätietoja käyttäjien mahdollisesti tulee sellaisten access pilvipalveluihin. Tämä voi olla yleinen Office 365-Järjestelmänvalvojat, Azure tilauksen Järjestelmänvalvojat ja käyttäjät, joilla on järjestelmänvalvojan oikeudet VMs tai SaaS sovellukset.

Microsoft suosittelee, noudata näitä ohjeita [suojaaminen sellaisten](https://technet.microsoft.com/library/mt631194.aspx)käyttö.

Azure Active Directoryn avulla voit hallita niiden käyttöä Azure, Office 365: ssä tai muiden Microsoftin palvelujen ja sovellusten asiakkaiden nämä periaatteet koskevat onko käyttäjätilien hallitaan ja todennettu Active Directory tai Azure Active Directoryn. Seuraavissa osissa on Lisätietoja sellaisten käytön tukemaan Azure AD ominaisuuksia.

## <a name="multi-factor-authentication"></a>Monimenetelmäisen todentamisen

Voit parantaa suojausta järjestelmänvalvojan käyttöoikeuden tarvitsevat monimenetelmäisen todentamisen (MFA) ennen oikeuksien myöntämistä. MFA on keinot vahvistusta, joka on edellyttää enemmän kuin vain käyttäjätunnusta ja salasanaa. Se on toisen kerroksen arvopaperin käyttäjän kirjautumiset ja tapahtumia.

Azure Monimenetelmäisen todentamisen auttaa suojaavat käyttämään tietoja ja sovelluksia käyttäjän tarve-kirjautuminen yksinkertaista kokouksen aikana. Se toimittaa vahva todennus helposti vahvistus-asetuksia, kuten puheluita, tekstiviestejä, mobiilisovelluksen ilmoituksia tai tarkistuskoodi ja 3 osapuolen sijasta tunnusten solualueen kautta.

Yleiskatsaus Azure Monimenetelmäisen todentamisen toiminnasta on seuraavassa videossa.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

Lisätietoja on artikkelissa [Office 365: ssä ja MFA Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Aika sidottujen oikeudet

Joissakin organisaatioissa saatat huomata, että heillä on liian monta käyttäjää erittäin sellaisten roolit. Käyttäjä voi on lisätty roolin tietyn toiminnon, kuten rekisteröityä palveluun itse, mutta ei käytä kyseiset oikeudet usein jälkeenpäin.

Pienennä oikeudet näyttäminen, kun ja näkyvyyden kyselyjä rajoittaa käyttöä, rajoita käyttäjien ottamalla vain niiden oikeudet Just-aika Tarpeeseen, kun ne on suoritettava tehtävä. Azure Active Directory-ja Microsoft Online Services voit käyttää [Azure AD sellaisten jäsenyyksien hallinta (PIM)](http://aka.ms/AzurePIM).


![PIM Raporttinäkymät-ikkunan][2]


## <a name="attack-detection"></a>Hyökkäyksen tunnistus

[Azure Active Directory tunnistetietojen suojaus](../active-directory-identityprotection.md) sisältää koottuja näkymän riskin tapahtumien ja mahdolliset heikkouksien, vaikuttavia organisaation käyttäjätietojen. Riskin tapahtumiin perustuvan tunnistetiedot laskee riskin käyttäjätason kunkin käyttäjän kohdalla voit määrittää riskiin perustuva käytäntöjä suojaaminen automaattisesti organisaation tunnistetietoja. Sekä muita ehdollisen käyttöoikeuden ohjausobjekteja Azure Active Directory-ja EMS, kyseiset käytännöt automaattisesti estää käyttäjää tai ehdotuksia, kuten salasanan vaihtamisesta ja monimenetelmäisen todentamisen käyttäminen.

![Azure AD-tunnistetiedot][3]

## <a name="conditional-access"></a>Ehdollisen käyttöoikeuden

Ohjausobjektin ehdollisen käyttöoikeuden kanssa Azure Active Directory tarkistaa tiettyjen ehtojen valitset todennuksessa käyttäjän, ennen kuin sallit access-sovellukseen. Ehtojen täyttyessä, kun käyttäjä on todennus ja sovelluksen käyttöoikeus.


![Asetus ehdollinen käyttösäännöt MFA kanssa][4]


## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Azure Monimenetelmäisen todentamisen](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md) ottaminen käyttöön
- Ota käyttöön [Azure AD-luottamuksellisten jäsenyyksien hallinta](../active-directory-privileged-identity-management-configure.md)
- Ota käyttöön [Azure AD-tunnistetiedot](../active-directory-identityprotection.md)
- [Ehdollinen access-komponenttien](../active-directory-conditional-access.md) ottaminen käyttöön


Lisätietoja rakentaminen valmis suojauksen suunnitelma on kohdassa "asiakkaan vastuut ja ohjeet" [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) asiakirjan. Lisätietoja kiinnostavissa Microsoft services näitä aiheita helpottamiseksi on Microsoftin edustajalta tai tutustu [Cybersecurity ratkaisuja sivun](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
