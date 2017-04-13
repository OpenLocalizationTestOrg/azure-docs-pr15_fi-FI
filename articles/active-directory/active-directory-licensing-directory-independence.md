<properties
   pageTitle="Lisätä ja hallita useita Azure Active Directory-kansioita | Microsoft Azure"
   description="Ohjeita ja parhaita käytäntöjä lisääminen ja Azure Active Directory-kansioita, kerrotaan täysin riippumaton resursseina kansioiden hallinta"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Lisätä ja hallita useita Azure Active Directory-kansiot

Azure Active Directoryn (Azure AD), jokaisen on täysin riippumaton resurssi: peer täysin tarjottu ja muita kansioita, voit hallita loogisesti riippumatta. Kansioiden välillä ei ole ylä-ja alatason yhteyttä. Tämä riippumaton kansioiden välillä sisältää resurssin riippumaton, järjestelmänvalvojan riippumaton ja synkronointi riippumaton.

##<a name="resource-independence"></a>Resurssin riippumaton

Jos olet luonut tai Poista resurssi samassa kansiossa, se ei ole vaikutusta mille tahansa resurssille toiseen kansioon, osittainen lukuun ottamatta Ulkoiset käyttäjät, jäljempänä kuvatulla tavalla. Jos käytät mukautettua toimialuetta "contoso.com" yhden kansion, sitä ei voi käyttää muissa Directory-hakemistosta.

##<a name="administrative-independence"></a>Järjestelmänvalvojan riippumaton

Jos kansion 'Contoso'-järjestelmänvalvojan käyttäjä luo testi hakemiston 'Testi' sitten:
- Oletusarvon mukaan hakemiston luova käyttäjä on lisätty ulkoisena käyttäjänä uuden kansion ja määritetty yleisen järjestelmänvalvojan rooliin kansiossa.
- Kansion "Contoso-järjestelmänvalvojilla on ei ole suoraa järjestelmänvalvojan oikeudet kansioon 'Testi', ellei järjestelmänvalvoja 'Testi' erityisesti myöntää heille nämä oikeudet. "Contoso-järjestelmänvalvojien käyttöoikeuksia voidaan hallita kansioon 'Testi' Jos ne käyttäjätili, joka luodaan"Testi."
- Jos muutat (lisääminen tai poistaminen) järjestelmänvalvojaroolin käyttäjän samassa kansiossa, muutos ei vaikuta minkä tahansa järjestelmänvalvojaksi, käyttäjä voi olla toiseen kansioon.

##<a name="synchronization-independence"></a>Synkronoinnin riippumaton

Voit määrittää kunkin Azure AD-hakemiston itsenäisesti, jos haluat synkronoida yksittäisen esiintymän joko tiedot:
  - Hakemistosynkronoinnille (DirSync) työkalu, tietojen synkronointi yksittäisen AD-metsää.
  - Azure Active Directory Connectorin varten Forefront käyttäjätietojen hallinta, voit synkronoida tiedot vähintään yksi paikallisen metsien ja/tai muu Azure AD-tietolähteet.

##<a name="add-an-azure-ad-directory"></a>Lisää Azure AD-kansio

Voit lisätä Azure perinteinen portaalin Azure Active directory-Valitse vasemman reunan Azure Active Directory-tunniste ja sitten **Lisää**.

> [AZURE.NOTE]   Toisin kuin muut Azure resurssit oman kansioiden eivät ole lapsen resurssien Azure-tilaukseen. Jos Peruuta tai Salli Azure tilauksesi vanhenee, voit silti käyttää hakemiston tietojen PowerShellin Azure Azure Graph Ohjelmointirajapinnan ja muut liittymät esimerkiksi Office 365-hallintakeskukseen. Voit myös yhdistää toiseen tilaukseen hakemistossa.

Laaja yleistä Azure AD-käyttöoikeuksista ja parhaita käytäntöjä saat [Mikä on Azure Active Directory käyttöoikeuksien?](active-directory-licensing-what-is.md).
