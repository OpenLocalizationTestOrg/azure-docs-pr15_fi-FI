<properties
   pageTitle="Ulkoisen käyttäjän suojaustunnuksen muoto Azure Active Directory-B2B yhteistyö esikatselu | Microsoft Azure"
   description="Azure Active Directory-B2B tukee yrityksen yhteydet ottamalla liikekumppanien käyttämään yrityksen sovellustesi valikoivasti"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-token-format"></a>Azure AD-B2B yhteistyön esikatselu: ulkoisen käyttäjän suojaustunnuksen muoto

Saatavat, vakio Azure AD tunnuksen on kuvattu [Tuetut tunnuksen ja varaa tyypit](active-directory-token-and-claims.md) -artikkelin azure.microsoft.com.

Vaateita koskevat rajoitukset, jotka ovat erilaisia todennetut B2B yhteistyön ulkoisen käyttäjän ovat seuraavat:<br/>
- **OID:** Objektitunnus resurssin vuokraajasta<br/>
- **TID**: vuokraaja resurssin vuokraajasta tunnus<br/>
- **Myöntäjä**: Tämä on resurssin vuokraajan<br/>
- **IDP**: Tämä on käyttäjän koti vuokraajan<br/>
- **AltSecId**: Tämä on vaihtoehtoinen Suojaustunnus, joka on peittävä sinulle<br/>

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
Siirry Azure AD B2B yhteistyö sekä muita artikkeleita:

- [Mikä on Azure AD B2B yhteistyö?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Toiminta](active-directory-b2b-how-it-works.md)
- [Laskun](active-directory-b2b-detailed-walkthrough.md)
- [CSV-tiedoston muodon Ohje](active-directory-b2b-references-csv-file-format.md)
- [Ulkoisen käyttäjän objektin määritemuutokset](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Nykyinen esikatselu-rajoitukset](active-directory-b2b-current-preview-limitations.md)
- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
