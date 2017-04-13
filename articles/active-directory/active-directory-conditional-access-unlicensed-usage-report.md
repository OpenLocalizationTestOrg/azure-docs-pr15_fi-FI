<properties
    pageTitle="Ei käyttöoikeutta käyttöraportti | Microsoft Azure"
    description="Käyttöoikeudettomien käyttäjien, jotka käyttävät selvittäminen käyttöoikeudeton käyttö raportti auttaa maksettu Azure AD-ominaisuuksia."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="unlicensed-usage-report"></a>Ei käyttöoikeutta käyttöraportti

Käyttöoikeudettomien käyttäjien, jotka käyttävät selvittäminen käyttöoikeudeton käyttö raportti auttaa maksettu Azure AD-ominaisuuksia. Näin voit tehdä paremmin käyttäminen käyttöoikeudet, jotka on ostettu ja tunnistavan tiedät kun joudut ehkä lisäkäyttöoikeuksia. 

Raportti näyttää aktiivisen maksettu ominaisuuksien käyttö viimeisten 30 päivän aikana. 

## <a name="report-structure"></a>Raportin rakenne
 
| Sarakkeen nimi          |    Kuvaus |
| :--                  | :--         |
| Käyttöoikeudettomien käyttäjien      |    Käyttäjän nimi |
| Toiminto              | Ominaisuuden nimi. Esimerkki: ehdollisen käyttöoikeuden |
| Käyttää sovelluksen | Sovellus, jota käytetään ominaisuuden nimi. Esimerkki: Office 365: n SharePoint Onlinessa |

 
> [AZURE.NOTE] Jos käyttäjätili on poistettu tunnus, kuten 1003000090D8B285 täytetään "Käyttöoikeudeton käyttäjä"-sarake


## <a name="conditional-access-feature"></a>Ehdollinen access-ominaisuus

Käyttöoikeudettomien käyttäjien merkitään, kun he käyttävät palvelu, joka on käytössä, jos hänellä ei ole Azure AD Premium-käyttöoikeuden ehdollinen käyttöoikeuskäytäntö. 

Tämä koskee MFA / sijainti käytäntöjä sekä laitteen käytännöt, jotka käyttävät Intune.
 

## <a name="see-also"></a>Katso myös

- [Ehdollinen Accessin käyttäminen Office 365: ssä ja muut Azure Active Directoryn yhdistetty sovellukset](active-directory-conditional-access.md)
- [Azure AD ehdollisen käyttöoikeuden käytön aloittaminen](active-directory-conditional-access-azuread-connected-apps.md) 


