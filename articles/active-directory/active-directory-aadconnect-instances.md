<properties
    pageTitle="Azure AD Connect: Synkronoida palveluesiintymiä | Microsoft Azure"
    description="Tällä sivulla asiakirjat erityistä Huomioitavaa Azure AD-esiintymät."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Erityistä Huomioitavaa esiintymiä
Azure AD Connect käytetään yleensä maailmanlaajuisesti esiintymässä Azure AD- ja Office 365: ssä. Mutta liittyy myös muut esiintymät ja ne on erilaisia vaatimuksia URL-osoitteet ja muut seikkoja.

## <a name="microsoft-cloud-germany"></a>Microsoftin Saksa
[Microsoftin Saksa](http://www.microsoft.de/cloud-deutschland) on Saksan tietojen luottamussuhde ylläpitämä valtion pilvestä.

Tämä cloud on tällä hetkellä esikatselu. Monia tilanteita, joissa voit normaalisti itse, kuten Tarkista toimialueet, vaatii operaattori. Lisätietoja siitä, miten voit osallistua esikatselu Ota yhteyttä paikalliseen Microsoftin edustajalta.

Avaa välityspalvelimen URL-osoitteet |
--- |
\*. microsoftonline.de |
\*. windows.net |
+ Kumottujen varmenteiden luettelot varmenne |

Kun kirjaudut sisään Azure Active directory sinun on käytettävä tili onmicrosoft.de toimialue.

Microsoft Cloud Saksan ei ole tällä hetkellä käytettävissä seuraavat ominaisuudet:

- Azure AD yhteyden kunto ei ole käytettävissä.
- Automaattiset päivitykset ei ole käytettävissä.
- Salasanan takaisinkirjoitusta ei ole käytettävissä.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Government cloud
[Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) on US government pilvestä.

DirSync aikaisempien versioiden tukemat tämän pilveen. FROM Azure AD Connect seuraavan 1.1.180 koontiversiota pilveen sukupolven tuetaan. Tämä on käytössä vain Yhdysvaltojen mukaan päätepisteiden ja on eri välityspalvelimeen avaaminen URL-osoitteet.

Avaa välityspalvelimen URL-osoitteet |
--- |
\*. microsoftonline.com |
\*. gov.us.microsoftonline.com |
+ Kumottujen varmenteiden luettelot varmenne |

Azure AD Connect ei voi tunnistaa automaattisesti Azure AD-kansio sijaitsee Government pilveen. Sen sijaan sinun täytyy tehdä seuraavat toimet, kun asennat Azure AD Connect.

1. Käynnistä Azure AD Connect asennus.
2. Kun näet ensimmäinen sivu, jossa on määritetty Hyväksy käyttöoikeussopimus, ei voi jatkaa, mutta jättää käynnissä ohjatun asennuksen.
3. Käynnistä regedit ja rekisteriavaimen muuttaminen `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` arvoon `2`.
4. Siirry takaisin Azure AD Connect ohjattu asennus Hyväksy käyttöoikeussopimus ja jatka. Asennuksen aikana Varmista, että **Mukautettu määritys** asennuspolku (ja ole pika-asennus). Jatkaa asennusta tavalliseen tapaan.

Tällä hetkellä ole Microsoft Azure Government pilvipalvelussa seuraavat ominaisuudet:

- Azure AD yhteyden kunto ei ole käytettävissä.
- Automaattiset päivitykset ei ole käytettävissä.
- Salasanan takaisinkirjoitusta ei ole käytettävissä.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
