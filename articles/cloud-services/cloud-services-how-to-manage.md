<properties 
    pageTitle="Yleiset cloud service management toiminnot (perinteinen) | Microsoft Azure" 
    description="Opi hallitsemaan pilvipalveluihin Azure perinteinen-portaalissa." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2016"
    ms.author="adegeo"/>





# <a name="how-to-manage-cloud-services"></a>Voit hallita pilvipalveluihin

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-manage-portal.md)
- [Azure perinteinen portal](cloud-services-how-to-manage.md)

Azure perinteinen portaalin **Cloud Services** -alueella voi päivittää palvelun rooli tai käyttöönoton, edistää vaiheistettu käyttöönoton tuotannon, linkki resurssien pilvipalvelussa sijaitsevaan niin, että voit tarkastella resurssin riippuvuudet ja skaalata resurssit yhteen ja poistaa pilvipalveluun tai käyttöönoton.


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Toimintaohje: Päivitä cloud roolin tai käyttöönotto

Jos haluat päivittää sovelluksen koodi cloud-palvelusta, käytä Raporttinäkymät-ikkunan, **Pilvipalveluihin** sivun tai sivun **tapaukset** **Päivitä** . Voit päivittää rooliin tai kaikki roolit. Sinun on lataaminen uuteen palvelupakettiin ja palvelun määritystiedostoa.

1. Valitse [Azure perinteinen portal](https://manage.windowsazure.com/)-Raporttinäkymät-ikkunan, **Pilvipalveluihin** sivun tai sivun **tapaukset** valitsemalla **Päivitä**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. **Käyttöönoton otsikko**Kirjoita nimi, joka yksilöi käyttöönoton (esimerkiksi mycloudservice4). Löydät käyttöönotto **Pika-aloitusopas** -kohdassa selite koontinäytössä.

3. **Paketin** **Selaa** Lataa käyttämällä service-pakettitiedosto (.cspkg).

4. **Määritysten** **Selaa** Lataa käyttämällä palvelun määritystiedosto (.cscfg).

5. **Rooli**Valitse **kaikki** , jos haluat päivittää kaikki roolit pilvipalvelussa. Voit suorittaa rooliin päivitys valitsemalla päivitettävän rooli. Vaikka voit valita tietyn rooli, jotta voit päivittää, palvelun määritystiedostossa päivitykset käytetään rooleista.

6. Jos päivitys muutoksia roolit määrä tai minkä tahansa rooli kokoa, valitse **Salli Päivitä rooli koot tai määrä roolien muuttuessa** -valintaruutu, jos käyttöön päivitys Jatka. 

    Huomioon, että jos haluat muuttaa koon rooli (rooli esiintymää isännöivän virtual-tietokoneen kokoa) tai roolit määrä, roolin jokaiselle esiintymälle (virtual machine) on oltava uudelleen kuvalliset ja paikalliset tiedot menetetään.

7. Jos palvelun mitään rooleja on vain yksi rooli esiintymä, valitse **päivittää vaikka vähintään yksi rooli sisältävät yhden esiintymän valintaruudun** Jatka päivityksen käyttöön. 

    Azure takaa vain 99.95 prosenttia palvelun saatavuus cloud päivittämisessä jos kullakin roolilla on vähintään kaksi roolin esiintymät (näennäiskoneiden). Joka mahdollistaa yhden virtual machine käsitellä asiakkaan pyyntöjä, kun toinen päivitetään.

8. Valitse **OK** (valintamerkki) Aloita päivitys palvelu.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Toimintaohje: Vaihda ominaisuuksissa edistää tuotannon vaiheistettu käyttöönotto

Ylennä pilvipalvelussa tuotannon väliaikaisen käyttöönoton **Vaihda** avulla. Kun päätät ottaa käyttöön uuden version pilvipalveluun, voit vaiheen ja testaa uudessa versiossa cloud-palvelun väliaikaisen ympäristösi asiakkaiden on käyttäessä nykyinen versio tuotannon. Kun olet valmis edistää uusien tuotannon, voit käyttää **Vaihda** vaihtaa URL-osoitteet, jonka kaksi ominaisuuksissa on osoitettu. 

Voit muuttaa ominaisuuksissa **Cloud Services** -sivulta tai koontinäytön.

1. Valitse [Azure perinteinen portal](https://manage.windowsazure.com/)- **Pilvipalveluihin**.

2. Valitse pilvipalveluihin luettelo napsauttamalla sitä pilvipalvelussa.

3. Valitse **Vaihda**.

    Seuraavat vahvistusviesti tulee näkyviin.

    ![Cloud Services vaihtaminen](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Kun olet varmistanut käyttöönoton tiedot, valitse **Kyllä** vaihdettava käyttöönotoissa.

    Vaihda käyttöönoton kauaa koska riittää, joka muuttuu virtual IP-osoitteet (VIPs) käyttöönotoissa.

    Tallenna Laske kustannukset, voit poistaa käyttöönoton väliaikaisen-ympäristössä, kun olet varma, että uusi tuotannon käyttöönoton toimii odotetulla tavalla.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Toimintaohje: Linkitä resurssin pilvipalveluun

Näyttää oman cloud palvelun riippuvuudet muita resursseja, voit linkittää erillisen Azure SQL-tietokanta tai tallennustilan tiliä pilvipalvelussa. Voit linkittää linkityksen resurssien **Linkitetyn resurssit** -sivulle ja seuraa niiden käyttö cloud palvelun koontinäytössä. Jos linkitetty tallennustilan tilin seuranta on käytössä, voit valvoa pyyntöjen kokonaismäärä cloud palvelun koontinäytössä.

Linkitä uuteen tai aiemmin luotuun SQL-tietokannan esiintymä tai tallennustilan tilin pilvipalveluun, että **linkin** avulla. Voit sitten skaalata tietokannan sekä cloud-palvelun rooli, joka käyttää sitä **asteikko** -sivulla. (Tallennustilan tilin Skaalaa automaattisesti kuin käyttö kasvaa.) Katso lisätietoja, [miten pilvipalvelussa ja linkitetyt resurssit](cloud-services-how-to-scale.md). 

Voit myös seurata, hallita ja skaalata tietokannan Azure perinteinen portaalin **Databases** -solmu. 

"Linkittäminen" tässä resurssin ei yhdistä sovelluksen resurssi. Jos luot uuden tietokannan **linkin**avulla, tarvitset lisää yhteyden merkkijonot sovelluksen-koodin ja päivität pilvipalvelussa. Tarvitset myös lisätä yhteyden merkkijonoja, jos sovellus käyttää resursseja linkitetyn tallennustilan tilillä.

Seuraavassa kerrotaan, miten voit linkittää uuteen SQL-tietokanta-esiintymän, otettu käyttöön uusi SQL-tietokanta-palvelimessa, pilvipalveluun.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Jos haluat linkittää SQL-tietokanta-esiintymän pilvipalveluun

1. Valitse [Azure perinteinen portal](http://manage.windowsazure.com/)- **Pilvipalveluihin**. Valitse Avaa koontinäyttö pilvipalvelussa nimi.

2. Valitse **linkitetty resurssit**.

    **Linkitetyn resurssit** -sivu avautuu.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Valitse joko **resurssi** tai **linkkiä**.

    **Linkitettävässä** ohjattu toiminto käynnistyy.

    ![Linkki Sivu1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Valitse **Luo uusi resurssi** tai **linkki aiemman resurssin**.

5. Voit linkittää resurssin lajin valitseminen Valitse [Azure perinteinen portal](http://manage.windowsazure.com/)- **SQL-tietokantaan**. (Perinteinen esikatselu Azure-portaalissa ei tue linkittäminen tallennustilan tilin pilvipalveluun.)

6. Viimeistele määritys tietokannan, ohjeiden ohjeen **SQL-tietokantoja** alueen Azure perinteinen portaalin.

    Voit seurata edistymistä viestialueelle linkitystoiminnon.

    ![Linkki edistyminen](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    Kun linkittämisestä on valmis, voit valvoa cloud palvelun Raporttinäkymät-ikkunan linkitetyn resurssin tila. Tietoja skaalaus linkitetyn SQL-tietokannan artikkelissa [miten pilvipalvelussa ja linkitetyt resurssit](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Jos haluat poistaa linkitetyn resurssin linkityksen

1. Valitse [Azure perinteinen portal](http://manage.windowsazure.com/)- **Pilvipalveluihin**. Valitse Avaa koontinäyttö pilvipalvelussa nimi.

2. Valitse **Linkitetty resurssit**ja valitse sitten resurssi.

3. Valitse **Poista**. Valitse **Kyllä** vahvistusviesti tulee näkyviin.

    Linkityksen SQL-tietokanta ei ole vaikutusta tietokannan tai sovelluksen yhteydet tietokantaan. Voit hallita silti tietokannan Azure perinteinen portaalin **SQL-tietokantoja** -alueella.



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Toimintaohje: Poista ominaisuuksissa ja pilvipalveluun

Ennen kuin voit poistaa pilvipalveluun, sinun on poistettava kunkin luodun käyttöönotto.

Tallenna Laske kustannukset, voit poistaa väliaikaisen käyttöönoton sen jälkeen, kun olet varmistanut, että tuotannon käyttöönoton toimii odotetulla tavalla. Olet laskutettu Laske kustannuksiin roolin esiintymät, vaikka pilvipalveluun ei ole käynnissä.

Seuraavien ohjeiden avulla voit poistaa käyttöönoton tai pilvipalvelussa sijaitsevaan. 

1. Valitse [Azure perinteinen portal](http://manage.windowsazure.com/)- **Pilvipalveluihin**.

2. Valitse pilvipalvelussa ja valitse sitten **Poista**. (Valitse koontinäyttö avaamatta pilvipalveluun, napsauttamalla mitä tahansa cloud palvelun tapahtuman nimi paitsi.)

    Jos sinulla on väliaikainen tai tuotannon käyttöönoton, näet valikon vaihtoehtojen samalla tavalla kuin yksi ikkunan alareunassa. Ennen kuin voit poistaa pilvipalvelussa, sinun on poistettava kaikki olemassa olevissa käyttöympäristöissä.

    ![Poista valikko](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. Voit poistaa käyttöönoton valitsemalla **Poista tuotannon käyttöönoton** tai **Poista väliaikaisen käyttöönotto**. Kun vahvistusviesti tulee näkyviin, valitse **Kyllä**. 

4. Jos haluat poistaa pilvipalvelussa, toistamalla vaihetta 3, tarvittaessa, jos haluat poistaa koko käyttöönotto.

5. Voit poistaa pilvipalvelussa valitsemalla **Poista pilvipalvelussa**. Kun vahvistusviesti tulee näkyviin, valitse **Kyllä**.

> [AZURE.NOTE]
> Jos yksityiskohtainen seuranta on määritetty pilvipalvelussa sijaitsevaan, Azure Poista tiedot tallennustilan tililtä kun poistat pilvipalvelussa. Sinun on poistettava tiedot manuaalisesti. Lisätietoja löydät arvot taulukot on "Toimintaohje: Access yksityiskohtainen ulkopuolella Azure perinteinen portal-tietojen valvominen" kohdan [näytön pilvipalveluihin](cloud-services-how-to-monitor.md).

## <a name="next-steps"></a>Seuraavat vaiheet

 * [Yleisten määritysten cloud-palvelun](cloud-services-how-to-configure.md).
* Lisätietoja käyttöönottamisesta [pilvipalveluun](cloud-services-how-to-create-deploy.md).
* Määritä [mukautettua toimialuenimeä](cloud-services-custom-domain-name.md).
* Määritä [ssl-varmenteita](cloud-services-configure-ssl-certificate.md).
