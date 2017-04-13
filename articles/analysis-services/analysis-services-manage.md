<properties
   pageTitle="Hallitse Azure Analysis Services | Microsoft Azure"
   description="Opi hallitsemaan Analysis Services-palvelimen Azure-tietokannassa."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="manage-analysis-services"></a>Analysis Services hallinta

Kun olet luonut Analysis Services-palvelimen Azure-tietokannassa, voi olla joitakin hallinta ja hallintatehtäviä, sinun on suoritettava heti tai viime myöhemmin. Suorita esimerkiksi käsittelyn tietojen päivittäminen, Määritä, kuka voi käyttää malleja palvelimeen tai palvelimen kunnon valvonta. Jotkin hallintatehtäviä voidaan suorittaa vain Azure-portaalissa, muiden: SQL Server Management Studiossa (SSMS), ja jotkin tehtävät voidaan toteuttaa joko.

## <a name="azure-portal"></a>Azure portal
[Azure portal](http://portal.azure.com/) on, missä voit luoda ja poistaa palvelinten, seurata palvelinresurssien, koon muuttaminen, sekä hallinta, kenellä on oikeudet palvelimiin.  Jos sinulla on ongelmia, voit lähettää tukipyynnön myös.

![Hae palvelimen nimi Azure-tietokannassa](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studiossa
Yhdistäminen palvelimeen Azure-tietokannassa on aivan kuin yhteyden server-esiintymän oman organisaation ulkopuolelta. SSMS voit suorittaa monia samoja tehtäviä, kuten tietojen tai käsittely komentosarjan luominen, roolien hallinta ja PowerShellin käyttäminen.

![SQL Server Management Studiossa](./media/analysis-services-manage/aas-manage-ssms.png)

 Suurentaa erot on todennus muodostat yhteyden palvelimeen. Haluat muodostaa yhteyden Azure Analysis Services-palvelimen Valitse **Active Directory salasanan todennusta**.

### <a name="to-connect-with-ssms"></a>Jos haluat muodostaa yhteyden SSMS
1. Ennen kuin yhdistät, sinun on palvelimen nimen. **Azure** -portaalissa > server > **Yleiskatsaus** > **palvelimen nimi**, kopioi palvelimen nimi.

    ![Hae palvelimen nimi Azure-tietokannassa](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. Valitse SSMS > **Object Explorer** **Yhdistä** > **Analysis Services**.

3. **Muodosta yhteys palvelimeen** -valintaikkunassa Liitä palvelimen nimi ja valitse kohdassa **todennus**-kohdassa jokin seuraavista:

    **Active Directory-todennusta** käyttää kertakirjautumisen Azure Active Directory federation Active Directory-hakemistosta.

    **Active Directory-salasanan todennus** käyttämään organisaation tilille. Esimerkiksi milloin käytettäessä muu kuin toimialueen liittyneet tietokoneeseen.

    Huomautus: Jos et näe Active Directory-todennus, joudut ehkä [Azure Active Directory-todennuksen käyttöön ottaminen](#enable-azure-active-directory-authentication) SSMS.

    ![Yhdistä SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Azure palvelinta hallinta käyttämällä SSMS on paljon sama kuin hallinnasta on paikallinen-palvelin, ei tutustutaan salliva asetus tietoja tässä. Kaikki ohjeessa sinun on löytyy MSDN-sivuston [Analysis Services esiintymän hallinta](https://msdn.microsoft.com/library/hh230806.aspx) .

## <a name="server-administrators"></a>Palvelimen järjestelmänvalvoja
Voit käyttää **Analysis Services-järjestelmänvalvojien** hallinta-sivu Azure portal tai SSMS palvelinta hallittavan palvelimen järjestelmänvalvojille. Analysis Services-valvojat ovat tietokanta palvelimen järjestelmänvalvojille yleisiä tietokannan hallintatehtäviä, kuten lisääminen ja poistaminen tietokantojen ja käyttäjien hallinta oikeudet. Oletusarvon mukaan käyttäjä, joka luo palvelimen Azure-portaalissa lisätään automaattisesti Analysis Services-järjestelmänvalvojana

Sinun tulee tietää myös:

-   Windows Live ID ei ole tuettu tunnistetietojen tyyppi Azure Analysis Services-palveluissa.  
-   Analysis Services-järjestelmänvalvojien on oltava kelvollinen Azure Active Directory-käyttäjiä.
-   Jos luot Azure Analysis Services-palvelimen kautta Azure Resurssienhallinta mallit, Analysis Services-järjestelmänvalvojien Vie JSON-taulukko, jonka käyttäjät, jotka on lisättävä järjestelmänvalvojat.

Analysis Services-järjestelmänvalvojat voivat olla eroaa Azure resurssin Järjestelmänvalvojat, jossa voit hallita resursseja Azure-tilaukset. Tämä ylläpitää aiemmin XMLA ja TSML yhteensopivuuden hallinta toiminnan Analysis Services-palveluissa ja sinulle tulleista välillä Azure resurssien hallintaa ja analysointia varten eroteltava Services-tietokannan hallinta.

Näytä kaikki roolit ja käsitellä Azure Analysis Services-resurssin tyypit, käytä ohjausobjektin sivu valvoa (IAM).

## <a name="database-users"></a>Tietokannan käyttäjiä
Azure Analysis Services-mallin tietokannan käyttäjiä on oltava Azure Active Directory. Mallin tietokannalle määritettyä käyttäjänimet on oltava organisaation sähköpostiosoite tai UPN. Tämä on eri paikallisen mallin tietokannoista, jotka tukevat käyttäjien Windows toimialueen käyttäjänimet mukaan.

Voit lisätä käyttäjiä [roolimäärityksiä Azure Active Directoryn](../active-directory/role-based-access-control-configure.md) avulla tai käyttämällä SQL Server Management Studiossa [Taulukkomuotoinen malli komentosarjan Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL).

**Esimerkki TMSL komentosarja**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>Azure Active Directory-todennuksen käyttöön ottaminen
Ottaa käyttöön SSMS rekisterin Azure Active Directory-todennus-ominaisuutta, Luo tekstitiedosto nimeltä EnableAAD.reg, kopioi ja liitä seuraavasti:


```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

Tallenna ja suorita sitten tiedosto.



## <a name="next-steps"></a>Seuraavat vaiheet
Jos olet jo eivät ole käyttöön Analysis Services-palvelimen uusi palvelimeen, nyt on hyvä. Lisätietoja on artikkelissa [Azure Analysis Servicesin käyttöönotto](analysis-services-deploy.md).

Jos olet ottanut mallin palvelimeen, voit ryhtyä muodostamaan yhteyden asiakkaan tai selaimen avulla. Lisätietoja on artikkelissa [Azure Analysis Services-palvelimesta tietojen noutaminen](analysis-services-connect.md).
