<properties
    pageTitle="Määritteen perustuva sovelluksen valmistelu rajauksen suodattimet | Microsoft Azure"
    description="Opettele tarkasteltavan suodattimilla voit estää objektit, jotka tukevat automaattinen käyttäjän valmistelu todella valmistellaan Jos objekti ei täytä business-vaatimukset-sovelluksissa."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="attribute-based-app-provisioning-with-scoping-filters"></a>Määritteen perustuva sovelluksen valmistelu rajauksen suodattimet

Tässä osassa tavoitteena on kerrotaan, kuinka voit määrittää määrite perustuva sääntöjä, jotka määrittävät, mitä käyttäjien valmistelemiseen sovelluksen tarkasteltavan suodattimien avulla.





## <a name="clauses-and-scope-groups"></a>Lausekkeita ja laajuus-ryhmät


![Vaikutusalueen määrittäminen suodatin][1] 




Rajaus suodattaa on määritetty vähintään yksi **laajuus ryhmät**, kukin ottavat pidä vähintään yksi **lauseita**. Jos haluat nähdä tietyn laajuiset ryhmän lauseet, laajenna se napsauttamalla ryhmänimen vasemmalla puolella olevaa nuolta.

**Lause** määrittää käyttäjät, jotka saavat välitystä tarkasteltavan suodattimen arvioimisen kunkin käyttäjän määritteet. Voit joutua esimerkiksi yksi lause, joka edellyttää, että käyttäjän "tila-määrite on sama kuin New York, mikä tarkoittaa, että vain New Yorkissa käyttäjien valmistelemiseen sovellukseen.

![Vaikutusalueen määrittäminen ryhmänimi][2] 



Kunkin **ryhmässä** alkaa yksi pakollinen **lauseen**, kuten edellä olevassa näyttökuvassa. Tämä lause yksinkertaisesti, joka ilmaisee, että käyttäjän ensin määritettävä sovelluksen ennen kuin se arvioidaan tarkasteltavan suodattimilla. Tämä lause ei voi poistaa tai muokata.

Voit lisätä uusia lauseita tai uusia laajuus ryhmiä napsauttamalla sopivaa painiketta painamalla. Voit tarkastella kunkin alueen ryhmän nimi muokkaamalla sen **Alueen ryhmänimi** -ominaisuus.





## <a name="how-scoping-filters-are-evaluated"></a>Miten arvioidaan vaikutusalueen määrittäminen suodattimet

Aikana valmistelu, Testaa määrittääksesi, jos käyttäjä ansaitsee sovelluksen käyttöoikeuksia tarkasteltavan suodattimiin jokaisen määritetty käyttäjä. Voit ajatella kunkin lauseessa testi, jolla on siirrettävä, jotta voit välttää käytön suodattanut pois käyttäjän käytössä. 

Jos sinulla on useita laajuus ryhmiä, jotka on määritetty, kukin käyttäjä on välitettävä vähintään yksi jotta voit käyttää sovellusta. Laajuus kunkin ryhmän kuitenkin käyttäjän täytyy välittää jokaisen yhden lauseen välittää tiettyyn alueeseen ryhmän. 

Toisin sanoen Voit ajatella laajuus ryhmät siten, että tai yhdistää, ja Voit ajatella lauseet sisällä siten, että ja yhdistää. Esimerkkinä alla tarkasteltavan suodatinta:


![Vaikutusalueen määrittäminen ryhmänimi][2]  


Tästä tarkasteltavan suodatettu käyttäjät on täytettävä seuraavat ehdot, jotta valmisteltava:

1. Ne on määritettävä sovellukseen.

2. Ne on työskenneltävä tekniikka osasto

3. Niiden on oltava työn San Francisco tai Kanadassa.


##<a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Automatisoida käyttäjän valmistelu ja valmistelun SaaS sovellusten poistaminen](active-directory-saas-app-provisioning.md)
- [Määritteiden yhdistämismääritykset käyttäjän valmisteluun mukauttaminen](active-directory-saas-customizing-attribute-mappings.md)
- [Määritteiden yhdistämismääritykset lausekkeiden kirjoittamiseen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Tilin valmistelu ilmoitukset](active-directory-saas-account-provisioning-notifications.md)
- [Ota käyttöön automaattinen valmisteleminen käyttäjien ja ryhmien Azure Active Directorysta sovellusten SCIM avulla](active-directory-scim-provisioning.md)
- [Luettelo siitä, miten voit integroida SaaS sovellusten opetusohjelmat](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./active-directory-saas-scoping-filters/ic782813.png
