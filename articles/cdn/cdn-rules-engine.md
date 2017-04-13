<properties
    pageTitle="HTTP oletustoiminnan Azure CDN käyttämällä säännöt-ohjelma ohittaminen | Microsoft Azure"
    description="Voit mukauttaa pyyntöjen käsittelytavan mukaan Azure CDN, kuten estää sen lähettämisen tietyn tyyppistä sisältöä, välimuistiin koskevien käytäntöjen määrittäminen ja muokata HTTP-otsikoiden säännöt-ohjelma."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="override-default-http-behavior-using-the-rules-engine"></a>Ohita HTTP oletustoiminnan käyttämällä säännöt-ohjelma

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Yleiskatsaus

Säännöt-ohjelma voit mukauttaa miten pyyntöjen käsitellään, kuten estää sen lähettämisen tietyn tyyppistä sisältöä, välimuistiin käytännön määrittäminen ja muokkaaminen HTTP-otsikot.  Tässä opetusohjelmassa näytetään, luot säännön, joka muuttuu CDN varat välimuistiin toimintaa.  On myös videosisältöä käytettävissä "[Katso myös](#see-also)-osan.

## <a name="tutorial"></a>Opetusohjelma

1. Valitse **hallinta** -painike CDN profiili-sivu.

    ![CDN profiilin sivu hallinta-painike](./media/cdn-rules-engine/cdn-manage-btn.png)

    CDN hallinta-portaalin avautuu.

2. Valitse **HTTP suuri** -välilehdessä **Säännöt ohjelma**perään.

    Uuden säännön asetukset näytetään.

    ![CDN uuden säännön asetukset](./media/cdn-rules-engine/cdn-new-rule.png)

    >[AZURE.IMPORTANT] Järjestyksessä, jossa useita sääntöjä on lueteltu vaikuttaa siihen, miten ne käsitellään. Seuraavien sääntöjen voi ohittaa määrittämää edellisen säännön toiminnot.
    
3. Kirjoita nimi- **nimi ja kuvaus** tekstiruutu.

4. Määritä säännön koskevat pyynnöt tyyppi.  Oletusarvon mukaan **aina** vastine-ehto on valittuna.  **Aina** käyttäminen tässä opetusohjelmassa, joten jätä valittuna.

    ![CDN vastine-ehto](./media/cdn-rules-engine/cdn-request-type.png)

    >[AZURE.TIP] Valitse avattavasta käytettävissä olevat ehdot on monenlaisia vastine.  Napsauttamalla sinistä tiedoksi kuvaketta vasemmalla puolella vastine-ehto kerrotaan yksityiskohtaisesti valittuna ehto.
    >
    >Yksityiskohtaiset tiedot ehtoja vastaavat koko luetteloon Tutustu [säännöt ohjelma vastaa ehto ja toiminto tiedot](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_0).

5.  Valitse **+** voit lisätä uuden toiminnon **Ominaisuudet** -painiketta.  Valitse avattavasta valikosta vasemmalla **Voimassa sisäinen Maks ikä**.  Kirjoita tekstiruutuun, joka tulee näkyviin, **300**.  Jätä muut oletusarvot.

    ![CDN-ominaisuus](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] Kuin vastine vaatimuksia napsauttamalla sinistä tiedoksi kuvaketta vasemmalla puolella uusi ominaisuus näyttää tietoja tätä ominaisuutta.  **Voimassa sisäinen Maks ikä**, kyseessä on ovat ohittaminen kohteen **Välimuisti-komponenttien** ja **Expires** -otsikot, voit hallita, kun CDN reuna-solmu päivitys kohteen alkuperäisestä resurssi.  Tässä esimerkissä 300 sekunnin tarkoittaa CDN reuna-solmu välimuistiin kohteen 5 minuuttia ennen päivittämistä alkuperäisestä resurssi.
    >
    >Katso täysi luettelo toiminnoista yksityiskohtaisesti, [säännöt ohjelma vastine ehto ja toiminto tiedot](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).

6.  Jos haluat tallentaa uuden säännön valitsemalla **Lisää** .  Uusi sääntö on nyt hyväksyntää. Kun se on hyväksytty-tilaksi muuttuu **Odotetaan XML** : stä **Aktiivinen XML**.

    >[AZURE.IMPORTANT] Sääntöjen muutokset saattaa kestää enimmillään 90 minuuttia leviäminen CDN kautta.

## <a name="see-also"></a>Katso myös
* [Azure perjantaisin: Azure CDN tehokkaita uusia Premium ominaisuuksia](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)
* [Sääntöjen ohjelma vastine ehto ja toiminto tiedot](https://msdn.microsoft.com/library/mt757336.aspx)
