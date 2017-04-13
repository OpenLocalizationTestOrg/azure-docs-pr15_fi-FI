<properties
pageTitle="Azure Active Directory-sovelluksen ja palvelun lyhennys objektien | Microsoft Azure"
description="Keskustelun sovelluksen ja palvelun pääasiallista objektien Azure Active Directoryn välinen suhde"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Sovelluksen ja palvelun pääasiallista objektien Azure Active Directory
Kun olet lukenut tietoja Azure Active Directory (AD)-sovelluksen", ei voi aina Tyhjennä täsmälleen mitä parhaillaan viitataan tekijä. Tässä artikkelissa on, jotta se selkeä, määrittämällä Azure AD-sovelluksen Esimerkki rekisteröinti ja hyväksyntä [usean vuokraajan sovelluksen](active-directory-dev-glossary.md#multi-tenant-application)kanssa integrointi käsitteellisiä ja betonin ominaisuuksia.

## <a name="overview"></a>Yleiskatsaus
Azure AD-sovellus on laajempi kuin vain piece ohjelmiston. Käsitteellinen termin, ei vain viittaaminen sovellusohjelma, mutta sen rekisteröinti on (eli: identity-määritys) kanssa Azure AD, joka sallii sen todennus- ja "keskusteluihin osallistuminen" suorituksen aikana. Määritelmän-sovelluksen toimivat (muissa resurssin) [asiakas](active-directory-dev-glossary.md#client-application) -roolin [resurssin palvelimen](active-directory-dev-glossary.md#resource-server) rooli (asiakkaiden paljastaa API) tai vaikka molemmat. Keskustelu-protokolla määritetään [OAuth 2.0 käyttöoikeuksien myöntäminen työnkulku](active-directory-dev-glossary.md#authorization-grant)-kanssa tavoite on salliminen asiakkaan ja resurssien käyttö/resurssin tietojen suojaaminen tarpeen mukaan. Nyt mennään tarkempaa tasolle ja katso, miten Azure AD-sovelluksen malli edustaa sovelluksen sisäisesti. 

## <a name="application-registration"></a>Sovelluksen rekisteröiminen
Kun sovellus rekisteröidään [Azure perinteinen portal][AZURE-Classic-Portal], kaksi objektit luodaan Azure AD-vuokraajan: sovellusobjektin ja service Pääobjektin.

#### <a name="application-object"></a>Sovellusobjektin
Azure AD-sovellus on *määritetty* sen yksi ja sovelluksen "koti" vuokraajan kutsutaan vain sovellusobjektin, joka sijaitsee Azure AD-vuokraajan, jossa sovellus on rekisteröity. Sovellusobjektin sisältää sovelluksen tunnistetietojen liittyviä tietoja ja malleista, josta sen vastaavan palvelun pääasiallista objekteja on *johdetun* käytettäväksi suorituksen aikana. 

Voit ajatella sovelluksen nimellä (käytettäväksi kaikissa kaikki alihallinnat)-sovelluksen *Yleinen* esitys ja palvelun lyhennys kuin *paikallisen* esitys (käytettäväksi tietyn alihallinnassa). Azure AD Graph- [sovelluksen kohteen] [ AAD-Graph-App-Entity] määrittää sovellusobjektin rakenne. Sovellusobjektin vuoksi on 1:1 yhteyteen sovellus ja 1:*n* yhteyteen sen vastaavan *n* palvelun pääasiallista objekteja.

#### <a name="service-principal-object"></a>Service Pääobjektin
Service Pääobjektin määrittää käytännön ja sovelluksen antamisen suojaus-lyhennys edustavan sovelluksen käytettäessä resurssien suorituksen aikana perusta käyttöoikeuksien. Azure AD-kaavion [ServicePrincipal kohteen] [ AAD-Graph-Sp-Entity] määrittää service Pääobjektin rakenne. 

Service Pääobjektin tarvitaan kunkin palveltavan, jonka esiintymä sovelluksen käyttö on edustaa, suojatun kyseisen alihallinnan käyttäjätilit omistaa resurssien käytön ottaminen käyttöön. Yhden vuokraajan-sovellus on vain yksi palvelun lyhennys (-sen koti Alihallinta). Usean vuokraajan [verkkosovelluksen](active-directory-dev-glossary.md#web-client) on valmisteltu palvelun lyhennys kunkin alihallinnan, jossa järjestelmänvalvoja tai käyttäjiä alihallintaan-annetut luvan salliminen resursseille käyttämään. Seuraavat luvan tulevien pyytää kuulla service Pääobjektin. 

> [AZURE.NOTE] Sovelluksen-objekti, tekemäsi muutokset näkyvät myös sen service Pääobjektin sovelluksen koti alihallinnan vain (Alihallinta, johon se on rekisteröity). Usean vuokraajan sovellusten sovelluksen objektiin ei näkyvät kaikki consumer alihallinnat palvelun pääasiallista objektit, kunnes kuluttaja-vuokraajan access poistaa ja antaa käyttöoikeudet uudelleen.

## <a name="example"></a>Esimerkki
Seuraavassa kaaviossa on kuvattu sovelluksen sovellusobjektin ja vastaava suhteen palvelun pääasiallista objektit-nimisen **HR app**otoksen usean vuokraajan-sovelluksen yhteydessä. Tässä skenaariossa on kolme Azure AD-alihallinnat: 

- **Adatum** - yritys, joka on kehittänyt **HR sovelluksen** käyttämä vuokraajan
- **Contoso** - vuokraajan **HR app** kuluttaja eli Contoso-organisaation käyttämän
- **Fabrikam** - vuokraajan Fabrikam organisaatiokaavion, joka käyttää myös **HR sovelluksen** käyttämä

![Sovellusobjektin ja service Pääobjektin välinen suhde](./media/active-directory-application-objects/application-objects-relationship.png)

Vaihe 1 on edellisen kaaviossa luominen sovelluksen ja palvelun pääasiallista objektien sovelluksen koti vuokraajan.

Valitse vaiheessa 2 Kun Contoso ja Fabrikam järjestelmänvalvojille luvan service Pääobjektin luodaan yhtiön Azure AD-vuokraajan ja, joka on myönnetty järjestelmänvalvojan käyttöoikeudet. Huomaa, että HR-sovelluksen voidaan määritetty/suunnitella suostumusta sallimaan käyttäjien yksittäisiä käyttöä varten.

Vaihe 3 HR sovelluksen (Contoso ja Fabrikam) kunkin kuluttaja-alihallinnat on oman service Pääobjektin. Kunkin edustaa niiden käyttö suorituksen käyttöoikeudet noudatettava sovelluksen esiintymää on suostunut vastaaviin järjestelmänvalvoja.

## <a name="next-steps"></a>Seuraavat vaiheet
Sovelluksen sovellusobjektin käyttää kautta Azure AD-kaavio-Ohjelmointirajapinnan mukaisessa sen OData- [sovelluksen kohde][AAD-Graph-App-Entity]

Sovelluksen service Pääobjektin käyttää kautta Azure AD-kaavio-Ohjelmointirajapinnan mukaisessa sen OData- [ServicePrincipal kohde][AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com