<properties
   pageTitle="Azure automaatio suojauksen | Microsoft Azure"
   description="Tässä artikkelissa on yleiskatsaus automaatio suojaus ja käytettävissä eri todennusmenetelmät Azure automaatio automaatio-tilin."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="automaatio suojaus, suojattu automaatio" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Azure automaatio-suojaus
Azure automaatio voit automatisoida tehtäviä vastaan resurssien Azure-paikallisen, ja muiden cloud palveluntarjoajien, kuten Amazon Web Services (sisältyy AWS).  Jotta runbookin suorittamaan kaikki tarvittavat toimet sen on oltava oikeudet käyttää turvallisesti tarvitsemat tilaus mahdollisimman vähän oikeuksilla resursseja.  
Tässä artikkelissa ovat vastuussa eri tilanteista todennus-tukemat Azure automaatio ja kerrotaan, kuinka pääset alkuun ympäristössä tai ympäristössä, sinun on hallittava perusteella.  

## <a name="automation-account-overview"></a>Automaatio-tilin yhteenveto
Kun käynnistät Azure automaatio ensimmäistä kertaa, sinun on luotava vähintään yksi automaatio-tili. Automaatio-tilien avulla voit eristää automaatio resurssit (runbooks, resurssien, määrityksiä) resursseilta, sisältyvät automaatio muihin tileihin. Automaatio-tilien käyttäminen erottaa useaan looginen erillisestä resurssit. Voit esimerkiksi käyttää tilien kehitystä, toiseen tuotannon ja toiseen paikallisen ympäristön.  Azure automaatio-tili on eri Microsoft-tili tai tilit, jotka on luotu Azure-tilaukseesi.

Automaatio-tileille automaatio-resurssit on liitetty yhden Azure alueen, mutta automaatio tilit voit hallita resursseja minkä tahansa alueella. Tärkeimmät syy automaatio-tilien luominen eri alueiden olisi onko käytännöt, jotka edellyttävät tietojen ja resurssien erillään tiettyyn alueeseen.

>[AZURE.NOTE]Automaatio-tilit, ja ne, jotka sisältävät resurssien luodaan Azure-portaalissa, ei voi käyttää Azure perinteinen-portaalissa. Jos haluat hallita näissä tileissä tai resursseille Windows PowerShellin avulla, sinun on käytettävä Azure Resurssienhallinta-moduuleja.

Kaikki tehtävät, jotka suorittavat vastaan resurssien Azure Resurssienhallinta ja Azure cmdlet-komentojen käyttäminen Azure automaatio on todentaa Azure Azure Active Directory-tunnuksillasi credential perustuva todennusta.  Varmenteen oli alkuperäisen todentamismenetelmän Azure hallinta-tilassa, mutta se on monimutkainen määritys.  Todennuksen Azure Azure AD-käyttäjän kanssa on käyttöön takaisin sisään 2014 ei vain yksinkertaistaa todennus-tilin määrittäminen, mutta tukea myös voi todentaa ei-vuorovaikutteisesti Azure yksittäinen käyttäjätili, jolla käsitellyt Azure Resurssienhallinta ja perinteinen resurssien kanssa.   

Tällä hetkellä, kun luot uuden automaatio-tilin Azure-portaalissa, luo automaattisesti:

-  Suorita tiliksi, joka luo uuden palvelun lyhennys Azure Active Directory-varmenne, ja määrittää avustaja Roolipohjainen käyttöoikeuksien hallinta (RBAC), jota käytetään Resurssienhallinta resurssien käyttäminen runbooks.
-  Perinteinen Suorita nimellä tilin hallinta-varmenne, jota käytetään Azure Service Management tai classic resurssien runbooks lataamalla.  

Roolipohjainen käyttöoikeuksien valvonta on käytettävissä Azure resurssien hallinnan myöntää Azure AD-käyttäjätilin ja Suorita nimellä tilin sallitut toiminnot ja todentaa kyseisen palvelun lyhennyksen.  Lue [Roolipohjainen käyttöoikeuksien valvonta Azure automaatio artikkelin](../automation/automation-role-based-access-control.md) lisätietoja kehittämässä malli automaatio käyttöoikeuksien hallinta.  

Hybrid Runbookin työntekijä oman palvelinkeskuksen tai vastaan tietojenkäsittely sisältyy AWS Services-palvelujen käytössä Runbooks ei voi käyttää samaa menetelmää, jota käytetään yleensä runbooks todentamisesta Azure resurssit.  Tämä johtuu siitä resursseja on käytössä Azure ulkopuolella ja siksi edellyttävät omia suojausvaltuudet määritelty automaatio todentaa resurssit, jotka he käyttävät paikallisesti.  

## <a name="authentication-methods"></a>Todennustavat

Seuraavassa taulukossa on yhteenveto kunkin ympäristön Azure automaatio ja on artikkelissa kerrotaan, että runbooks todennuksen määrittäminen tukevat eri todennusmenetelmät.

Menetelmä  |  Ympäristön  | Artikkelissa
----------|----------|----------
Azure AD-käyttäjätili | Azure Resurssienhallinta ja Azure hallinta | [Azure AD-käyttäjätilin Runbooks todentamismenetelmä](../automation/automation-sec-configure-aduser-account.md)
Azure Suorita tiliksi | Azure Resurssienhallinta | [Todennetaan Runbooks Azure Suorita nimellä-tilillä](../automation/automation-sec-configure-azure-runas-account.md)
Azure perinteinen Suorita tiliksi | Azure hallinta | [Todennetaan Runbooks Azure Suorita nimellä-tilillä](../automation/automation-sec-configure-azure-runas-account.md)
Windows-todennus | Paikallisen Palvelinkeskukseen | [Todennetaan Runbooks Hybrid Runbookin työntekijöille](../automation/automation-hybrid-runbook-worker.md)
Sisältyy AWS tunnistetiedot | Amazon verkkopalvelut | [Todennetaan Runbooks Amazon Web Services (sisältyy AWS)](../automation/automation-sec-configure-aws-account.md)



