
<properties 
    pageTitle="Azure AD + Azure RemoteApp Active Directory-vaatimukset | Microsoft Azure" 
    description="Opi määrittämään Active Directory Azure RemoteApp-käyttöä varten." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + Azure RemoteApp Active Directory-vaatimukset

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .


Azure RemoteApp-hybrid sivustokokoelman tai cloud kokoelma, jonka haluat järjestäjäorganisaatiota AD Connect avulla sinun on suoritettava seuraavat.

### <a name="connect-azure-ad-and-active-directory"></a>Yhdistä Azure AD ja Active Directoryn

Jos haluat yhdistää Azure AD-vuokraajan ja paikallisen Active Directory-ympäristöissä, käytä AD Connect. Voit vain [4 napsautuksella](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) muodostaa kahden kansioiden kestää.

Huomautus - Directory-synkronointiin tarvitaan hybrid sivustokokoelmat.

### <a name="make-sure-your-domaincom-match"></a>Varmista, että "@domain.com" vastaa
Ennen kuin aloitat, varmista, että paikallisen metsää UPN vastaa Azure AD-toimialueen liitteen. 

Kun olet määrittänyt täydellisen Käyttäjätunnuksen jälkiliitteen Azure AD-kirjautumalla järjestelmään Azure RemoteApp kaikki käyttäjät Kirjaudu sisään “user@ <the suffix you set up>". Varmista, että käyttäjät voivat kirjautua sisään sama myös user@suffix paikallisen toimialueelle. Joissakin tapauksissa voit määrittää yhden toimialuenimen Azure AD aikana käyttäjän käyttöön-prem. eri jälkiliitteen määrittäminen Tässä tapauksessa käyttäjät eivät voi yhdistää toimialueeseen liittyneet tietokoneet tai resursseja Azure RemoteApp kautta.

Jos määrität oman täydellisen Käyttäjätunnuksen jälkiliitteen AAD kuin contoso.com-, mutta jotkin käyttäjien paikallisen/AD on määritetty kirjautumaan sisään esimerkiksi @contoso.uk, sitten nämä käyttäjät eivät pysty kirjautumaan oikein ARA-kokoelmaan. Käyttäjien UPN AAD ja AD on oltava sama mahdollista kirjautumiset"

### <a name="create-objects-for-azure-remoteapp"></a>Luo objektien Azure RemoteApp
Sinun on myös seuraavat paikalliseen Active Directory-objektien luomiseen:

- Palvelutili antamaan toimialueen resurssien käytön RemoteApp-ohjelmien liittymällä RDSH päätepisteestä paikallisen toimialueen.
- Organisaatioyksikkö (OU) sisältää RemoteApp tietokoneen objekteja. Käyttö OU suositellut (mutta ei ole pakko täyttää) eristää asiakkaat ja käytät RemoteApp käytännöt.

Tarvitset molemmat objektit luodessasi RemoteApp-sivustokokoelman, joten muista tehdä seuraavat toimet ensin.