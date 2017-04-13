<properties
 pageTitle="Ajoituksen suuren käytettävyyden ja luotettavuutta"
 description="Ajoituksen suuren käytettävyyden ja luotettavuutta"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/16/2016"
 ms.author="deli"/>


# <a name="scheduler-high-availability-and-reliability"></a>Ajoituksen suuren käytettävyyden ja luotettavuus

## <a name="azure-scheduler-high-availability"></a>Azure ajoituksen suuren käytettävyyden

Core Azure ympäristö palvelun Azure ajoitus on erittäin käytettävissä ja geo tarpeettomat palvelun käyttöönoton ja geo aluekohtaiset työn replikoinnin ominaisuudet.

### <a name="geo-redundant-service-deployment"></a>GEO tarpeettomat palvelun käyttöönotto

Azure ajoitus on Käyttöliittymä, joka on jo tänään Azure geo lähes kaikki alueen kautta. Alueiden tietueet, jotka Azure Scheduler ei käytettävissä on [Tässä luettelossa](https://azure.microsoft.com/regions/#services). Tietokeskuksen isännöityä alueen muodostetaan ei ole käytettävissä, jos Azure ajoituksen automaattisesti-ominaisuuksia on siten, että palvelu on käytettävissä MSDN toiseen data Centerissä.

### <a name="geo-regional-job-replication"></a>GEO aluekohtaiset työn sallittuja

Ei ainoastaan Azure-ajoitus on edusta hallinta pyynnöt, mutta itse työn käytettävissä on myös geo replikoida. Kun jonkin alueen on käyttökatkosta, Azure ajoituksen epäonnistuu päälle ja varmistaa, että projektin suorittaminen pisteparin maantieteellisen alueen toisen tietokeskuksen.

Esimerkiksi jos olit luonut työn Etelä keskitetyn Meille, Azure ajoituksen automaattisesti kopioi kyseisen työn Pohjois keskitetyn US. Kun-Etelä keskitetyn US on ilmennyt virhe, Azure ajoituksen varmistaa työ suoritetaan meiltä Pohjois keskitetyn. 

![][1]

Tuloksena Azure ajoituksen varmistaa, että tietosi pysymisen rajoitusten sisällä saman laajempi maantieteellisen alueen Jos Azure virhe. Tuloksena on ei kaksoiskappaleet työtäsi vain, jos haluat lisätä suuren käytettävyyden – Azure ajoituksen sisältää automaattisesti suuren käytettävyyden-ominaisuuksia, joilla työt.

## <a name="azure-scheduler-reliability"></a>Azure ajoituksen luotettavuutta

Azure ajoituksen takaa omassa suuren käytettävyyden ja siirtyä sisäkkäistä käyttäjän luoma työt. Esimerkiksi työtäsi saa kutsua HTTP päätepiste, joka ei ole käytettävissä. Azure ajoitus yrittää malli suorittaa työtäsi onnistuu, antamalla vaihtoehtoinen vaihtoehdot virheen ratkaisemiseksi. Azure ajoituksen tekee kahdella tavalla:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Määritettävät uudelleen käytännön kautta "retryPolicy"

Azure ajoituksen avulla voit määrittää käytännön uudelleen. Oletusarvon mukaan työn epäonnistuu, jos ajoituksen pyritään työn uudelleen neljä kertaa 30 sekunnin välein. Yritä käytäntö on enemmän kohdetoimialueen (esimerkiksi kymmenen kertaa 30 sekunnin välein) voi määrittää uudelleen tai looser (esimerkiksi kaksi kertaa päivän välein.)

Esimerkki kun tämä voi auttaa voit luoda työn, joka suoritetaan kerran viikossa ja käynnistää HTTP-päätepisteen. Jos HTTP-päätepisteen on alaspäin muutaman tunnin, kun työ suoritetaan, voit ehkä halua odottamaan suorittaa uudelleen myös uudelleen oletuskäytäntö epäonnistuu, koska resurssit Lisää viikkoa. Tällaisissa tapauksissa voi määrittää uudelleen vakio uudelleen käytännön uudelleen joka kolmas tunti (esimerkiksi) sen sijaan, että 30 sekunnin välein.

Lisätietoja uudelleen käytännön määrittäminen, [retryPolicy](scheduler-concepts-terms.md#retrypolicy)viittaavat.

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Vaihtoehtoinen päätepisteen Configurability "errorAction" kautta

Jos Kohdepäätepisteen Azure ajoitus-työ pysyy saavuttamattomissa, Azure ajoituksen kuuluu takaisin vaihtoehtoinen Virheenkäsittely-päätepisteelle sen uudelleen käytännön toimenpiteiden jälkeen. Jos vaihtoehtoinen Virheenkäsittely-päätepisteen on määritetty, Azure ajoitus käynnistää sen. Vaihtoehtoinen päätepisteen Omat työt ovat käytettävissä menettämisen tapahtuessa virhe.

Esimerkkinä seuraavassa kuvassa Azure ajoituksen seuraa osumien New Yorkissa web-palvelu uudelleen sen käytännön. Kun uudelleenyritykset epäonnistuu, se tarkistaa, onko vaihtoehtoisen. Siirtyy eteenpäin jälkeen ja käynnistää tekeminen pyynnöt vaihtoehtoiseen uudelleen käytännön kanssa.

![][2]

Huomaa, että yritä käytännön koskee sekä alkuperäinen toiminnon vaihtoehtoinen virhe-toimintoa. On myös mahdollista, että vaihtoehtoinen virhe-toimintoa toiminnon tyyppi poiketa tärkeimmät toiminnon toiminnon tyyppi. Esimerkiksi kun tärkeimmät toiminto saattaa käynnistettäessä päätepisteen HTTP-virhe-toimintoa sen sijaan ehkä tallennustilan jonossa, palvelun bus jonossa tai palvelun bus aiheen toiminto, joka toimii lokiin.

Lisätietoja vaihtoehtoisen päätepisteen määrittämisestä on viitata [errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Katso myös

 [Ajoituksen ominaisuudet](scheduler-intro.md)

 [Azure ajoitus-käsitteistä, terminologiaan ja kohteen hierarkia](scheduler-concepts-terms.md)

 [Aloita käyttö ajoituksen Azure-portaalissa](scheduler-get-started-portal.md)

 [Suunnitelmien ja Azure ajoituksen Laskutus](scheduler-plans-billing.md)

 [Miten voit luoda monimutkaisia aikatauluja ja Lisäasetukset toistuminen Azure schedulerilla](scheduler-advanced-complexity.md)

 [Azure ajoituksen REST API-viittaus](https://msdn.microsoft.com/library/mt629143)

 [Azure ajoituksen PowerShell cmdlet-viittaus](scheduler-powershell-reference.md)

 [Azure ajoituksen rajoitukset, oletusarvot ja virhekoodit](scheduler-limits-defaults-errors.md)

 [Azure ajoituksen lähtevien yhteyksien todennusta](scheduler-outbound-authentication.md)


[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
