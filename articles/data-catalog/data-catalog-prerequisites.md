<properties
   pageTitle="Azure tietoluettelon edellytykset | Microsoft Azure"
   description="Azure tietoluettelon-edellytykset - tarvitset Azure tietoluettelon käytön aloittaminen."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Azure tietoluettelon edellytykset

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>Mitä tarvitsen aloittaminen Azure tietoluettelon?

Sinun on huolehtia ennen kuin voit määrittää **Azure tietoluettelon**muutaman tavoilla. Älä huoli – ne eivät hidasta!

## <a name="azure-subscription"></a>Azure-tilaus
Määrittämään Azure tietoluettelon on oltava omistaja tai rinnakkaisomistajan Azure-tilaukseen.

Azure tilaukset avulla voit järjestää cloud palvelun resurssien kuten Azure tietoluettelon. Ne myös Ohje, voit määrittää, miten raportoidun resurssien käyttö, laskuttaa ja maksettu. Jokaisen tilauksen voi olla eri laskutus- ja asetukset, joten voi olla eri tilaukset ja erilaiset osasto, project, alueellisen office ja niin edelleen. Jokaisen pilvipalvelussa kuuluu tilauksen ja tarvitset tilauksesi, ennen kuin määrität Azure tietoluettelon. Lisätietoja on artikkelissa [tilien hallinta- ja tilaukset-järjestelmänvalvojan roolit](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Määrittääksesi Azure tietoluettelon, sinun on kirjauduttava sisään käyttämällä Azure Active Directory-käyttäjätiliä.

Azure Active Directory (Azure AD) tarjoaa yrityksesi hallittavan tunnistetietojen ja käytön, sekä cloud ja paikallisen helposti. Käyttäjät voivat käyttää yhden työpaikan tai oppilaitoksen tilille kertakirjautumisen minkä tahansa pilveen ja paikallisen web-sovelluksen. Azure tietoluettelon avulla Azure AD Sign-todennusta. Lisätietoja on artikkelissa [Azure Active Directory ominaisuudet](../active-directory/active-directory-whatis.md).

> [AZURE.NOTE] Kirjaudu sisään Microsoft-Account tai Azure Active Directory-työpaikan tai koulun tili [Azure portaalin](http://portal.azure.com/) käyttäjät. Azure-tietoluettelon Azure-portaalissa tai [tietoluettelon portaalin](http://www.azuredatacatalog.com) määrittäminen sinun on kirjauduttava sisään käyttämällä Azure Active Directory-tiliä, henkilökohtainen tili.

## <a name="active-directory-policy-configuration"></a>Active Directory-käytännön määritys

Joissakin tilanteissa käyttäjät voivat kohdata tilanteeseen, jossa ne voi kirjautua sisään Azure tietoluettelon-portaaliin, mutta yrittäessään heidän havaitsemistaan virhesanoma, joka estää niiden kirjautuminen tietojen lähteen rekisteröinti työkalun kirjautua. Tämä ongelma ongelma voi ilmetä vain, kun käyttäjä on yrityksen verkossa tai saattaa ilmetä vain silloin, kun käyttäjä muodostaa yhteyden ulkopuolella yrityksen verkkoon.

Tietojen lähteen rekisteröinti työkalu käyttää todennusmenetelmien vahvistamiseen käyttäjän kirjautumiset Active Directorysta. Onnistuneen kirjautumisen todennusmenetelmien on otettava käyttöön yleisen todennus-käytännön Active Directory-järjestelmänvalvoja.

Yleinen todennus-käytännön avulla käyttöön erikseen intranetissä ja ekstranet-yhteydet, todennusmenetelmät jäljempänä kuvatulla. Kirjautuminen virheitä saattaa ilmetä, jos todennusmenetelmien ei ole käytössä, joista käyttäjä muodostaa verkon.

 ![Active Directoryn Yleinen todennus käytäntö](./media/data-catalog-prerequisites/global-auth-policy.png)

Lisätietoja on artikkelissa [Käyttöoikeuksien käytäntöjen määrittäminen](https://technet.microsoft.com/library/dn486781.aspx).
