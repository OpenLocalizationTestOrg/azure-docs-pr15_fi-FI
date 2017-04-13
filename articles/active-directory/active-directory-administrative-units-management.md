<properties
   pageTitle="Azure Active Directoryn hallinta järjestelmänvalvojan resurssit"
   description="Hallinnollisten yksiköiden käytön Azure Active Directory käyttöoikeuksien eritellympiä delegointi"
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

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Azure AD - Public Preview hallinnollisten yksiköiden hallinta

Tässä artikkelissa kuvataan hallinnollisten yksiköiden – uuden Azure Active Directory-säilö resursseja, joiden avulla voidaan delegoinnin järjestelmänvalvojan oikeudet alijoukot käyttäjien ja kohdistaa käytäntöjä, jotka käyttäjät alijoukkoa päälle. Azure Active Directory-hallinnollisten yksiköiden käyttöön keskitetyn hallinnan järjestelmänvalvojien delegoiminen alueellisen järjestelmänvalvojille tai määrittää käytännön hajautetun tasolla.

Tästä on hyötyä organisaatiot, joiden riippumaton rajapintojen, esimerkiksi suuria university, joka koostuu monta yksipuolisia koulut (Business koulun, tekniikka koulun ja niin edelleen), joka ei vaikuta päähän toisistaan. Tällaisten rajapintojen on omia IT-järjestelmänvalvojille, jotka käyttöoikeuksien hallinta ja käyttäjien hallinta määrittää jäsenyydelle erityisesti niiden jakautuminen. Keskitetyn hallinnan järjestelmänvalvojien haluat voi myöntää nämä tehdyiksi järjestelmänvalvojien käyttöoikeuksia niiden tietyn jakokohtien käyttäjien päälle. Tarkemmin sanottuna tämän esimerkin central-järjestelmänvalvoja, esimerkiksi luoda hallinnollinen yksikkö tietyn oppilaitoksen (Business koulun) ja lisää vain koulun yrityskäyttäjät. Central-järjestelmänvalvoja voi lisätä Business oppilaitoksen IT-henkilöstön kohdistetuissa rooli, toisin sanoen Myönnä sitten koulun hallintaoikeudet IT-henkilöstön vain Business koulun hallinnollinen yksikkö päälle.

> [AZURE.IMPORTANT]
> Voit luoda ja käyttää hallinnollisten yksiköiden vain, jos otat Azure Active Directory-Premium. Lisätietoja on artikkelissa [Azure AD Premium käytön aloittaminen](active-directory-get-started-premium.md).

Keskitetyn hallinnan järjestelmänvalvoja näkökulmasta hallinnollinen yksikkö on hakemisto-objekti, joka voi luoda ja jossa resurssit. **Tässä versiossa nämä resurssit voi olla vain käyttäjät.** Kun luodaan ja täytetään, järjestelmänvalvojan yksikkö voidaan kuin alueen voit rajoittaa myönnetty vain hallintoalue sisältyviä resursseja päälle.

## <a name="managing-administrative-units"></a>Hallinnollisten yksiköiden hallinta

Tämä preview-versio voit luoda ja hallita järjestelmänvalvojan yksikköjä, käyttämällä Active Directory moduulin Windows PowerShellin Azure Cmdlet-komentoja.

Lisätietoja ohjelmistovaatimukset ja Azure AD-moduulin asentaminen ja Azure AD-moduulin cmdlet-komennot hallintaan hallinnollisten yksiköiden, mukaan lukien syntaksi, parametrien kuvaukset ja esimerkkejä tietojen ohjeaiheessa [hallinta Windows PowerShellin Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx).


## <a name="next-steps"></a>Seuraavat vaiheet
[Azure Active Directory-versiot](active-directory-editions.md)
