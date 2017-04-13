<properties
pageTitle="Paikallisen SQL Server-tietokannan tietojen käyttäminen koneen Learning | Azure"
description="Paikallisen SQL Server-tietokannan tietojen avulla voit suorittaa kehittyneen analyysin Azure koneen Learning kanssa."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Suorittaa kehittyneen analyysin Azure koneen Learning paikallisen SQL Server-tietokannan tietojen avulla


Usein yrityksille, jotka toimivat paikalliset tiedot haluat siirtää hyödyntää asteikko ja niiden konepohjaisten oppimistekniikoiden työmääriä Pilvipalvelun joustavuutta. Mutta ne eikä sinua häiritä niiden nykyiseen business prosesseja ja työnkulkuja siirtämällä niiden paikalliset tiedot pilvipalveluun. Azure koneen Learning tukee nyt tietojen luettaessa paikallisen SQL Server-tietokantaan ja sitten koulutus ja näkyvissä pistemäärä mallin näiden tietojen kanssa. Sinulla ei enää kopioida ja pilveen ja paikallisen palvelimen tietojen synkronoiminen manuaalisesti. Sen sijaan Azure koneen Learning Studiossa **Tietojen tuominen** -moduulin nyt luettavissa suoraan paikallisen SQL Server-tietokantaan oman koulutukseen ja näkyvissä pistemäärä työt. 

Tässä artikkelissa on yleiskatsaus siitä, miten tunkeutumisen paikallisen SQL server data Azure koneen Learning kyselyjä. Vaiheissa oletetaan, että olet käyttänyt Azure koneen Learning käsitteistä, kuten *työtilat, moduulit, tietojoukkoja, kokeissa jne*...

> [AZURE.NOTE] Tämä ominaisuus ei ole käytettävissä vapaa työtilojen. Saat lisätietoja tietokoneen Learning hinnat ja tasojen [Azure koneen Learning hinnat](https://azure.microsoft.com/pricing/details/machine-learning/).

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="install-the-microsoft-data-management-gateway"></a>Asenna Microsoft Data Management Gateway

Jos haluat käyttää Azure koneen Learning sinun on ladattava ja asennettava Microsoft Data Management Gateway-paikallisen SQL Server-tietokantaan. Kun määrität yhdyskäytävän yhteys tietokoneen Learning Studiossa on ladattava ja asennettava jäljempänä **lataaminen ja rekisteröi tietojen yhdyskäytävä** -valintaikkunan yhdyskäytävän mahdollisuuden.

Voit myös asentaa Data Management Gateway etuajassa lataamalla ja suorittamalla MSI-asennuspaketin [Microsoft Download Centeristä](https://www.microsoft.com/download/details.aspx?id=39717). Valitse uusimpaan versioon, 32-bittinen tai 64-bittinen sopiva tietokoneen valitseminen. MSI voidaan myös päivittää aiemmin luotuun Data Management Gatewayta uusimpaan versioon, kaikki säilötyt asetukset.

Yhdyskäytävä on seuraavat edellytykset:

- Tuetut Windows-käyttöjärjestelmien on Windows 7: ssä, Windows 8/8.1, Windows 10: ssä, Windows Server 2008 R2, Windows Server 2012: ssa ja Windows Server 2012 R2.
- Yhdyskäytävän tietokoneen suositellut määritykset on vähintään 2 GHz, 4 sydämiä, 8 gt RAM-Muistia ja 80 gt levy.
- Jos host koneen valmius-yhdyskäytävän ei voi vastata tiedot. Määrittää vuoksi asianmukaiset power suunnitelma tietokoneessa ennen asennusta yhdyskäytävän. Yhdyskäytävän asennuksen näyttää sanoman, jos tietokoneessa on määritetty horrostilan.
- Kopioi tehtävän tapahtuu tietyn korkojakso, koska resurssien käyttö (suorittimen ja muistin) tietokoneeseen seuraa myös samoissa huippu-ja vapaa-aikoja. Resurssien käytön vaikuttaa myös raskaasti siirretään tietojen määrää. Kun käynnissä on useita kopioi töitä tarkkailla resurssien käyttö piikin aikoina eteenpäin. Yllä olevassa luettelossa pienin määritysten ollessa tarkalleen ottaen riittävän haluat ehkä on enemmän resursseja kuin sen mukaan, että tiettyjä latautuvat tietojen siirtoa varten pienin määritysten kokoonpanon.

Ota huomioon seuraavat asiat, kun määrittämisestä ja käyttämisestä Data Management Gatewayn:

- Voit asentaa vain yhden esiintymän Data Management Gateway samassa tietokoneessa.
- Voit käyttää yhtä yhdyskäytävää paikallisen useista tietolähteistä.
- Voit yhdistää useita yhdyskäytäviä eri tietokoneissa samaa paikalliseen tietolähteeseen.
- Voit määrittää yhdyskäytävän vain yksi työtilan kerrallaan. Yhdyskäytävää ei voi jakaa muiden työtilojen tällä hetkellä.
- Voit määrittää yksittäisen työtilan useita yhdyskäytäviä. Jos esimerkiksi haluat käyttää yhdyskäytävän, joka on liitetty testi tietolähteet kehityksen aikana ja tuotannon yhdyskäytävän, kun olet valmis operationalize.
- Yhdyskäytävää ei tarvitse olla tietolähteenä samaan tietokoneeseen, mutta yhteyden pitäminen tietolähteen lähempänä vähentää ajan yhdyskäytävän muodostaa yhteyden tietolähteeseen. On suositeltavaa, että asennat yhdyskäytävän koneessa, joka ei ole sama kuin se, joka isännöi paikallisen tietolähteen niin, että yhdyskäytävän ja tietolähteen ei kilpailemaan resurssien.
- Jos sinulla on jo asennettu käsittelevä Power BI- tai Azure Data Factory skenaariota yhdyskäytävän, asenna erillisessä yhdyskäytävän for Azure koneen Learning toisessa tietokoneessa. 

    > [AZURE.NOTE] Et voi suorittaa Data Management Gateway ja Power BI-yhdyskäytävän samassa tietokoneessa.

- Haluat käyttää Data Management Gateway Azure koneen Learning vaikka käytössäsi Azure ExpressRoute muita tietoja. Käsittele tietolähteen paikallisen tietolähteeksi (joka on palomuurin takana) myös kun käytät ExpressRoute, ja Data Management Gateway avulla voit määrittää tietokoneen oppiminen ja tietolähteen väliset yhteydet. 

Löydät yksityiskohtaiset tiedot asennusvaatimukset, asennusohjeita ja vianmääritysvihjeitä on artikkelissa [paikallisesti ja pilvi Data Management Gateway ja tietojen siirtäminen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway), alkavat [Huomioitavaa Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway)-osassa.

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Tunkeutumisen tietoja paikallisen SQL Server-tietokannasta kyselyjä Azure koneen oppiminen


Tätä vaiheittaista määrittäminen Data Management Gatewayn Azure koneen Learning-työtilassa, määrittää sen ja Lue tietoja paikallisen SQL Server-tietokannasta.

> [AZURE.TIP] Ennen kuin aloitat, poista käytöstä selaimen ponnahdusikkunoiden `studio.azureml.net`. Jos käytät Google Chrome-selaimessa, lataa ja asenna jokin useita laajennukset osoitteessa Google Chrome WebStore [Napsauttamalla kerran App-laajennus](https://chrome.google.com/webstore/search/clickonce?_category=extensions).

### <a name="step-1-create-a-gateway"></a>Vaihe 1: Gatewayn luominen

Ensimmäiseksi on luotava ja määritettävä yhdyskäytävän käyttämään paikallista SQL-tietokantaan.

1. Kirjaudu [Azure koneen Learning Studio](https://studio.azureml.net/Home/) ja valitse työtila, jonka haluat työskennellä.

2. Valitse vasemman reunan **asetukset** -sivu ja valitse sitten yläreunassa **Tietojen YHDYSKÄYTÄVÄT** -välilehti.

3. Valitse näytön alareunassa **Uusi tietojen yhdyskäytävä** .

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. **Uusi tietoyhdyskäytävä** -valintaikkunassa **Yhdyskäytävän nimi** ja halutessasi lisätä **kuvaus**. Napsauta oikeassa alakulmassa Siirry seuraavaan vaiheeseen määrityksen nuolta.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. Lataa ja rekisteröi tietojen yhdyskäytävän-valintaikkunassa Kopioi Leikepöydälle YHDYSKÄYTÄVÄN rekisteröinti-näppäintä.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Jos olet ei ole vielä ladattu ja asennettu Microsoft Data Management Gateway, valitse **Lataa data management Gatewayn**. Tämä avaa kohtaa, johon voit valita tarvitsemasi, lataa se yhdyskäytäväversio ja asenna se Microsoft Download Centeristä. Löydät yksityiskohtaiset tiedot asennusvaatimukset, asennusohjeita ja vianmääritysvihjeitä alkuun osien [siirtäminen paikallisesti ja pilvi Data Management Gateway ja tietojen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)artikkelin.

7. Kun yhdyskäytävä on asennettu, Avaa Data Management Gatewayn määritysten hallinta ja **Rekisteröi yhdyskäytävä** -valintaikkuna tulee näkyviin. Liitä **Yhdyskäytävän rekisteröinti avaimen** , joka on kopioitu Leikepöydälle ja valitse **Rekisteröi**.

8. Jos sinulla on jo asennettu yhdyskäytävä, suorita Data Management Gatewayn määritysten hallinta, valitse **Muuta avain**, Liitä  **Yhdyskäytävän rekisteröinti avaimen** , joka on kopioitu Leikepöydälle ja valitse **OK**.

9. Kun asennus on valmis, näyttöön tulee **Rekisteröi yhdyskäytävä** -valintaikkunassa varten Microsoft Data Management Gatewayn määritysten hallinta. Liitä YHDYSKÄYTÄVÄN rekisteröinti AVAINTA, joka on kopioitu Leikepöydälle yllä ja valitse **Rekisteröi**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. Yhdyskäytävän määritys on valmis, kun **Aloitus** -välilehden toimi Microsoft Data Management Gatewayn määritysten hallinnassa on määritetty seuraavat arvot:

    - **Yhdyskäytävän nimi** ja **esiintymänimi** on määritetty yhdyskäytävän nimeä.

    - **Rekisteröinti** on asetettu **rekisteröity**.

    - **Tilana on **Aloitettu**.**

    - Alaosassa tilarivillä näkyy **yhteys Data Management Gateway pilvipalvelussa** sekä vihreä valintamerkki.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure koneen Learning Studio päivitetään myös, kun rekisteröinti onnistuu.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. **Lataa ja rekisteröidä yhdyskäytävän tiedot** -valintaikkunassa valitsemalla Viimeistele määritys valintamerkkiä. **Asetukset** -sivulla näkyy "Online" yhdyskäytävän tila. Löydät oikeanpuoleisessa ruudussa tila ja muita hyödyllisiä tietoja.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. Valitse Microsoft Data Management Gatewayn määritysten hallinta Siirry **varmenteen** -välilehti. Tässä välilehdessä määritetyn varmennetta käytetään salaus/salauksen purkaminen paikallisen tietovaraston, jotka määrität portaalissa tunnistetiedot. Tämä on oletusarvo-varmenne, joka on luotu. Microsoft suosittelee muuttaminen tämä oman varmenne, jota varmenteen hallintajärjestelmän varmuuskopioida. Valitse **Muuta** käyttämään oman allekirjoituksen.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (valinnainen) Jos haluat ottaa käyttöön, jotta yhdyskäytävän ongelmien vianmääritystä yksityiskohtainen kirjaus,-Microsoft Data Management Gatewayn määritysten hallinta **Diagnostiikka** -välilehteen ja valitse **vianmääritystä varten yksityiskohtainen kirjaus käyttöön** -vaihtoehto. Lokiin kirjaaminen löytyvät Windowsin Tapahtumienvalvonta **sovellukset- ja palvelulokit**  - &gt; **Data Management Gateway** -solmu. Voit myös Testaa yhteys paikalliseen tietolähteeseen käyttämällä yhdyskäytävän **Diagnostiikka** -välilehti.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

Tämä suorittaa yhdyskäytävän Azure koneen Learning prosessin määrittäminen.
Voit nyt käyttää paikalliset tiedot.

Voit luoda ja määrittää useita yhdyskäytäviä Studiossa kunkin työtilan. Esimerkiksi on ehkä yhdyskäytävän, johon haluat muodostaa testi tietolähteiden kehityksen aikana ja eri yhdyskäytävän tuotannon tietolähteiden. Azure koneen Learning lisää joustavuutta useita yhdyskäytäviä sen mukaan, yrityksen ympäristön määrittäminen. Tällä hetkellä et voi jakaa yhdyskäytävää välillä työtilojen ja vain yhden yhdyskäytävän voidaan asentaa samassa tietokoneessa. Katso Lisää huomioitavista yhdyskäytävän asennettaessa, on artikkelissa [paikallisesti ja pilvi Data Management Gateway ja tietojen siirtäminen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) [Data Management Gateway Huomioitavaa](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) .

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Vaihe 2: Käytä yhdyskäytävän lukemaan paikallisen tietolähteen tiedot

Yhdyskäytävän määritettyäsi **Tietojen tuominen** -moduulin voi lisätä koe, joka syöttää tiedot paikallisen SQL Server-tietokannasta.

1.  Koneen Learning Studiossa **kokeissa** -välilehti, valitse **+ Uusi** vasemmassa yläkulmassa ja valitse **Tyhjä kokeen** (tai valitse jokin seuraavista useita malli-kokeissa).

2.  Etsi ja vedä **Tietojen tuominen** -moduulin kokeen alusta.

3.  Piirtoalustan alapuolella, valitse **Tallenna nimellä** . Kirjoita "Azure koneen Learning paikallisen SQL Server opetusohjelma" kokeen nimen, valitse työtila ja valitse **OK** -valintaruudun valinta.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  **Tietojen tuominen** -moduulin napsauttamalla sitä ja valitse Valitse **Ominaisuudet** -ruudun oikealla puolella alusta "Paikallisen SQL-tietokantaan" **tietolähteen** avattavasta luettelosta.

5.  Valitse **tietojen yhdyskäytävän** asennettu ja rekisteröity. Voit asennuksen toiseen yhdyskäytävään valitsemalla "(Lisää uusi... yhdyskäytävän tiedot)".

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Kirjoita SQL- **Tietokantapalvelimen nimi** ja **tietokannan nimi**, ja haluat suorittaa SQL- **kysely** .

7.  Valitse **Enter arvot** , valitse **käyttäjänimi ja salasana** ja anna tietokannan tunnistetiedot. Voit käyttää Windows-todennusta tai SQL Server-todennus sen mukaan, miten paikallisen SQL-palvelin on määritetty.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Viesti "arvot pakollinen" Siirry "arvojen määrittäminen" vihreä valintamerkki. Haluat syöttää tunnistetietoja kerran, ellet tietokannan tietoja tai salasana muuttuu. Azure koneen Learning käyttää antamasi asentaessasi pilveen tunnistetietojen salaamisessa yhdyskäytävän varmenne. Azure koskaan tallentaa paikallisen tunnistetiedot ilman salausta.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  Valitse Suorita kokeen **Suorita** .

Kun kokeen loppuu käynnissä voit voit esittää tiedot on tuotu tietokannasta valitsemalla **Tuontitiedot** -moduulin tulostusportin ja **Visualisoi**valitsemalla.

Kun lopetat kehittäminen oman koe, voit ottaa käyttöön ja operationalize mallin. Erän suorittamisen Service-Esityspalvelua käyttämällä **Tuontitiedot** -moduulissa määritetty paikallisen SQL Server-tietokannan tietojen lukeminen ja käyttää näkyvissä pistemäärä. Voit pyytää vastauksen palvelun näkyvissä pistemäärä paikalliset tiedot, kun Microsoft suosittelee käyttämistä [Excel-apuohjelma](machine-learning-excel-add-in-for-web-services.md) . Tällä hetkellä kirjoittaminen paikallisen SQL Serveriä – **Tietojen vieminen** tietokantaan ei tueta tutkimuksiin tai julkaistut WWW-palvelut.

