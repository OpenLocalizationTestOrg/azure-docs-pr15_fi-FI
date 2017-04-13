<properties
    pageTitle="Azure AD Connect ja Federation | Microsoft Azure"
    description="Tämä sivu on paikka, AD FS-toimintoja käyttämällä Azure AD Connect koskevat kaikki asiakirjat"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect ja liittäminen

Azure AD Connect voit ja paikallisen Liittoutuminen määrittäminen AD FS ja Azure AD. Voit ottaa federation etumerkin, käyttäjät voivat kirjautua Azure AD-pohjaisten palvelujen paikallisen salasanansa ja yrityksen verkkoon, valitse tarvitsematta kirjoittaa salasanansa uudelleen. AD FS federation-asetuksen avulla voit ottaa käyttöön uuden tai määrittää aiemmin AD FS-Liittoutumispalvelut Windows Server 2012 R2-klusterin.

Tässä artikkelissa on lisätietoja Federation aloitus liittyviä toimintoja Azure AD Connect ja luetteloiden linkkejä muiden siihen liittyviä ohjeita. Saat linkkejä Azure AD Connect Azure Active Directory-integraation paikallisen käyttäjätietojen.

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect - federation aiheita

| Aihe | Se peittää ja milloin lukeminen |
|:------|:-----------|
| **Azure AD Connect-kirjautuminen käyttäjäasetusten** ||
| [Tietoja käyttäjäasetusten-kirjautuminen](active-directory-aadconnect-user-signin.md) | Eri käyttäjän-asetusten kuvaus ja miten ne vaikuttavat Azure-kirjautuminen käyttöympäristön kuvaus |
| **AD FS-asennuksen avulla Azure AD Connect**||
| [Vaatimukset](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Vaatimat AD FS asennusta Azure AD Connect kautta|
| [AD FS-klusterin määrittäminen](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Uuden AD FS-klusterin käyttämällä Azure AD Connect asentaminen |
| **AD FS-määritysten muokkaaminen** | |
| [Luota korjaaminen](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Nykyinen välinen luottamussuhde korjaamisesta paikallisen AD FS- ja Office 365: n / Azure |
| [Uusi ADFS-palvelimen lisääminen](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | AD FS-klusteriin on muita AD FS palvelimen kirjaa ensimmäisen asennuksen laajentaminen |
| [Uusi AD FS WAP-palvelimen lisääminen](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | AD FS-klusteriin on muita WAP palvelimen kirjaa ensimmäisen asennuksen laajentaminen |
| [Lisää uusi liitetyssä toimialueessa](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Voit olla äänipuheluita Azure AD toisen toimialueen lisäämistä |
|**Kirjaa asennuksen tehtävät**||
| [Lisää mukautettu yrityksen logo tai kuva](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| Muokkaa kirjautuminen määrittämällä mukautetun logon, joka näytetään sivun AD FS-kirjautuminen |
| [Lisää kuvaus-kirjautuminen](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Kirjaudu sisään kuvaus AD FS-kirjautumissivulla muuttaminen | 
| [Muokkaaminen AD FS ottaa säännöt](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Muokkaa / vaatimus sääntöjen lisääminen AD FS vastaavat Azure AD Connect synkronoinnin määritys |


## <a name="additional-resources"></a>Lisäresursseja

* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)
* [Azure AD FS käyttöönottoa](active-directory-aadconnect-azure-adfs.md)
* [Suuren käytettävyyden rajat maantieteelliset AD FS käyttöönoton Azure-tietokannassa Azure liikenteen hallinta](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


