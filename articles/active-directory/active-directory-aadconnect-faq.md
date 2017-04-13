<properties
    pageTitle="Azure AD Connect: Usein kysytyt kysymykset | Microsoft Azure"
    description="Tämä sivu on usein kysyttyjä kysymyksiä Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Azure AD Connect usein kysytyt kysymykset

## <a name="general-installation"></a>Yleiset asennus
**K: Jos Azure AD-Yleinen järjestelmänvalvoja on ottanut käyttöön 2FA toimii asennuksen?**  
Helmikuussa 2016 versiot ja tämä on tuettu.

**K: onko voi asentaa Azure AD Connect automaattisen?**  
Se on tuettu vain asentaminen Azure AD Connect ohjatun asennuksen avulla. Automaattisen ja hiljainen asennus ei tueta.

**Kysymys: saan on metsää, jossa yksi toimialue ei saa yhteyttä. Miten voin asentaa Azure AD Connect?**  
Helmikuussa 2016 versiot ja tämä on tuettu.

**K: AD DS: N kunto-agentti käsitellä server core?**  
Kyllä. Asennettuasi agentti voi suorittaa käyttämällä PowerShell-komentosovelmalla rekisteröintiprosessi loppuun: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Verkon
**Kysymys: saan on palomuuri, verkon tai jotain muuta, joka rajoittaa enimmäisajan yhteydet pysyvät Avaa verkossa. Kuinka kauan Omat asiakkaan puoli aikakatkaisu raja-arvo olisi Azure AD Connect käytettäessä?**  
Kaikki verkko-ohjelmisto, fyysisten laitteiden tai muu, joka rajoittaa yhteydet voivat olla Avaa enimmäisajan käytettävä raja-arvon on oltava vähintään viiteen minuuttiin (300 sekuntia) palvelimen, johon on asennettu Azure AD Connect-asiakasohjelman ja Azure Active Directory väliset yhteydet. Tämä koskee myös kaikki aiemmin julkaistut Microsoft Identity synkronoinnin työkalut.

**K: ovat SLDs (yksittäinen tarra toimialueita) tukee?**  
Ei, Azure AD Connect ei tue paikallisen metsien/toimialueita SLDs avulla.

**K: ovat "pisteviiva"-niminen NetBios tukee?**  
Ei, Azure AD Connect ei tue paikallisen metsien/toimialueita missä NetBios nimi sisältää piste-. "nimessä.

## <a name="federation"></a>Liitetty viestintä
**K: Mitä teen, jos saada sähköpostiviestin, joka pyytää minua uusi Office 365-sertifikaatin**  
Käytä ohjeisiin, jotka on ääriviivat uusiminen varmenteen [uusia varmenteita](active-directory-aadconnect-o365-certs.md) aiheesta.

**Kysymys: saan on "Päivittää automaattisesti varmenteen käyttäjän osapuolen" O365 varmenteen käyttäjän osapuolen määrittäminen. Tarvitsen suorittavan toiminnon, kun oma tunnuksen allekirjoitusvarmenne automaattisesti palauttaa?**  
Käytä ohjeisiin, jotka on kuvattu artikkelissa [uusia varmenteita](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Ympäristön
**K: onko se tukee nimeä palvelimen Azure AD Connect asentamisen jälkeen?**  
Ei. Palvelimen nimen muuttaminen aiheuttaa synkronointi-ohjelma ei voi muodostaa yhteyden SQL-tietokantaan ja palvelun voi käynnistää.

## <a name="identity-data"></a>Käyttäjätiedot
**Azure AD-kysymys UPN (userPrincipalName)-määrite ei vastaa paikallinen - UPN miksi?**  
Seuraavissa artikkeleissa:

- [Office 365: ssä, Azure tai Intune käyttäjänimet eivät vastaa paikallisen täydellisen Käyttäjätunnuksen tai vaihtoehtoinen Käyttäjätunnus](https://support.microsoft.com/en-us/kb/2523192)
- [Muutokset eivät ole synkronoitu Azure Active Directory-synkronointi-työkalun avulla, kun olet muuttanut käyttäjätilin täydellisen Käyttäjätunnuksen käyttämään eri liitetyssä toimialueessa](https://support.microsoft.com/en-us/kb/2669550)

Voit myös määrittää Azure AD sallimaan päivittäminen userPrincipalName kuvatulla tavalla [Azure AD Connect synkronointiominaisuudet](active-directory-aadconnectsyncservice-features.md)synkronointi-ohjelma.

## <a name="custom-configuration"></a>Mukautettu määritys
**K: missä Azure AD Connect PowerShell cmdlet-komennoista on kuvattu?**  
Lukuun ottamatta kuvattu tämän sivuston cmdlet-komennot muut PowerShellin cmdlet-komennot-kohdan Azure AD Connect ei tue asiakkaiden.

* *K: Voinko käyttää "Vie/palvelinten tuo" löytyi *synkronoinnin palvelun hallinnan* siirtäminen määritysten välillä servers? **  
Ei. Tämä vaihtoehto ei ole noutaa kaikki asetukset ja ei voi käyttää. Ohjatun toiminnon avulla kannattaa sen sijaan luominen peruskokoonpano toisen palvelimeen ja synkronoi säännön editorin avulla voit luoda PowerShell-komentosarjojen, voit siirtää minkä tahansa mukautetun säännön palvelinten välillä. Katso, [Siirrä Mukautettu määritys aktiivisen väliaikaisen palvelimeen](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Vianmääritys
**K: Miten voin hankkia Azure AD Connect apua?**

[Etsi Microsoft Knowledge Base (kt)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- Etsi Microsoft Knowledge Base (KB), teknisiä yleisimpien ongelmatilanteiden ratkaisuista korjauksista tietoja Azure AD Connect tuesta.

[Microsoft Azure Active Directory-keskustelupalstat](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- Voit etsiä ja selata tekniset kysymyksiä ja vastauksia yhteisöstä tai kirjoita oma kysymyksesi napsauttamalla [tätä](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD Connect-asiakastuki](https://manage.windowsazure.com/?getsupport=true)

- Tämän linkin avulla voit ottaa yhteyttä tukeen palvelun Azure-portaalissa.
