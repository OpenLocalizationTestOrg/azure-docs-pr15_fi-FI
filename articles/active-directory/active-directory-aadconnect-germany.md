<properties
    pageTitle="Azure AD Connect Microsoftin Saksa"
    description="Azure AD Connect integroida paikallisen kansioiden Azure Active Directory-hakemistosta. Voit antaa yleisiä tunnistetietojen Azure Active Directory-integrointi Office 365: ssä ja Azure SaaS sovellukset."
    keywords="Johdanto voit Azure AD Connect Azure AD Connect yleiskuvaus, mikä on Azure AD Connect asentamalla active Directoryssa, saksa, musta metsää"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect Microsoftin Saksa – julkisen esikatselu

## <a name="introduction"></a>Johdanto
Azure AD Connect on paikallisen Active Directory ja Azure Active Directoryn synkronointia.
Tällä hetkellä monia [Microsoftin](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) Saksan käyttötavoista on tehty toimija. Kun käytät Microsoft Cloud saksa, sinun on otettava huomioon seuraavat:


- Seuraavat URL-osoitteet on voi avata synkronoidaan onnistuneesti välityspalvelimen:
    - *. microsoftonline.de
    - *. windows.net
    - + Kumottujen varmenteiden luettelot

- Kun kirjaudut sisään Azure AD-kansioon, sinun on käytettävä tili onmicrosoft.de toimialue.
- Seuraavat toiminnot eivät ole käytettävissä:
    - Azure AD Connect kunto
    - Automaattiset päivitykset
    - Salasanan takaisinkirjoituksen

## <a name="download"></a>Lataa
Voit ladata Azure AD Connect portaalin Azure AD Connect-sivu.  Alla olevien ohjeiden avulla voit etsiä Azure AD Connect-sivu.

### <a name="the-azure-ad-connect-blade"></a>Azure AD Connect sivu

Kun ole kirjautunut sisään Azure-portaaliin, toimi seuraavasti:

1. Siirry Selaa-painike
2.  Valitse Azure Active Directory
3.  Valitse Azure AD Connect

Katso seuraavasti:

![Azure AD Connect sivu](media\active-directory-aadconnect-germany\germany1.png)

 
Seuraavassa taulukossa esitellään ominaisuudet, näkyvät sivu.


Otsikko|Kuvaus|
----- | ----- |
SYNKRONOINNIN TILA|Oletetaan, että tiedät, onko synkronointi käytössä vai ei.|
VIIMEISIN SYNKRONOINTI|Edellisen onnistuneen synkronointi valmis.|
YHDISTETTYJEN TOIMIALUEIDEN|Näyttää nykyisen yhdistettyjen toimialueiden määrän.|


## <a name="installation"></a>Asennus
Voit asentaa Azure AD Connect, käyttämällä ohjeista [tähän](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Lisäominaisuudet ja lisätiedot
Lisätietoja ja ohjeita mukautettuja asetuksia tai Lisäasetukset määritykset Aloita [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).  Tällä sivulla on tietoja ja linkkejä liittyviä lisäohjeita.
