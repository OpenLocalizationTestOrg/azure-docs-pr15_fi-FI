<properties
   pageTitle="Hakujen tallentaminen ja kiinnitä tietojen varat | Microsoft Azure"
   description="Toimintaohjeet artikkelissa korostaminen Azure tietoluettelon ominaisuuksista tallennuksen tietolähteet ja tietojen varat tulevaa käyttöä varten."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/10/2016"
   ms.author="maroche"/>

# <a name="how-to-save-searches-and-pin-data-assets"></a>Hakujen tallentaminen ja tietojen varat kiinnittäminen

## <a name="introduction"></a>Johdanto

Microsoft Azure tietoluettelon sisältää ominaisuuksia, joilla tietojen lähteen etsimiseen. Käyttäjät voivat hakea nopeasti ja suodattaa luettelon löytää tietolähteet ja ymmärtää niiden käyttötarkoitus Etsi oikeita tietoja työn kätevästi on helpompaa.

Mutta Entä, kun käyttäjät tarvitsevat säännöllisesti samat tiedot käyttöä varten? Mitä tietoja, kun käyttäjät säännöllisesti osallistuja heidän tietoonsa saman tietolähteisiin luettelossa? Näissä tilanteissa tarvitse myöntää toistuvasti saman haut voi olla tehotonta – tämä on tallennettu haku ja kiinnitetyt tietojen resurssien avulla.

## <a name="saved-searches"></a>Tallennetut haut

Tallennetun haun Azure-tietoluetteloon on Uudelleenkäytettävän, käyttäjäkohtainen haun määritys. Kun käyttäjä on määritetty haun – mukaan lukien hakusanat, tunnisteet ja muut suodattimet, hän tallentaa sen myöhempää käyttöä varten. Tallennetun haun määritelmän sitten voidaan käyttää myöhemmin, voit palauttaa kaikki tiedot resurssit, jotka vastaavat sen hakuehdot.

### <a name="creating-a-saved-search"></a>Tallennetun haun luominen

Luo tallennetun haun, kirjoita hakuehdot uudelleen. Valitse hakuruutuun "Nykyinen" Azure tietoluettelon portaalissa "Tallenna"-linkki.

 ![Valitse '' Tallenna Tallenna nykyinen hakuasetukset](./media/data-catalog-how-to-save-pin/01-save-option.png)

Kirjoita pyydettäessä tallennetun haun nimi. Valitse, joka on merkityksellinen ja tietojen kohteita, jotka haun palauttamat kuvaava nimi.

 ![Anna sille nimi tallennettu haku](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>Hallinta tallennetut haut

Kun käyttäjä on tallennettu yksi tai useampi, "Tallennettujen hakujen"-vaihtoehto näkyy "nykyinen haku-ruudun alapuolella Azure tietoluettelon-portaalissa. Kun laajennettu, luettelo kaikista tallentaa haut näkyvät.

 ![Tallennetun hakujen luettelo](./media/data-catalog-how-to-save-pin/03-list.png)

Tallennetun haun valitsemalla luettelosta aiheuttaa voidaan suorittaa haun.

Valitsemalla avattavasta valikosta Ilmoita hallinta vaihtoehdoista:

 ![Tallennettujen hakujen hallinta-asetukset](./media/data-catalog-how-to-save-pin/04-managing.png)

Valitsemalla "Nimeä" kehote käyttäjän on syötettävä tallennetun haun uusi nimi. Etsi-määritys ei voi muuttaa.

Valitsemalla "Poista" pyytää vahvistamaan käyttäjä ja valitse Poista tallennetun haun käyttäjän luettelosta.

"Tallenna nimellä oletus" valitsemalla Merkitse valitun haun tallennetaan oletusarvoisesti Etsi käyttäjä. Jos käyttäjä etsii "tyhjä" Azure tietoluettelon kotisivulla, käyttäjän oletusarvoinen haku suoritetaan. Lisäksi haun merkitty oletusarvon näkyvät tallennetun Etsi-luettelon yläreunassa.

### <a name="organizational-saved-searches"></a>Organisaation tallennetut haut

Jokaiselle käyttäjälle tallentaa oman käyttöön etsii. Tietoluettelon järjestelmänvalvojat voit myös tallentaa haut kaikille organisaation käyttäjille. Kun tallennat haun, järjestelmänvalvojat esitetään sähköpostit jakaa tallennetun haun yrityksen sisällä. Jos tämä asetus on valittuna, tallennetun haun sisältyvät käytettävissä hakuluettelosta kaikille käyttäjille.

 ![Organisaation tallennetut haut](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)


## <a name="pinned-data-assets"></a>Kiinnitetty tietojen resurssit

Voit tallentaa sekä käyttää haun määritelmät; tallennetut haut käyttäjät haut palauttamien tietojen varat voivat muuttua ajan kuluessa muutoksesta luettelon sisällön. Tietoja resurssien kiinnittäminen avulla käyttäjät voivat erikseen tunnistaa tietyt tiedot varat helpottaa niiden löytämistä Accessiin tarvitsematta käyttämään haun.

Tietoja kohteiden kiinnittäminen on yksinkertaista – käyttäjien valita, tiedot annetaan lisääminen niiden kiinnitetty luetteloon "PIN-tunnuksen"-kuvaketta. Tämä kuvake näkyy vierekkäin-näkymässä ja vasemmanpuoleisimmasta sarakkeesta luettelonäkymässä Azure tietoluettelon portaalissa resurssi-ruudun oikeassa yläkulmassa.

![Tietoja kohteiden kiinnittäminen](./media/data-catalog-how-to-save-pin/05-pinning.png)

Sijoituksen unpinning on tasaisesti yksinkertaista – käyttäjien valita "PIN-tunnuksen"-kuvaketta uudelleen, jos haluat ottaa valitun kohteen-asetusta.

![Unpinning tietojen resurssi](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>"Omat varat"
Azure tietoluettelon portaalin aloitussivulla sisältää, joka näyttää nykyisen käyttäjän halutut varat "Omat varat"-osassa. Tässä osassa on sekä kiinnitetty varat ja tallennetaan Haut.

!["Omat varat" aloitussivulla](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Yhteenveto
Azure tietoluettelon sisältää ominaisuuksia, jotka helpottavat käyttäjien löydät tarvitsemiaan tietolähteitä, jotta ne käyttävät riittävästi aikaa esittää tiedot ja käyttämiseen enemmän aikaa. Tallennettuja hakuja ja kiinnitetyt varat muodosta core seuraavia ominaisuuksia, jotta käyttäjät voivat helposti havaita, johon ne toimivat toistuvasti tietolähteiden tiedot.
