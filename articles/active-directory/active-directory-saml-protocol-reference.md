<properties
    pageTitle="Azure AD-SAML protokolla viittaus | Microsoft Azure"
    description="Tässä artikkelissa on yleiskatsaus Azure Active Directoryn kertakirjautumisen ja yksittäisen Sign-Out SAML-profiileista."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Miten Azure Active Directory käyttää SAML-protokollaa

Azure Active Directory (Azure AD) käyttää SAML 2.0 protokolla sovellukset ja tarjota yksittäisen Sign-käyttökokemuksen käyttäjilleen ottaa käyttöön. Azure AD [Kertakirjautumisen](active-directory-single-sign-on-protocol-reference.md) ja [Kirjaudu ulos yhden](active-directory-single-sign-out-protocol-reference.md) SAML-profiileista kerrotaan, miten SAML vahvistukset, protokollat ja sidontojen käytetään tunnistetietojen toimittaja-palvelussa.

SAML-protokolla edellyttää tunnistetietojen toimittaja (Azure AD) ja palvelun (sovelluksen) Exchange tietoja itsestään.

Kun sovellus rekisteröidään Azure AD-sovelluksen kehittäjä Rekisteröi Azure AD federation liittyviä tietoja. Tämä sisältää **Uudelleenohjaaminen URI** ja **Metatietojen URI** -sovelluksen.

Azure AD käyttää pilvipalvelussa sijaitsevaan **Metatietojen URI** allekirjoitetun avain- ja uloskirjautuminen URI cloud-palvelun hakemiseen. Jos sovellus ei tue metatietojen URI-kehittäjä Ota yhteyttä Microsoftin asiakaspalveluun antamaan Kirjaudu URI ja avain.

Azure Active Directory paljastaa vuokraajan kielikohtaiset ja yleisiä (vuokraajan riippumattomalla) yksittäisen Sign ja yksittäisen Kirjaudu ulos päätepisteet. Näitä URL-osoitteet edustavat käytettävissä sijainnit – ne eivät juuri tunnisteet--, jotta voit avata päätepisteen lukemaan metatiedot.

 - Vuokraajan kielikohtaiset päätepisteen sijaitsee `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  <TenantDomainName> Paikkamerkki rekisteröity toimialuenimi tai Azure AD-vuokraajan TenantID GUID-tunnus. Esimerkiksi contoso.com-vuokraajassa federation metatiedot on osoitteessa: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- Vuokraajan riippumattomalla päätepisteen sijaitsee `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. Päätepisteen tähän osoitteeseen **Yleiset** tulee näkyviin, sen sijaan, että vuokraajan toimialuenimi tai tunnus.

Federation metatietojen tiedostot, jotka Azure AD julkaisee tietoja on artikkelissa [Federation metatiedot](active-directory-federation-metadata.md).
