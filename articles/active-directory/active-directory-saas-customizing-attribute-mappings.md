<properties
    pageTitle="Mukauttaminen määritteiden yhdistämismääritykset | Microsoft Azure"
    description="Löydä mitä SaaS sovellusten Azure Active Directory-määrite yhdistämismääritykset siitä, miten voit muokata niitä sähköpostiosoite yrityksesi tarpeisiin."
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


# <a name="customizing-attribute-mappings"></a>Määritteiden yhdistämismääritykset mukauttaminen


Microsoft Azure AD tukee käyttäjän valmistelu kolmannen osapuolen SaaS sovelluksia, kuten Salesforce-, Google Apps ja muiden. Jos sinulla on käyttäjän valmistelu kolmannen osapuolen SaaS sovelluksen käytössä, Azure hallinta-portaalin ohjausobjektit määritettä arvonsa kutsutaan "määritettä yhdistämiseksi" määrityksen lomakkeessa.

Tällä määrite yhdistämismääritysten Azure AD käyttäjän ja kunkin SaaS-sovelluksen käyttäjän objektit esimääritettyjä joukko. Jotkin sovellukset hallita muita objekteja, kuten ryhmien tai yhteystietojen. <br> 
Voit mukauttaa oletusarvon määritteiden yhdistämismääritykset yrityksesi tarpeiden mukaan. Tämä tarkoittaa sitä, voit muuttaa tai poistaa aiemmin määritteiden yhdistämismääritykset tai luoda uusia määrite yhdistämismäärityksiä.

Azure AD-portaaliin voit käyttää tätä toimintoa valitsemalla määritteet SaaS-sovelluksen työkalurivillä.

> [AZURE.NOTE] **Määritteet** -linkki on vain, jos sinulla on käyttäjän valmistelu SaaS-sovelluksen käytössä. 


![Salesforce][1] 


Kun napsautat määritteet-työkalurivin Nykyinen yhdistämismäärityksiä, jotka on määritetty SaaS sovelluksen luettelo.

Seuraavassa näyttökuvassa näkyy esimerkki tämän ominaisuuden:



![Salesforce][2]  


Edellä olevassa esimerkissä näet, että linkitetyn **givenName** arvo lisätään objektia, jota hallitaan Salesforce- **Etunimi** -määritteen Azure AD-objekti.

Jos haluat mukauttaa määritteiden yhdistämismääritykset tai jos haluat palata mukautettuja asetuksia takaisin oletusarvo-asetuksiin, voit tehdä tämän liittyvät sovelluksen alaosassa olevalla työkalurivillä-painiketta.


![Salesforce][3]  


On määritettä määritykset, joita tarvitaan SaaS-sovellus toimii oikein. Määritteet-taulukossa liittyvän määritteen yhdistämismääritykset on **Kyllä** arvoksi **pakollinen** määrite. Jos määrite yhdistäminen on tarpeen, et voi poistaa. Tässä tapauksessa **Poista** ominaisuus ei ole käytettävissä.

Jos haluat muokata aiemmin määrite-määritys, valitsemalla yhdistäminen ja valitse sitten **Muokkaa**. Tämä näyttää valintasivun, jonka avulla voit muokata valitun määritteen.


![Muokkaa määritettä yhdistäminen][4]  



## <a name="understanding-attribute-mapping-types"></a>Tietoa määritteen yhdistäminen tyypit


Määritteen yhdistämisen voit määrittää, miten määritteet lisätään kaikille kolmannen osapuolen SaaS sovelluksen. Tuetut eri yhdistäminen tyyppejä on:

- **Suora** – kohdemäärite on kaikille linkitetyn objektin määritteen arvo Azure AD.


- **Vakion** – kohdemääritteen täytetään tietyn merkkijonon, joka on määritetty.


- **Lauseke** - kohdemäärite on kaikille komentosarjan kaltaisessa lausekkeen tuloksen perusteella. Lisätietoja on artikkelissa [Azure Active Directory-määrite yhdistämismääritykset kirjoittaminen lausekkeita](active-directory-saas-writing-expressions-for-attribute-mappings.md).


- **Ei mitään** - kohdemäärite on vasen muuttamattomia. Jos kohdemäärite on koskaan tyhjä, se täytetään, voit määrittää oletusarvon.



Lisäksi seuraavat neljä basic määrite yhdistämistä mukautettujen määritteiden yhdistämismäärityksiä tukea **Oletusarvo** -arvon määrittäminen. Oletusarvo-arvon määrittäminen varmistaa, että kohdemäärite lisätään arvo Jos arvoa Azure AD eikä kohde objektin.

Microsoft Azure AD on erittäin tehokkaasti soveltaminen synkronoinnin. Valmisteltu ympäristössä vain objekteja, jotka edellyttävät päivitykset käsitellään synkronoinnin jakson aikana. Määritteiden yhdistämismääritykset päivitys vaikuttaa synkronoinnin jakson suorituskykyä. Tämä johtuu siitä määritteen työnkulkuun yhdistäminen päivitys edellyttää hallitun objektit on reevaluated. Tästä syystä on suositellut paras käytäntö säilyttää useita peräkkäisiä muutoksia määritettä yhdistämismääritykset vähintään.


##<a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Automatisoida käyttäjän valmistelu ja valmistelun poistaminen SaaS sovelluksiin](active-directory-saas-app-provisioning.md)
- [Määritteiden yhdistämismääritykset lausekkeiden kirjoittamiseen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Vaikutusalueen määrittäminen suodattimet käyttäjän valmistelu](active-directory-saas-scoping-filters.md)
- [Ota käyttöön automaattinen valmisteleminen käyttäjien ja ryhmien Azure Active Directorysta sovellusten SCIM avulla](active-directory-scim-provisioning.md)
- [Tilin valmistelu ilmoitukset](active-directory-saas-account-provisioning-notifications.md)
- [Luettelo siitä, miten voit integroida SaaS sovellusten opetusohjelmat](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
