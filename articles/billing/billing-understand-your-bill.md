<properties
   pageTitle="Laskun tietoja | Microsoft Azure"
   description="Lue, miten voit lukea ja ymmärtää käyttö- ja Azure tilauksen laskun"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/31/2016"
   ms.author="erihur;genli"/>


# <a name="understand-your-bill-for-microsoft-azure"></a>Microsoft Azure-ratkaisun laskun ymmärtäminen

> [AZURE.NOTE] Jos tarvitset apua milloin tahansa tämän artikkelin, ota [yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ongelmaa saat ratkaista nopeasti.

Microsoft Azure-tilaukset ovat vaihtelevat korko suunnitelma. Jotkin korko-Palvelupaketit, kuten Visual Studio Enterprise (MPN)-tilaajille sisältävät kuukausittaiset, jota voit käyttää minkä tahansa Azure-palvelusta on tarvittavat käyttöoikeudet.

Huomaa, että määrittäminen 24 tunnin piilevän käyttö edellisen laskutusjaksolla avulla voidaan raportoida nykyisen laskutusjakson.

Saat lisätietoja kulutus ja korko suunnitelmien [Microsoft Azure hankkiminen asetukset-sivulla](https://azure.microsoft.com/pricing/purchase-options/).

<!-- The below links cover a complete list of all Microsoft Azure services.

<!-- - [Service Details list (csv1)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
<!-- - [Service Details list (csv2)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)

<!-- *NOTE: The **csv1** link refers to the column header names for csv version 1 and **csv2** link refers to the new column header names for csv version 2.  These files are updated monthly.*-->

### <a name="view-or-download-a-bill-for-microsoft-azure"></a>Tarkastele tai ladata laskun Microsoft Azure:

1. Kirjaudu sisään [Tilille Center](https://account.windowsazure.com/subscriptions) käyttäen Microsoft-Account tai organisaation.

2. Valitse tilaus, johon haluat nähdä tiedot ja käyttö.

3. Valitse **Laskuhistorian**

    ![Yhteenveto - Laskuhistorian -1](./media/billing-understand-your-bill/ContentViewaBillforMA1.png)

4. **Laskutushistoria** -osassa on luettelo oman lauseet laskutuksen kausilta sekä laskuttamatta tilikauden. Tilikauden-lause on arvio maksuista arvio on luotu aikana. Nämä tiedot päivitetään vain päivittäin, ja kaikki kertyneet käyttö ei välttämättä sisällä. Kuukausittainen laskun voivat poiketa tässä arvio.  

    ![Yhteenveto-laskutushistorian 2](./media/billing-understand-your-bill/ContentViewaBillforMA2.png)

5. Valitse tarkasteltavan arvio arvio on luotu aikana maksuista **Nykyinen näkymä-lause** . Nämä tiedot päivitetään vain päivittäin, ja kaikki kertyneet käyttö ei välttämättä sisällä. Kuukausittainen laskun voivat poiketa tässä arvio.

    ![Yhteenveto-laskutushistorian 3](./media/billing-understand-your-bill/ContentViewaBillforMA3.png)

    ![Yhteenveto-laskutushistorian 4](./media/billing-understand-your-bill/ContentViewaBillforMA4.png)

6. Valitse **Lataa lasku** edellisen laskun kopion.

    ![Yhteenveto-laskutushistorian 5](./media/billing-understand-your-bill/ContentViewaBillforMA5.png)


> [AZURE.NOTE] Kulujen luettelossa Laskutus lauseet kansainväliset asiakkaiden on arvio tarkoitettu vain, kun tallenteiden on eri kustannukset muuntokurssit.

Seuraavassa on kaksi eri tarjouksia käytettävissä Microsoft Azure otoksen joitakin lauseita.

 Tarjouksen tyyppi | Kuvaus | Lataa |
 :--------- |:-------- | :-------|
Ryhmävakuutussopimukset | Maksaa kuukausittain jälkikäteen | [Mallitiedosto](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_ccinvoice_Sample.pdf)
Sitoumus tarjous | Silmäile vähennettävän-ennakkoon sitoumus | [Mallitiedosto](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_invoice_Sample.pdf)

## <a name="account-information"></a>Tilitiedot

Tilin tiedot-osassa tunnistaa olennaiset tiedot käyttö ja profiilin.

![ylätunniste](./media/billing-understand-your-bill/Header.png)

| Termi                | Kuvaus                                                                                         |
|---------------------|-----------------------------------------------------------------------------------------------------|
| Laskun numero         | Laskun yksilöllinen tunnus, seurantaa varten                                                   |
| Laskutus kehä       | Aikaväli, jossa käyttö on tapahtunut                                                       |
| Laskutuspäivämäärä        | Päivämäärä, joka on luotu lasku                                                                 |
| Maksutapaa.      | Maksutapoja käytetään tilin (laskulla tai luottokortilla)                                   |
| Laskutusosoitteen             | Microsoft Azure maksut osoite                                                                    |
| Tarjoukseen  | Tyypin tarjoukseen, jotka on ostettu (ryhmävakuutussopimukset, BizSpark Plus, Azure Pass jne.) |
| Tilin omistajan sähköposti | Tilin sähköpostiosoite, joka on rekisteröity Microsoft Azure-tili                      |

## <a name="understand-the-invoice-summary"></a>Tietoja laskun yhteenveto

Laskussa **Laskun yhteenveto** -osa on yhteenveto tapahtumat edellisen laskun ja nykyisen käyttömaksujen jälkeen.

![laskun yhteenveto](./media/billing-understand-your-bill/InvoiceSummary.png)

Edellinen saldo, maksut ja jäännöksen laskun osa on yhteenveto tapahtumat edellisen laskun jälkeen.

| Termi                                              | Kuvaus                                                                              |
|---------------------------------------------------|------------------------------------------------------------------------------------------|
| Edellinen saldo                                  | Tilauksen kokonaissumma määräpäivä viimeinen laskustasi                                                 |
| Maksut                                          | Maksut edellisen laskun käytetty                                                 |
| Avoin saldo (edellisen laskutusjakson) | Laskun säädöt (hyvitykset tai saldojen) käytetty tilisi edellisen laskun jälkeen  |

## <a name="understand-the-current-charges"></a>Nykyiset maksut ymmärtäminen
Laskussa nykyiset maksut-osan sisältää kuukausittain maksuja tietoja. Seuraavissa jaksoissa on järjestetty linkit.

| Termi          | Kuvaus                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Käyttökustannukset | Käyttökustannukset ovat kuukausittaiset maksut yhteensä-tilaus. Edellisen kuukauden käytön jälkikäteen on laskutettu.                                                                                                                                                                                                                                                                                                                                       |
| Alennus     | Palvelun alennuksia käytetty nykyisen laskun näkyvät myös rivi-kohdetta.                                                                                                                                                                                                                                                                                                                                              |
| Muutokset   | Muut muutokset ovat muiden hyvitykset tai avoin käytetty nykyisen laskun maksuja. Esimerkiksi jos Visual Studio yrityksen MSDN tarjouksen, voit nähdä tämän rivin kohteen kuukausittaiset luottotietojen. Jos peruutat tilauksen, näkyy kuukausittainen käyttö ylittävät nykyisen laskutusjakson alusta tarjous tilauksen peruuttaminen-päivämäärä sisältyy kuukausittain luottotietojen kulut.|

## <a name="footer-information"></a>Alatunnistetietojen
![alatunniste](./media/billing-understand-your-bill/footerinformation.png)

## <a name="understand-the-additional-information"></a>Tietoja lisätiedot
Lisätietoja-sivulla näet muita resursseja, saat lisätietoja laskun sekä linkit käyttö ja muut haluamasi tiedot laskun tarkasteleminen viittauksia.

![Lisätietoja](./media/billing-understand-your-bill/AdditionalInformation.png)

### <a name="detailed-usage"></a>Yksityiskohtainen käyttö
Linkin kuvaus- **Yksityiskohtaiset käyttö** -kohdassa ohjaa tilin keskelle, jossa voit tarkastella yksityiskohtaisia käyttö tähän tilaukseen.  On nyt kaksi versiota ladattavissa: **.csv-versio 1** on vanha nimeämiskäytäntöä ja käyttö kenttiä ja **.csv-version 2** sisältää asiakkaiden helpossa muodossa nimien kunkin luokat ja muita kenttiä, jotka auttavat sinua ymmärtämään, mitä palveluista käytetään Microsoft Azure. Huomaa, että .csv-versiossa 1, ei ole Azure Resurssienhallinta. Azure Resurssienhallinta tiedot löytyvät .csv-version 2.

### <a name="additional-information-and-useful-resources"></a>Lisätietoja ja hyödyllisiä resursseja
Tässä osassa on linkkejä yksinkertainen Laske esiintymän koot, SQL DB maksut ja hyödyllisiä linkkejä, jotta näet edelleen kysymyksiin liittyviä kysymyksiä.

| Termi                 | Kuvaus                                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Myyty              | Tämä on kopioidaan profiilin tilin osoitteella                                                                           |
| Maksuohjeet | Tässä osassa on, mihin lähettää tarkistukset, wire siirrot tai perillä tarkistukset, jos maksutavan laskusta maksuohjeet |

## <a name="understand-detailed-usage-charges"></a>Yksityiskohtainen käyttökustannukset ymmärtäminen

Osana Microsoftin jatkuvaa sitoumus asiakkaat voivat hallita niiden Azure käyttö on on parannettu Lataa käyttö tiedosto, joka ilmoittaa Azure palveluiden käyttö- ja kustannukset.  Lataa-linkki sisältää käyttö tiedoston kahden version:

- **Version 1** käyttää olemassa muotoa

- **Version 2** sisältää lisätiedot ja sarakkeiden nimet, valitse päivittäinen käyttö.  

Käyttö kulut ovat yhteensä **kuukausittain** vähemmän hyvitys- tai alennus-tilauksessa. Edellisen kuukauden käytön jälkikäteen on laskutettu.  Tiedoston yläosassa näkyy tietoja palveluja, sinun on laskutettava varten edellisen kuukauden laskutusjakson aikana.  Edellisessä taulukossa sarakkeiden nimet kunkin .csv-version tiedostot.

Versio 1 |  Version 2  |  Kuvaus|
:---------------| :---------------- | --------|
Laskutus piste | Laskutus piste | Kun resurssi on käytetty laskutusjaksolla.
Nimi | Mittarin luokka | Tunnistaa, jonka tämä käyttö kuuluu ylimmän tason palvelun.
Tyyppi | Mittarin aliraportti luokka | Azure-palvelu voidaan määrittää tarkemmin tässä sarakkeessa, jotka voivat vaikuttaa nopeus tyypin mukaan.
Resurssi | Mittarin nimi | Näet, että kulutettu resurssin mittayksikön.
Alue | Mittarin alue | Ilmoittaa joidenkin palvelujen, laitteiden hinnoittelu palvelinkeskuksen sijainnin perusteella palvelinkeskuksen sijainnin.
TUOTE | TUOTE | Näet kunkin Azure resurssin yksilöllinen järjestelmän-tunniste.
Yksikkö | Yksikkö | Tunnistaa, palvelun veloitetaan-yksikkö. Esimerkiksi gt, työaika, 10,000s.
Kulutettu | Kulutettu määrä | Sisältää resurssin, joka on jo käytetty laskutusjakson aikana.
Levy | Sisältää määrä | Sisältää resurssin, joka sisältyy nykyisen laskutusjakson maksutta.
Laskutettavat | Overage määrä | Jos Kulutettu määrä ylittää sen mukaan, tässä sarakkeessa näkyy erotus. Sinun on laskuttaa summa. Ryhmävakuutussopimukset tarjouksia, jossa ei ole mukana tarjous summa kokonaissumma on sama kuin Kulutettu määrä.
Sitoumus sisällä | Sitoumus sisällä | Sisältää resurssin maksut, jotka liittyvät 6 tai 12 kuukauden tarjous sitoumus oman summasta arvo. Resurssin maksuja ovat arvo sitoumus summasta aikajärjestyksessä.
Valuutta | Valuutta | Määrittää nykyisen laskutusjakson näkyvät valuutta.
Kattavuus | Kattavuus | Sisältää resurssin kulut, jotka ylittävät 6 tai 12 kuukauden tarjous liittyvä sitoumus oman summan.
Sitoumus korko | Sitoumus korko | Sisältää sitoumus korko, että kokonaismäärästä summa 6 tai 12 kuukauden tarjous liittyvät perusteella.
Korko | Korko | Korko näyttää perittävän kohti laskutettavan korko.
Arvo | Arvo | Näyttää korko-sarakkeen mukaan laskutettavat sarakkeen tulon. Jos Kulutettu määrä enintään mukana summa, ilmenee maksutonta tässä sarakkeessa.

## <a name="analyze-daily-usage-data"></a>Päivittäinen käyttötietojen analysointiin
Sen mukaan, käyttö voi olla tuhansia päivittäinen käyttö tietorivejä. Jos haluat analysoida tiedot, valitse **Lataa käyttö** ja valitse CSV-säädettävät tiedostoon (.csv)-version Nähdäksesi päivittäinen käyttö tietojen tarvittavat laskutusjaksolla.  Voit ladata kunkin versiolle otoksen .csv-tiedoston omaa käyttöäsi varten.

 Nimi | Lataa |
 :----------:| :-------: |
  Yksityiskohtainen käyttö .csv-versio 1|  [Mallitiedosto](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
  Yksityiskohtainen käyttö .csv-Version 2 | [Mallitiedosto](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)



![csv2screenshot](./media/billing-understand-your-bill/csv2screenshot.png)



Kohteet ovat .csv-tiedostossa eritellä saat näkyviin luettelon, kuinka paljon kullekin resurssille on käytetty nykyisen laskutusjakson kuluessa.

![CSV-tilannevedos](./media/billing-understand-your-bill/csvsnapshotportal.png)

Seuraavat sarakkeet näyttää tiedot, jotka vaikuttavat kurssit laskutuksen kauden alussa:

Versio 1 |   Version 2   |  Kuvaus |
:---------------| :----------------| -----|
Käyttö päivämäärä | Käyttö päivämäärä | Päivämäärä, jolloin resurssi on lähetetty.
Nimi | Mittarin luokka | Tunnistaa, jonka tämä käyttö kuuluu ylimmän tason palvelun.
Resurssin GUID-tunnus | Mittarin tunnus | Laskutettu mittarin tunnus.  Tämä käytetään tunnus, jolla hinta laskutuksen käyttö.
Tyyppi | Mittarin aliraportti luokka | Azure-palvelu voidaan määrittää tarkemmin tässä sarakkeessa, jotka voivat vaikuttaa nopeus tyypin mukaan.
Resurssi | Mittarin nimi | Näet, että kulutettu resurssin mittayksikön.
Alue | Mittarin alue | Ilmoittaa joidenkin palvelujen, laitteiden hinnoittelu palvelinkeskuksen sijainnin perusteella palvelinkeskuksen sijainnin.
Yksikkö | Yksikkö | Tunnistaa, palvelun veloitetaan-yksikkö. Esimerkiksi gt, työaika, 10,000s.
Kulutettu | Kulutettu määrä | Sisältää resurssi, joka on pitänyt käyttää kyseistä päivää.
Sub-alue | Resurssin sijainti | Tunnistaa palvelinkeskukseen, jossa resurssi on käynnissä.
Palvelun | Käytetty palvelu | Tämän sarakkeen on käytössä, voit seurata yksittäisiä Azure platform-palvelun, joka voi erikseen tunnistettu nimi-sarakkeen. Tämä palvelu sarake ilmaisee, mikä koskevien palvelun käyttö liittymisestä.
PUUTTUU | Resurssiryhmä | _**Uuden sarakkeen lisääminen.**_ Resurssiryhmä, jossa käyttöön resurssin suoritetaan. Lisätietoja [Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md)
Osa | Esiintymätunnus | Käynnissä olevan resurssin tunnus. Tunniste sisältää nimen, voit määrittää resurssin, kun se on luotu.
PUUTTUU | Tunnisteet | _**Uuden sarakkeen lisääminen.**_ Azure uusi resurssityypit mahdollistavat tunnisteen resurssit. Voit [järjestää ja tunnisteita Azure resurssien](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) viitata
Muut tiedot | Muut tiedot | Palvelun liittyviä metatietoja.
Palvelun tiedot 1 | Palvelun tiedot 1 | Tässä sarakkeessa on projektinimi, joka kuuluu palvelun tilauksen.
Palvelun tiedot 2 | Palvelun tiedot 2 | Tämä on vanha kenttä, joka sisältää valinnaisen palvelukohtaisia metatiedot.

Jotkin uudet kentät ja nimenmuutoksia CSV versio 2 Lisäksi siellä standardoitu tietojen muotoilu-kentät alla:

- **Esiintymätunnus**: Esiintymätunnus-kenttä edustaa käyttäjän määrittämän valmisteltu palvelun tunnus. Tällä hetkellä on kaksi muotoa, johon tunniste esitetään: on resurssin nimen tai täydellinen resurssin tunnus. Microsoft Azure-palveluiden siirtymässä edustavat tunniste standardoitu täydellinen Resurssitunnus-muodossa _**(/subscriptions/<subscription id>/resourcegroups/<resourcegroupname>/providers/<providername>/<resourcename>)**_. Kun services siirtymän uuteen muotoon tulee näkyviin Esiintymätunnus tietokenttä muuttaa resurssinimi resurssin tunnus. Resurssitunnus on muoto, jota [Azure Resurssienhallinta API](https://msdn.microsoft.com/library/azure/dn790567.aspx) tilauksen resurssien tunnistamiseen.

![esiintymän tunnus](./media/billing-understand-your-bill/instanceid.png)

- **Lisätietoja**: lisätiedot käyttö .csv-sarakkeessa määrittää palvelukohtaisia metatiedot. Esimerkiksi kuvan laji, AM. Tällä hetkellä palvelu-tietokoneesta kuuluu äänimerkki palvelukohtaisia metatietojen useassa sarakkeessa: lisätiedot, palvelun Info1 ja palvelun tiedot 2-kenttiin. Microsoft Azure-palvelut standardoimalla lähettävän palvelukohtaisia metatietojen vain lisätiedot-sarakkeessa.  Katso standardoitu muoto tilannevedoksen alla:

![additionalinfo_csv2](./media/billing-understand-your-bill/AdditionaInfo_csv2.png)

- **Tunnisteet**: sarake sisältää määritetyn käyttäjän resurssin tunnisteet. Tunnisteiden avulla voidaan ryhmitellä laskutuksen tietueita. Tunnisteiden avulla voit jakaa kustannukset-palvelun avulla osaston mukaan. Lisätietoja [tunnisteiden järjestämiseen Azure resurssit](../resource-group-using-tags.md). Palvelut, jotka tukevat säteilevä tunnisteet ovat:  

    - Näennäiskoneiden

    - Tallennustilan ja

    - Verkkopalvelut valmisteltu [Azure Resurssienhallinta Ohjelmointirajapinnan](https://msdn.microsoft.com/library/azure/dn790567.aspx) käyttäminen

![tunnisteet](./media/billing-understand-your-bill/tags.png)


## <a name="next-steps"></a>Seuraavat vaiheet

- [Laskutus ilmoitusten määrittäminen](../billing-set-up-alerts.md)

- [Maksutapasi hallinta](../billing-how-to-change-credit-card.md)

- [Tietoja Azure Marketplace-kulut](../billing-understand-your-azure-marketplace-charges.md)

- [Azure laskutus ja usein kysytyt kysymykset](../billing-subscription-faq.md)

> [AZURE.NOTE] Jos on edelleen muita kysymyksiä, ota [yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ongelmaa saat ratkaistu nopeasti.

<!--
OLD MSDN Articles
- [What do I do if my Azure subscription become disabled?](https://msdn.microsoft.com/library/azure/dn736049.aspx)
- [Edit payment information for an existing credit card](https://msdn.microsoft.com/library/azure/dn736053.aspx)
- [Add a new credit card to use as a payment method](https://msdn.microsoft.com/library/azure/dn736057.aspx)
- [Change the credit card on your Microsoft Azure account](https://msdn.microsoft.com/library/azure/dn736050.aspx)
- [Manage your payment method](https://msdn.microsoft.com/library/azure/dn736054.aspx)
-->



<!--Image references-->
