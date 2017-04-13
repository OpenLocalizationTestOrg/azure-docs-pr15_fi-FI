<properties
    pageTitle="Käyttäjätietojen ilman salasanan vaihtamiseen Microsoft Passport todennustapa | Microsoft Azure"
    description="Tässä artikkelissa on yleiskatsaus Microsoft Passport ja muita tietoja Microsoft Passport ottamisesta käyttöön."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>Käyttäjätietojen ilman salasanan vaihtamiseen Microsoft Passport todennustapa

Todentaminen salasanat pelkän nykyisen menetelmät eivät riitä lukemaan käyttäjät. Käyttäjien uudelleen ja unohdat salasanan. Salasanoissa breachable, phishable voi enää halkeamia ja guessable. He saavat myös muista on vaikeaa ja voi enää kalastelu, kuten "[välittää hajautuksen](https://technet.microsoft.com/dn785092.aspx)".

## <a name="about-microsoft-passport"></a>Tietoja Microsoft Passportista
Microsoft Passport on yksityinen ja julkinen avain tai varmenteen lähestymistapa organisaatiot ja käyttäjät, jotka ulottuvat salasanat. Tähän lomakkeeseen todennus on riippuvainen avaimen pari tunnistetiedot, jotka voit korvata salasanat ja ilmeisesti, thefts ja tietojen kalastelu ja.

 Microsoft Passport avulla käyttäjä todennetaan Microsoft-tiliä, Windows Server Active Directory-tilin, Microsoft Azure Active Directory (Azure AD)-tiliä tai-Microsoft palvelu, joka tukee nopeaa käyttäjätietoja Online (FIDO) todennusta. Alkuperäinen kaksivaiheinen vahvistus, Microsoft Passport rekisteröitymisen yhteydessä, kun käyttäjän laitteessa on määritetty Microsoft Passport ja käyttäjä voi määrittää ele, joka voi olla Windows Hei tai PIN-tunnuksen. Käyttäjän on liikkeen ja Vahvista jäsenyys. Windows käyttää sitten Microsoft Passport todennetaan ja he voivat käyttää suojattuja resursseja ja palveluja.

Yksityinen avain on saatavilla ainoastaan "käyttäjän liikkeen" kuten PIN-tunnuksen, biometria tai etälaitteen, kuten, jota käyttäjä käyttää laitteen kirjautuminen älykortin avulla. Nämä tiedot on linkitetty varmenteen tai asymmetrisen avaimen pari. Yksityinen avain on laitteiston suoritettavat, jos laitteessa on luotettu Platform Module (TPM)-suoritin. Yksityinen avain ei koskaan laite.

Julkinen avain on rekisteröity Azure Active Directory-ja Windows Server Active Directory (paikallisen). Tunnistetietojen toimittajat (IDPs) tarkistaa käyttäjän yhdistämällä julkinen avain käyttäjän yksityinen avain ja anna Kirjaudu sisään tiedot OTP-salasanan (yhden kerran Portaalin), PhoneFactor tai järjestelmä eri ilmoitus, jonka kautta.

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>Miksi yritykset käyttävät Microsoft Passport

Ottamalla Microsoft Passport yrityksille voi tehdä resursseille entistä turvallisempi mukaan:

* Määrittäminen Microsoft Passport laitteiston ensisijainen-vaihtoehto. Tämä tarkoittaa, että avaimet luodaan TPM 1.2 tai TPM 2.0, kun niitä on saatavilla. Kun TPM ei ole käytettävissä, ohjelmiston Luo avain.

* PIN-koodi, pituus ja monimutkaisuus määrittäminen ja onko esittelyssä käyttö on otettu käyttöön organisaatiossa.

* Määritä Microsoft Passportin tue älykortin kaltaisessa skenaarioita käyttämällä sertifikaattiin perustuva luota.

## <a name="how-microsoft-passport-works"></a>Microsoft Passport toiminta
1. Näppäimet luodaan laitteistoon TPM tai ohjelmisto. Useiden laitteiden on valmiita TPM-suoritin, joka suojaa laitteiston mukaan salausavaimet integroiminen laitteet. TPM 1.2 tai TPM 2.0 Luo avaimet tai varmenteet, jotka on luotu luotu näppäimet.

2. TPM todistaa laitteiston sidottujen painettavat näppäimet.

3. Yksittäisen unlock liikkeen lukitus laite. Liike sallii useiden resurssien käytön Jos laite on toimialueeseen liittymistä tai Azure AD liittyneet.

## <a name="how-the-microsoft-passport-lifecycle-works"></a>Microsoft Passport elinkaari toiminta

![Microsoft Passport elinkaari](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Edellisen kaaviossa on kuvattu yksityinen/julkisen avaimen pari ja perusteella tunnistetietojen toimittaja. Kunkin nämä vaiheet selitetään seuraavat tiedot:

1. Käyttäjän todista lisätoimenpiteisiin jäsenyyden kautta useita valmiita oikeinkirjoituksen tarkistuksen menetelmiä (liikkeitä, fyysinen älykortit, Multi-factor authentication) ja lähettää nämä tiedot, tunnistetietojen toimittaja (IDP) kuten Azure Active Directory tai paikallisen Active Directory.

2. Laitteen sitten Luo avain, todistaa avaimen, kestää avaimeen julkisen osan, liittää sen aseman lauseita, kirjautuu ja lähettää sen IDP Rekisteröi sitten-näppäintä.

4. Heti, kun IDP Rekisteröi avaimen julkisen osan, IDP haastaa käyttäjäavainten yksityisten aikaosaa kirjautua laitteeseen.

5. Valitse IDP vahvistaa ja ongelmat todennus-tunnuksen, jonka avulla käyttäjä ja laitetta käyttämästä suojattuja resursseja. IDPs voit kirjoittaa Office kaikissa ympäristöissä sovellukset tai selaintuki (joko JavaScript/Webcrypto API) avulla voit luoda ja käyttää Microsoft Passport tunnistetiedot käyttäjilleen.

## <a name="the-deployment-requirements-for-microsoft-passport"></a>Microsoft Passport käyttöönottovaatimukset
### <a name="at-the-enterprise-level"></a>Yrityksen tasolla

* Yrityksen on Azure-tilaus.

### <a name="at-the-user-level"></a>Käyttäjän tasolla

* Käyttäjän tietokoneessa suorittaa Windows 10 Professional- tai Enterprise.

Katso ohjeet yksityiskohtaiset käyttöönoton [Työn organisaation käyttöön Microsoft Passport](active-directory-azureadjoin-passport-deployment.md).


## <a name="additional-information"></a>Lisätietoja

* [Windows 10: n yrityksen: tapoja käyttää laitteet](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liittyä laajentaminen](active-directory-azureadjoin-user-upgrade.md)
* [Lisätietoja käyttötapoja Azure AD liittyminen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä kokemukset](active-directory-azureadjoin-devices-group-policy.md)
* [Määritä Azure AD liittyminen](active-directory-azureadjoin-setup.md)
