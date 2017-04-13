<properties
   pageTitle="Ohje tietojen palvelun luomiseen Marketplacen | Microsoft Azure"
   description="Lisätietoja siitä, miten luominen, varmentaminen ja ottaa käyttöön tietojen palvelun hankkia Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Palvelun tietojen julkaiseminen Azure Marketplace-opas

>[AZURE.IMPORTANT] **Tällä hetkellä onboarding olemme eivät ole enää mitään uusi tietopalvelu julkaisijat. Uusi dataservices ei Hae hyväksyä luettelo.** Jos haluat julkaista AppSource SaaS yrityssovelluksen löydät lisätietoja [tähän](https://appsource.microsoft.com/partners). Jos sinulla IaaS sovellusten tai haluat julkaista Azure Marketplace-Kehitystyökalut-palvelun löytyy lisätietoja [tästä](https://azure.microsoft.com/marketplace/programs/certified/).

Kun olet suorittanut vaihe 1- [tilin luominen ja rekisteröintiä](marketplace-publishing-accounts-creation-registration.md), emme ohjattu voit [Yleiset tehtävistä selainpohjaisista](marketplace-publishing-pre-requisites.md) ja tietopalvelu tarjouksen [Tekniset vaatimukset](marketplace-publishing-data-service-creation-prerequisites.md) Azure Marketplace. Nyt on edetään ohjatusti [Julkaisemisportaali] tietopalvelu tarjouksen luomisen vaiheet[ link-pubportal] Azure Marketplacen.

## <a name="1---login-to-the-publishing-portal"></a>1. Kirjaudu sisään, julkaisu Portal.

Siirry [https://publish.windowsazure.com](https://publish.windowsazure.com. )

**Julkaisemisportaali ensimmäisen kerran kirjautumiset Käytä samaa tiliä, jolla Developer Center rekisteröity yrityksen myyjän profiilin.**  (Myöhemmin voit lisätä minkä tahansa yrityksen työntekijä Julkaisemisportaali Mää-järjestelmänvalvojaksi).

Valitse **Julkaise Data Services** -ruutu, jos tämä on ensimmäinen kirjautuminen julkaisun portaaliin.

## <a name="2---choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2. Valitse **Tietopalvelut** siirtymisvalikkoa vasemmassa reunassa.

  ![piirustuksen](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3---create-a-new-data-service"></a>3. Luo uudet tiedot-palvelu

Täytä oman uudet tiedot palvelun tarjota otsikko ja valitse Valitse "+" oikealla.

  ![piirustuksen](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4---review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4. Valitse juuri luomasi tietojen palvelun hakuruudussa siirtymisvalikkoa alivalikosta Tarkista.

Valitse **vaihe vaiheelta** -välilehdessä ja tarkastele kaikki tarvittavat vaiheet, voit julkaista oikein Azure Marketplace-tiedot-palvelun.

> [AZURE.TIP] Voit aina napsauttamalla "Ongelmatilanteita"-sivun linkkejä tai välilehtien käyttäminen tietojen tarjouksen alivalikosta vasemmassa reunassa.

## <a name="5---create-a-new-plan"></a>5. Luo uusi suunnitelma.

### <a name="offers-plans-transactions"></a>Tarjoukset, suunnitelmat, tapahtumat.

Kunkin tarjouksen voi olla useita suunnitelmat, mutta se on oltava vähintään yksi (1) suunnitelma. Loppukäyttäjien tilaaminen tarjous ne tilaa johonkin tarjouksen suunnitelma. Kunkin suunnitelman määrittää, miten loppukäyttäjien voi käyttää palvelua.

Azure Marketplacesta tällä hetkellä tueta vain kuukausittain tilauksen tapahtuman mallin tietopalvelujen, eli loppukäyttäjien maksaa kuukausittain maksu hinnan, ne on tilattu tietyn suunnitelman mukaisesti ja voivat tarjoaman tapahtuman suunnitelma määrittämiä kunkin kuukauden numero.

Lähetetään palveluun kyselyyn perustuvaa myyntitapahtumien määritellään yleensä Data-palvelu palauttaa tietueiden määrän. Oletusarvo on 100. Kukin kysely palauttaa tapahtumien määrä on luku tietueiden tulos jaetaan luvulla 100 ja pyöristetty ylöspäin lähimpään kokonaislukuun.

Azure Marketplace-palvelun kerroksen vastuu seurannassa (mittari) kummankin kyselyn käyttämä tapahtumien määrä on.

> [AZURE.IMPORTANT] Loppukäyttäjät, johon tapahtuma raja kuukauden aikana saavutettu estetään jatkuvasta saakka niiden kuukausittain tilauksen jakson-palvelun käyttöä varten.

> Suunnitelman tai jokin palvelupaketin voit (mutta ei on) ovat tapahtumia rajoittamaton määrä.

### <a name="create-a-plan"></a>Suunnittele.
1. Valitse **"+"** "Lisää uusi suunnitelma"-kohdan vieressä.

2. Valitse jokin seuraavista vaihtoehdoista: Tämä suunnitelma **Rajoittamaton** tai **rajoitettu** käyttö.  Jos rajoitettu annettava tapahtuman määrä suunnitelman sallii tarjoaman kuukaudessa.

    ![piirustuksen](media/marketplace-publishing-data-service-creation/step-5.1.png)  

    Julkaiseminen Portal ehdottaa myös "Suunnitelman tunnus", jonka viestintä loppukäyttäjien käyttöliittymässä suunnitelman nimeä käytetään ja käyttää myös Marketplace-palvelu tunnistaa suunnitelma. Voit halutessasi muuttaa "Suunnitteleminen tunnus".

    > [AZURE.NOTE] "Suunnitteleminen tunnus" on oltava yksilöllinen kunkin tarjouksen aluetta. Kuin muiden tunnisteet julkaisun Portal suunnittelu-tunniste lukitaan, kun ensimmäinen julkaisu tuotannon ja määrität ei voi muuttaa tunnus.

3. Hyväksy valintasi napsauttamalla tätä.

4. Valitse ohjelma pyytää sinua joitakin muita kysymyksiä koskeva juuri luomasi suunnitelma.

    ![piirustuksen](media/marketplace-publishing-data-service-creation/step-5.2.png)


|Kysymys|Tarkkuus|
|----|----|
|**Tätä palvelupakettia ei maksuttomia ja saatavilla maailmanlaajuisesti?**|Voit luoda täysin vapaasti-,-maksu-suunnitelma. Jos se on tämän tarjouksen – vain suunnitteleminen se tarkoittaa, että julkaiset "Vapaa tarjota" Marketplacesta. Jos se on tarkoitettu vain yksi (vähän)-palvelupakettia, se antaa vaihtoehdon tarjota loppukäyttäjien lisätietoja palvelussa on suhteellisen vähän tapahtumien kuukaudessa.  Jos vastaus on "Kyllä", sinua pyydetään ei ole vielä kysymyksiä.|

> [AZURE.NOTE] Loppukäyttäjät aina päivittää käyttämään maksettu.

|Kysymys|Tarkkuus|
|----|----|
|**On ilmainen kokeiluversio?**|Voit valita "Ei-kokeiluversio" lainkaan tai anna asetus, kun suunnitelmasi "Kuukausi". Julkaisijat, kuten tämän asetuksen avulla voit antaa loppukäyttäjien mahdollisuus ymmärtää kuukautta maksutta tarjousta edut.|

> [AZURE.IMPORTANT] Loppukäyttäjien vain voi ostaa maksuttoman kokeiluversion käyttäjäksi, jos ne on muodostettu maksuvälineen esimerkiksi luottokortin enterprise Agreement-sopimus.

> Maksuttoman kokeiluversion kuukausi, kun Azure Marketplacesta alkavat charging asiakkaat tilauksen päivämäärästä hinnan paitsi asiakkaan aloittanut tilauksen peruuttaminen. Erityistä puheluilmoitusta toimitetaan käyttäjät.

|Kysymys|Tarkkuus|
|----|----|
|**Tätä palvelupakettia edellyttää Kampanjakoodi ostamista?**| Julkaisijoilla on vaihtoehto käyttörajoitukset Service-Palvelupaketit antamalla koodia nimeltä "A Promocode" tietyn asiakkaille. Vain tämän Promocode joiden loppukäyttäjien voi tilata suunnitelma. Jos valitset "Ei", sitten asiakas hyväksyy, että kaikki alue, jossa tarjous on saatavilla (katso [Marketplace markkinoinnin sisällön opas](marketplace-publishing-push-to-staging.md) lisätietoja) voivat tilaa tämä suunnitelma. Ei ole vielä kysymyksiä pyydetään.|
|**Myös piilottaa keneltä, joilla ei ole kelvollinen Kampanjakoodi tätä palvelupakettia?**|Jos edelliseen kysymykseen vastaus on "Kyllä" julkaisija on vaihtoehto poistaminen kokonaan näkymisen on Marketplace Käyttöliittymän tätä palvelupakettia. Se tarkoittaa sitä, asiakkaat eivät näe tätä palvelupakettia tarjouksen tiedot-sivulla. Loppukäyttäjien, jotka saavat promocode ostaa sen voi tilata käyttämällä tämän promocode.|

## <a name="6---create-your-marketplace-marketing-content"></a>6. luoda oman sisällön markkinoinnin Marketplace
Lisätietoja Anna vaaditut tiedot **markkinointi, hinnoittelu, tuen ja luokat** välilehtien Ota Tutustu [Marketplace markkinoinnin sisällön opas](marketplace-publishing-push-to-staging.md) , joka on yhteinen kaikkia tietoja, jotka on julkaistu Azure Marketplacesta.  

## <a name="7---connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7. tarjous muodostaa yhteyden palveluun (SQL Azure-pohjainen tai WWW-palvelun mukaan).

Valitse **Tietopalvelut** alivalikossa.

Valitse sivun yläosassa, sinua pyydetään antamaan tarjouksen **Namespace**.  

  ![piirustuksen](media/marketplace-publishing-data-service-creation/step-7.png)

Kysymyksen alapuolella määrittää kuinka julkaisijan siirtymällä vastaluotu paljastus tarjous Azure Marketplacesta. (Katso lisätietoja [Data Services tekninen valmistelevat opas](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![piirustuksen](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Julkaiseminen perustuva tietokanta-palvelu**

Valitse **tietokanta**. Seuraava sivu tulee näkyviin:

  ![piirustuksen](media/marketplace-publishing-data-service-creation/step-7.3.png)

Voit luoda CSDL yhdistämismäärityksen SQL Azure-DB perusteella tietojoukko:

  ![piirustuksen](media/marketplace-publishing-data-service-creation/step-7.4.png)

Ja valitse sitten kullekin taulukolle

  ![piirustuksen](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![piirustuksen](media/marketplace-publishing-data-service-creation/step-7.6.png)

Jos Web-palvelu

  ![piirustuksen](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [AZURE.IMPORTANT] Tässä artikkelissa [aiemmin luodun WWW-palvelun muodostaminen OData-CSDL yhdistäminen](marketplace-publishing-data-service-creation-odata-mapping.md) yksityiskohtaisia ohjeita ja esimerkkejä CSDL verkkopalvelun luominen.

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet luonut tietopalvelu tarjous, varmista, että [Marketplace markkinoinnin sisällön opas](marketplace-publishing-push-to-staging.md) ohjeita suorittaa loppuun, ennen kuin siirryt seuraavaan [Tiedot-palvelussa väliaikaisen](marketplace-publishing-data-service-test-in-staging.md)testaamiseen.

## <a name="see-also"></a>Katso myös
- [Aloittaminen: Miten voit julkaista tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md)
- Jos olet kiinnostunut tietoja yleinen OData kartoituksen ja tarkoitus, lue artikkeli [Tietoja palvelun OData yhdistämisen](marketplace-publishing-data-service-creation-odata-mapping.md) tarkistettava määritykset, rakenteet ja ohjeita.
- Jos olet kiinnostunut liittyviä tietoja tietyn solmut ja niiden parametrit määritykset ja selityksiä sekä esimerkkejä [Tietojen palvelun OData yhdistäminen solmujen](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) tässä artikkelissa ja käyttää kirjainkoon kontekstissa.
- Jos olet kiinnostunut tarkistaminen esimerkkejä, lue tämän artikkelin [Tiedot palvelun OData yhdistäminen esimerkkejä](marketplace-publishing-data-service-creation-odata-mapping-examples.md) otoksen koodi ja ymmärtää kenttäkoodin syntaksi ja kontekstia.


[link-pubportal]:https://publish.windowsazure.com
