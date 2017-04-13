<properties
    pageTitle="Azure MFA cloud ja palvelimen | Microsoft Azure"
    description="Valitse monimenetelmäisen todentamisen secutiry ratkaisu, joka on pyytämällä mitä am yritetään suojatun i, oikea- ja missä oma sijaitsevat käyttäjät ovat.  Valitse pilvipalveluun, MFA palvelimeen tai AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Valitse Azure multi-factor Authentication-ratkaisun puolestasi

Koska useita flavors Azure multi-factor Authentication (MFA), emme on kysymyksiin muutaman selvittää, mikä versio on oikea käyttämään.  Näihin kysymyksiin ovat seuraavat:

-   [Mitä suojaamiseen 'M kokeilujakson](#what-am-i-trying-to-secure)
-   [Missä käyttäjät sijaitsevat](#where-are-the-users-located)
- [Mitä ominaisuuksia tarvitsen?](#what-featured-do-i-need)

Seuraavassa esitellään määritettäessä kunkin nämä vastaukset.

## <a name="what-am-i-trying-to-secure"></a>Mitä suojaamiseen 'M kokeilujakson?

Voit selvittää oikean kaksivaiheinen vahvistus-ratkaisun, ensin on on kysymykseen, mitä voit yrittää toisen todennusmenetelmän, suojaaminen.  Onko sovellus, joka on Azure?  Tai etäkäyttöpalvelimen järjestelmän?  Olemme kokeilujakson suojaamiseen selvittämällä emme voi vastata kysymys, johon Monimenetelmäisen todentamisen on otettava käyttöön.  


Mitä Yritätkö suojatun| Monimenetelmäisen todentamisen pilveen|Monimenetelmäisen todentamisen Server
------------- | :-------------: | :-------------: |
Ensimmäisen osapuolen Microsoft-sovellukset|● |● |
SaaS sovellukset app-valikoimassa|● |● |
IIS-sovellukset, jotka on julkaistu Azure AD-sovelluksen välityspalvelimen kautta|● |● |
IIS-sovelluksia ei julkaista Azure AD-sovelluksen välityspalvelimen kautta | |● |
Etäkäyttö, kuten VPN-RDG| |● |



## <a name="where-are-the-users-located"></a>Missä käyttäjät sijaitsevat

Seuraavaksi katsoo, jossa käyttäjien sijaitsevat auttaa määrittämään oikea-ratkaisun käyttämään cloud tai paikalliseen MFA-palvelinta.



Käyttäjän sijainti| Monimenetelmäisen todentamisen pilveen|Monimenetelmäisen todentamisen Server
------------- | :-------------: | :-------------: |
Azure Active Directory|● | |
Azure AD ja paikallisen AD federation käyttäminen AD FS|● |● |
Azure AD ja paikallisen AD käyttämällä DirSync-Azure AD-synkronointi Azure AD Connect – ei ole salasanan synkronointi|● |● |
Azure AD- ja -ympäristöön DirSync-Azure AD-synkronointi Azure AD Connect - käyttämisestä salasanan synkronointi AD|● | |
Paikallisen Active Directory| |● |

## <a name="what-features-do-i-need"></a>Mitä ominaisuuksia tarvitsen?

Seuraavassa taulukossa on vertailu ominaisuuksista, jotka ovat käytettävissä Monimenetelmäisen todentamisen pilveen ja multi-factor Authentication-palvelimen kanssa.

 | Monimenetelmäisen todentamisen pilveen | Monimenetelmäisen todentamisen Server
------------- | :-------------: | :-------------: |
Toinen tekijä ilmoitus mobiilisovelluksessa | ● | ● |
Mobiilisovelluksen tarkistuskoodi kuin toinen tekijä | ● | ●
Puhelun kuin toinen tekijä | ● | ●
Yksisuuntainen SMS kuin toinen tekijä | ● | ●
Kaksisuuntainen SMS kuin toinen tekijä |  | ●
Laitteiston tunnusten kuin toinen tekijä |  | ●
Asiakkaat, jotka eivät tue MFA ohjelmasalasanat | ● |  
Järjestelmänvalvoja määrittää todennustavat | ● | ●
PIN-tilassa |  | ●
Ilmoituksen ilmoitus | ● | ●
MFA-raportit | ● | ●
Ohita kerran |  | ●
Mukautetut tervehdykset puhelut | ● | ●
Mukautettavissa Soittajatunnus puhelut | ● | ●
Luotettu IP-osoitteet | ● | ●
Muista MFA luotettujen laitteille  | ● |  
Ehdollisen käyttöoikeuden | ● | ●
Välimuistin |  | ●

Nyt kun on määrittänyt käytetäänkö cloud monimenetelmäisen todentamisen tai MFA palvelimen paikalliseen, emme voivat aloittaa määrittämisestä ja käyttämisestä Azure Monimenetelmäisen todentamisen. **Valitse kuvake, joka edustaa käyttämässäsi skenaariossa.**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
