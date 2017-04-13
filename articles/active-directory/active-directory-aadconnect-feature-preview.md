<properties
   pageTitle="Azure AD Connect: Ominaisuuksien esikatselu | Microsoft Azure"
   description="Tässä ohjeaiheessa kerrotaan Lisää tiedot ominaisuuksia, jotka ovat Azure AD Connect esikatselussa."
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

# <a name="more-details-about-features-in-preview"></a>Lisätietoja esikatseluversion toiminnot
Tässä artikkelissa käsitellään ominaisuuksien käyttäminen tällä hetkellä esikatselu.

## <a name="group-writeback"></a>Ryhmän takaisinkirjoituksen
Ryhmän takaisinkirjoituksen valinnaisia ominaisuuksia-vaihtoehto mahdollistavat takaisinkirjoituksen **Office 365-ryhmiä** , metsää asennettu Exchangen kanssa. Tämä on ryhmää, joka on aina selvittänyt pilveen. Jos sinulla on Exchange-paikallisia jälkeen voit kirjoittaa takaisin ryhmiin paikalliseen niin käyttäjät, joilla on paikallisen Exchange-postilaatikon voit lähettää ja vastaanottaa sähköpostiviestejä nämä ryhmistä.

Lisätietoja Office 365-ryhmiä ja niiden käyttämisestä löytyy [tähän](http://aka.ms/O365g).

Tämän ryhmän voi esittää jakeluryhmänä paikallisessa AD DS: ÄÄN. Paikallisen Exchange-palvelimeen on oltava Exchange 2013: n kumulatiivinen päivitys (julkaistu maaliskuussa 2015) 8- tai Exchange 2016 tunnistettavan tällaista ryhmän.

**Muistiinpanojen aikana esikatselu**

- Address book määrite on tällä hetkellä ole kaikille esikatselu. Tämän määritteen ilman ryhmän ei näy osoitteistossa. Täytä tämän määritteen helpoin tapa on Exchange-PowerShell-cmdlet-komennolla `update-recipient`.
- Vain Exchange-rakennetta metsien ovat kelvollisia ryhmien kohteet. Jos havaittiin ei ole Exchange-ryhmän takaisinkirjoitusta ei voi ottaa käyttöön.
- Vain yhden metsää Exchange organisaation ominaisuuksissa tällä hetkellä tueta. Jos sinulla on useampi kuin yksi Exchange organisaation paikallisia, sinun on paikallisen GALSync-ratkaisun näiden ryhmien oman metsien näkyvän.
- Ryhmän takaisinkirjoituksen-toimintoa ei tällä hetkellä Käsittele käyttöoikeusryhmiä tai jakeluryhmien.

>[AZURE.NOTE] Azure AD Premium-tilauksen vaaditaan ryhmän takaisinkirjoituksen.

## <a name="user-writeback"></a>Käyttäjän takaisinkirjoituksen
> [AZURE.IMPORTANT] Käyttäjän takaisinkirjoituksen esikatselu-toimintoa poistettiin Azure AD Connect elokuussa 2015 Päivitä. Jos olet ottanut sen, olisi käytöstä tätä ominaisuutta.

## <a name="next-steps"></a>Seuraavat vaiheet
Siirry [Azure AD Connect mukautettu asennus](./connect/active-directory-aadconnect-get-started-custom.md).

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
