<properties
    pageTitle="Uusi laitetta, jonka asennuksen aikana Azure AD määritetty | Microsoft Azure"
    description="Aihe, jossa kerrotaan, miten käyttäjät voivat määrittää Azure AD liittyä ensimmäisen käyttökerran kokemuksensa aikana."
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

# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Määrittää uuden laitteen kanssa Azure AD asennuksen aikana

Windows 10: ssä käyttäjät voivat liittyä laitteensa Azure Active Directory (Azure AD)-ensimmäinen käyttökerta (FRX). Näin organisaatiot voivat jakaa huonolaatuiset laitteiden niiden työntekijöiden tai opiskelijoiden tai Ilmoita valita oman laitteiden (CYOD).
Jos laitteessa on asennettu Windows 10 Professional tai Windows 10 Enterprise-versiot, kokemus oletusarvoisesti määritysprosessi yrityksen omistamien laitteita.

## <a name="to-join-a-device-to-azure-ad"></a>Laitteen liittäminen Azure AD


1. Kun uusi laitteesi ottaminen käyttöön ja aloita määritys, pitäisi näkyä **Käytön valmis** viesti. Laitteen määrittäminen kehotteiden mukaisesti.
2. Aloita mukauttamalla alue ja kieli. Valitse Hyväksy Microsoft-ohjelmiston käyttöoikeussopimuksen ehdot.
![Alueesi mukauttaminen](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Haluat käyttää Internet-yhteyden muodostaminen verkon valitseminen
4. Valitse, onko käytössäsi henkilökohtainen- tai yrityksen omistamien laite. Jos yrityksen omistamien, valitse **laite kuuluu oman organisaation ulkopuolelta**. Azure AD Join-toiminto käynnistyy. Seuraavassa on, että näet, jos käytössäsi on Windows 10 Professional näyttöön.
<center>
![Kuka omistaa tämän tietokoneen näytön](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)

5.  Anna tunnistetiedot, jotka olivat sinulle organisaatiosi tarjoama.
<center>
![Kirjautumisnäyttö](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6.  Kun olet kirjoittanut käyttäjänimen, vastaava alihallintaympäristöä sijaitsee Azure AD. Jos olet liitetyssä toimialueessa, sinut ohjataan paikallisen suojatun suojaustunnuksen Service (STS) palvelimeen – esimerkiksi Active Directory Federation Services (AD FS).
7. Jos olet käyttäjä ei ole liitetty toimialueeseen, kirjoittamalla tunnistetietosi suoraan sivulla Azure AD isännöidään. Jos yrityksen ulkoasu on määritetty, katso on organisaation logo myös ja tukevat tekstiä.
8.  Sinua pyydetään monimenetelmäisen todentamisen hankala varten. Tämä todennus on määritettävä IT-järjestelmänvalvoja.
9.  Azure AD tarkistaa, onko käyttäjän/laite vaatii mobiililaitteiden hallinta rekisteröinti.
10. Windows Rekisteröi laite organisaation hakemiston Azure AD- ja liittää sen mobiililaitteiden hallinta tarvittaessa.
11. Jos olet hallitut käyttäjien, Windows siirryt työpöydän automaattisen kirjautumisen vaiheet.
12. Jos olet liitetyt käyttäjät, sinua pyydetään tunnistetietosi Windows sisäänkirjautumisikkunan.

> [AZURE.NOTE] Liittyminen paikallisen Windows Server Active Directory-toimialueen uudelleen Windows-ruutuun kokeiluversioon ei tueta. Sen vuoksi, jos haluat liittää tietokoneen toimialueeseen, sinun on valittava **Windowsin paikallisen tilin määrittäminen** linkki sen sijaan. Voit liittyä asetuksista toimialueen sitten tietokoneessa, kun olet valmis ennen.

## <a name="additional-information"></a>Lisätietoja
* [Windows 10: n yrityksen: tapoja käyttää laitteet](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liittyä laajentaminen](active-directory-azureadjoin-user-upgrade.md)
* [Käyttäjätietojen ilman salasanan vaihtamiseen Microsoft Passport todennustapa](active-directory-azureadjoin-passport.md)
* [Lisätietoja käyttötapoja Azure AD liittyminen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä kokemukset](active-directory-azureadjoin-devices-group-policy.md)
* [Määritä Azure AD liittyminen](active-directory-azureadjoin-setup.md)
