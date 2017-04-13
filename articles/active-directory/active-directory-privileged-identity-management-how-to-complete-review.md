<properties
   pageTitle="Miten access-Tarkista | Microsoft Azure"
   description="Kun olet aloittanut access-Tarkista Azure AD sellaisten jäsenyyksien hallinta, tietoja ne suoritetaan ja tarkasteleminen"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Miten access-Tarkista Azure AD sellaisten tunnistetietojen hallinta


Sellaisten roolin järjestelmänvalvojat voivat tarkastella sellaisten access kerran [tietoturva Tarkista on käynnistetty](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure AD-luottamuksellisten jäsenyyksien hallinta (PIM) lähettää automaattisesti sähköpostiviestin, jossa kehotetaan tarkistamaan heillä on käyttöoikeudet käyttäjille. Jos käyttäjä saanut sähköpostia, voit lähettää ne ohjeita [tietoja tietoturva Tarkista](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Kun tietoturva Tarkista kuluessa tai kaikille käyttäjille tehnyt niiden itse tarkistaa, voit hallita Tarkista sekä tarkastella tuloksia tämän artikkelin ohjeiden mukaisesti.

## <a name="manage-security-reviews"></a>Hallitse suojauksen tarkistukset

1. Siirry [Azure portal](https://portal.azure.com/) ja valitse raporttinäkymän **Azure AD sellaisten jäsenyyksien hallinta** -sovellus.
2. Valitse koontinäyttö **Access tarkistaa** -osassa.
3. Valitse access-tarkistus, jonka haluat hallita.

Valitse access-Tarkista tiedot-sivu on lukuasetukset hallintaan tarkastelu.

![PIM access Tarkista painikkeet - näyttökuva][1]

### <a name="remind"></a>Muistuta

Jos access-Tarkista on määritetty, jotta käyttäjät tarkastella itse, **Muistuta** -painike lähettää ilmoituksen. 

### <a name="stop"></a>Seis

Kaikki access-tarkistukset on päättymispäivän, mutta **Stop** -painikkeen avulla voit päättyminen etuajassa. Jos tällä hetkellä eivät ole tarkastelleet sellaisten käyttäjien, hän ei voi sen jälkeen, kun lopetat Tarkista. Arviota ei voi käynnistää uudelleen sen jälkeen, kun se pysäytetään.

### <a name="apply"></a>Käytä

Kun access-tarkistus on valmis, joko koska saavuttanut päivämäärä tai pysäyttää manuaalisesti, **Käytä** toteuttaa Tarkista lopputulos. Jos käyttäjän käyttö on estetty Tarkista, tämä on vaihetta, jonka avulla voit poistaa niiden roolimääritys.  

### <a name="export"></a>Vie

Jos haluat käyttää suojauksen tarkistuksen tulokset manuaalisesti, voit viedä Tarkista. **Vie** -painiketta alkaa lataaminen CSV-tiedostoon. Voit hallita hakutuloksia Excelissä tai muissa ohjelmissa, jotka avaavat CSV-tiedostoja.

### <a name="delete"></a>Poista

Jos et ole kiinnostunut Tarkista enempää, poista se. **Poista** -painike poistaa Tarkista PIM-sovelluksesta.

> [AZURE.IMPORTANT] Tulevat ei toiminto, joka varoittaa ennen poistamista, joten muista, että haluat poistaa tarkastelu.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
